---
tags: [code-analysis]
product: Codex
pillar: context
status: complete
created: 2026-04-12
key_files:
  - code/codex/codex-rs/core/src/compact.rs
  - code/codex/codex-rs/core/src/environment_context.rs
  - code/codex/codex-rs/core/src/session_prefix.rs
  - code/codex/codex-rs/core/templates/compact/prompt.md
---

# Codex — Context Management

## Purpose

Codex compacts context by calling the model with a "checkpoint handoff" prompt that generates a summary for the next LLM turn. The system distinguishes mid-turn compaction (which reinjects initial context into the replacement history) from pre-turn/manual compaction (which defers reinjection to the next regular turn). Environment context is serialized as XML and diffed between turns to minimize injected bytes.

## Architecture

```
Context grows during turn
        │
        ▼
Context window exceeded? ──► ContextWindowExceeded error from API
        │
        ▼
run_inline_auto_compact_task()           ← Mid-turn (auto)
  or run_compact_task()                  ← User-triggered (manual)
        │
        ▼
run_compact_task_inner_impl()
  │
  ├─ Build prompt: SUMMARIZATION_PROMPT + personality + developer instructions
  ├─ Call model via drain_to_completed()
  │   └─ On ContextWindowExceeded: trim oldest history, retry
  │
  ├─ Extract last assistant message as summary
  ├─ Collect user messages (newest first, within 20K token budget)
  │   └─ Truncate last message if needed
  │
  ├─ Build compacted history:
  │   ├─ BeforeLastUserMessage: reinject initial context ← mid-turn
  │   └─ DoNotInject: clear reference, next turn reinjects ← manual
  │
  └─ Replace session history with compacted version
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `src/compact.rs` | Compaction logic — summarize, build replacement history, retry on overflow | 591 |
| `src/environment_context.rs` | Environment state gathering and XML serialization | 219 |
| `src/session_prefix.rs` | Session-start context formatting (sub-agent notifications) | 29 |
| `templates/compact/prompt.md` | Compaction prompt: "Create a handoff summary for another LLM" | 9 |
| `templates/compact/summary_prefix.md` | Prefix prepended to summaries (currently empty placeholder) | 0 |

## Implementation Walkthrough

### 1. The Compaction Prompt

Loaded at compile time via `include_str!()`:

```
You are performing a CONTEXT CHECKPOINT COMPACTION. Create a handoff
summary for another LLM that will resume the task.

Include:
- Current progress and key decisions made
- Important context, constraints, or user preferences
- What remains to be done (clear next steps)
- Any critical data, examples, or references needed to continue

Be concise, structured, and focused on helping the next LLM
seamlessly continue the work.
```

This is notably **simpler** than Claude Code's 9-section compaction prompt — Codex trusts the model to decide what's important rather than prescribing sections.

### 2. Compaction Trigger and Entry Points

**Two entry points:**

| Function | Trigger | InitialContextInjection |
|----------|---------|------------------------|
| `run_inline_auto_compact_task()` | Mid-turn, context exceeded | `BeforeLastUserMessage` |
| `run_compact_task()` | Manual `Op::Compact` from user | `DoNotInject` |

Both create a synthetic user input from the compaction prompt and call `run_compact_task_inner()`.

**`should_use_remote_compact_task(provider)`**: returns `true` only for OpenAI providers (delegates to backend service). Other providers use local inline compaction.

### 3. The Compaction Engine (`run_compact_task_inner_impl`)

**Step 1: Record start** — emit `ContextCompactionItem` event with initial state.

**Step 2: Build prompt** — combine `SUMMARIZATION_PROMPT` with personality and developer instructions into a `Prompt` struct.

**Step 3: Call model with retry loop**:
- Stream model response via `drain_to_completed()`
- On `ContextWindowExceeded` error: trim the oldest history items and retry
- Preserves cache where possible (trim from head, not tail)
- Max retries from provider config

**Step 4: Build compacted history**:
- Collect user messages in reverse chronological order
- Use `approx_token_count()` to estimate each message
- Select messages that fit within `COMPACT_USER_MESSAGE_MAX_TOKENS` (20,000)
- If the final message exceeds remaining budget, truncate it with `truncate_text()`
- Reverse to restore chronological order

**Step 5: Initial context injection**:

```rust
enum InitialContextInjection {
    BeforeLastUserMessage,  // Mid-turn: reinject before last user message
    DoNotInject,            // Manual: clear reference; next turn handles it
}
```

- `BeforeLastUserMessage`: The model is trained to see the compaction summary as the last item. Initial context (system prompt preamble, AGENTS.md) is injected just above the last real user message so the model sees: `[summary] [initial context] [last user message]`.
- `DoNotInject`: Clears `reference_context_item`. The next regular turn will fully reinject initial context after detecting the cleared reference.

**Step 6: Finalize** — replace full session history with compacted version. Emit warning: "Long threads and multiple compactions can cause less accuracy. Start a new thread when possible."

### 4. Environment Context (`environment_context.rs`)

The `EnvironmentContext` struct captures system state for injection into the model's context:

```rust
struct EnvironmentContext {
    cwd: Option<PathBuf>,           // Working directory
    shell: Shell,                    // zsh, bash, powershell, etc.
    current_date: Option<String>,    // YYYY-MM-DD
    timezone: Option<String>,        // IANA timezone
    network: Option<NetworkContext>,  // Allowed/denied domains
    subagents: Option<String>,       // Active sub-agent info
}
```

**Serialized as XML** via `serialize_to_xml()`:
```xml
<environment_context>
  <cwd>/Users/dev/project</cwd>
  <shell>zsh</shell>
  <current_date>2026-04-12</current_date>
  <timezone>America/Los_Angeles</timezone>
  <network enabled="true">
    <allowed>github.com</allowed>
    <denied>malicious.com</denied>
  </network>
</environment_context>
```

**Diff-based injection**: `diff_from_turn_context_item()` compares before/after turn contexts and only includes changed fields. This minimizes injected bytes — if nothing changed between turns, no environment update is needed. Shell is excluded from diffs (immutable per session).

### 5. Session Prefix (`session_prefix.rs`)

Two formatting helpers for session-start context:

**`format_subagent_notification_message()`**: wraps JSON in XML tags for sub-agent status updates:
```xml
<SUBAGENT_NOTIFICATION_FRAGMENT>
{"agent_path": "agent-1", "status": "completed"}
</SUBAGENT_NOTIFICATION_FRAGMENT>
```

**`format_subagent_context_line()`**: formats a sub-agent inventory line:
```
- agent_reference: agent_nickname
```

### 6. Token Counting

Codex uses `approx_token_count()` from the `codex_utils_output_truncation` crate — a character-based approximation (not a full tokenizer). Combined with `truncate_text()` for enforcing limits:

```rust
const COMPACT_USER_MESSAGE_MAX_TOKENS: usize = 20_000;

// In build_compacted_history_with_limit():
let tokens = approx_token_count(&message);
if accumulated + tokens > COMPACT_USER_MESSAGE_MAX_TOKENS {
    let remaining = COMPACT_USER_MESSAGE_MAX_TOKENS - accumulated;
    truncate_text(&message, TruncationPolicy::Tokens(remaining));
}
```

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| `COMPACT_USER_MESSAGE_MAX_TOKENS` | Hard cap (20K) on retained user messages post-compact |
| Provider config `max_retries` | How many times to retry on ContextWindowExceeded |
| `should_use_remote_compact_task()` | OpenAI → remote compaction; others → local |
| `developer_instructions` | Custom instructions injected into compaction prompt |
| `personality` | Personality template applied during compaction |
| Network sandbox config | Allowed/denied domains in environment context |

## Design Decisions

- **9-line compaction prompt vs Claude Code's multi-page template** — Codex's prompt is minimal: "create a handoff summary." This trusts the model's judgment about what to include, while Claude Code prescribes 9 specific sections. The tradeoff is flexibility vs consistency — Codex summaries vary more but aren't constrained to a fixed format.
- **InitialContextInjection handles mid-turn vs pre-turn gracefully** — Mid-turn compaction must reinject initial context because the model expects it in the replacement history. Manual compaction can defer this to the next turn, keeping the compacted history cleaner.
- **XML environment context with diffing** — XML is verbose but unambiguous for the model to parse. Diff-based injection means most turns add zero environment bytes. This is a cache-friendly design — the environment section stays the same until something actually changes.
- **Compile-time prompt embedding** — The compaction prompt, like all Codex prompts, is embedded via `include_str!()`. No runtime file reads, no missing-file errors. See [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md) for the full pattern.
- **Character-based token estimation** — Rather than shipping a tokenizer, Codex uses `approx_token_count()` — a rough character-to-token ratio. Good enough for budget decisions; exact counts come from the API response.

## See Also

- [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md) — How the compaction prompt fits into the template hierarchy
- [Codex — Agent Loop](references/code-analysis/loops/codex-agent-loop.md) — Where compaction tasks are dispatched within the event loop
- [04 - Context Management](references/dimensions/04-context-management.md) — Design rationale from interviews (AGENTS.md, context window philosophy)

---
tags: [code-analysis]
product: Codex
pillar: prompts
status: complete
created: 2026-04-12
key_files:
  - code/codex/codex-rs/core/gpt_5_codex_prompt.md
  - code/codex/codex-rs/core/gpt_5_1_prompt.md
  - code/codex/codex-rs/core/gpt_5_2_prompt.md
  - code/codex/codex-rs/core/review_prompt.md
  - code/codex/codex-rs/core/templates/realtime/backend_prompt.md
  - code/codex/codex-rs/core/templates/collab/experimental_prompt.md
  - code/codex/codex-rs/core/templates/personalities/
  - code/codex/codex-rs/core/templates/agents/orchestrator.md
  - code/codex/codex-rs/core/templates/memories/
  - code/codex/codex-rs/core/src/realtime_prompt.rs
  - code/codex/codex-rs/core/src/instructions/mod.rs
---

# Codex — Prompt System

## Purpose

Codex uses **markdown template files** compiled into the Rust binary at build time via `include_str!()`. Prompts are versioned per model generation (GPT-5, 5.1, 5.2) and supplemented by personality templates, specialized prompts (review, collab, realtime), and a two-phase memory consolidation system. Runtime customization uses `{{ }}` template variables substituted in Rust.

## Architecture

```
Build Time (Rust)                          Runtime
─────────────────                          ───────
gpt_5_codex_prompt.md ──┐
gpt_5_1_prompt.md ──────┤                 Model selection
gpt_5_2_prompt.md ──────┼─ include_str!() ──► picks prompt variant
review_prompt.md ───────┤                       │
compact/prompt.md ──────┘                       ▼
                                           prepare_realtime_backend_prompt()
templates/realtime/backend_prompt.md ──────► {{ user_first_name }} substitution
templates/personalities/*.md ──────────────► {{ personality }} injection
templates/model_instructions/*_template.md ► wraps personality + task behavior
templates/agents/orchestrator.md ──────────► multi-agent coordination
templates/memories/stage_one_system.md ────► Phase 1: rollout → raw memory
templates/memories/consolidation.md ───────► Phase 2: raw → consolidated
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `gpt_5_codex_prompt.md` | Base system prompt — identity, editing, git safety, planning | 68 |
| `gpt_5_1_prompt.md` | GPT-5.1 variant — adds AGENTS.md spec, autonomy, responsiveness, planning depth | 331 |
| `gpt_5_2_prompt.md` | GPT-5.2 variant — adds parallelization focus, condensed responsiveness | 298 |
| `review_prompt.md` | Code review — bug determination, JSON output format, confidence scoring | 87 |
| `templates/realtime/backend_prompt.md` | Voice-friendly real-time assistant with `{{ user_first_name }}` | 50 |
| `templates/collab/experimental_prompt.md` | Collaborative multi-agent — "treat user as equal co-builder" | 15 |
| `templates/personalities/gpt-5.2-codex_pragmatic.md` | Pragmatic personality: clarity, rigor, efficiency | 19 |
| `templates/personalities/gpt-5.2-codex_friendly.md` | Friendly personality: empathy, collaboration, "we" language | 20 |
| `templates/model_instructions/gpt-5.2-codex_instructions_template.md` | Instruction wrapper with `{{ personality }}` slot | 81 |
| `templates/agents/orchestrator.md` | Coordinator mode: sub-agent spawning, parallel orchestration | 43 |
| `templates/memories/stage_one_system.md` | Phase 1 memory agent: rollout → structured raw memory | 190+ |
| `templates/memories/consolidation.md` | Phase 2 memory agent: raw → consolidated folder structure | 50+ |
| `src/realtime_prompt.rs` | Rust runtime: prompt selection + template variable substitution | 82 |
| `src/instructions/mod.rs` | Re-exports `codex_instructions::UserInstructions` | 1 |

## Implementation Walkthrough

### 1. Model-Versioned Prompts

Codex maintains separate prompt files per model generation. Each is a complete, standalone system prompt compiled into the binary.

**`gpt_5_codex_prompt.md`** (base, 68 lines) — core identity and constraints:
- Identity: "You are Codex, based on GPT-5"
- Prefers `rg` over grep for speed
- ASCII-only output by default
- `apply_patch` for single-file edits
- Git safety: NEVER `git reset --hard` without explicit request
- Plan tool: skip for straightforward tasks (~25% easiest)
- Code review mindset on "review" requests
- Frontend: avoid "AI slop" — intentional, bold interfaces

**`gpt_5_1_prompt.md`** (331 lines) — major expansion:
- Identity: "You are GPT-5.1 running in the Codex CLI"
- **AGENTS.md specification**: repo files that guide behavior, deeper nesting takes precedence
- **Autonomy**: persist until task fully handled; assume code changes unless analysis requested
- **Responsiveness**: short updates (1-2 sentences) when meaningful; heads-down notes for longer work
- **Planning**: 8-12 high-quality examples; status tracking (pending → in_progress → completed)
- **Validation**: start specific, then broader tests; don't add tests to no-test codebases
- **Ambition vs precision**: ambitious for new tasks, surgical in existing codebases

**`gpt_5_2_prompt.md`** (298 lines) — refinements:
- Identity: "You are GPT-5.2 running in the Codex CLI"
- **Parallelization focus**: "Parallelize tool calls whenever possible — especially file reads. Use `multi_tool_use.parallel`"
- Condensed responsiveness section
- Frontend: "preserve the established patterns" in existing design systems

Variant files (`gpt-5.1-codex-max_prompt.md`, `gpt-5.2-codex_prompt.md`) are identical 68-line copies of the base — configuration-specific aliases.

### 2. Compile-Time Embedding

All prompt files are embedded via Rust's `include_str!()` macro:

```rust
pub const SUMMARIZATION_PROMPT: &str = include_str!("../templates/compact/prompt.md");
pub const SUMMARY_PREFIX: &str = include_str!("../templates/compact/summary_prefix.md");
```

This means prompts are **baked into the binary** — no filesystem reads at runtime, no risk of missing files, and inherently cache-stable (prompt bytes never change between turns within a build). The tradeoff: prompt changes require recompilation.

### 3. Runtime Template Substitution

The only runtime prompt assembly happens in `src/realtime_prompt.rs` (82 lines):

```rust
const BACKEND_PROMPT: &str = include_str!("../templates/realtime/backend_prompt.md");

pub fn prepare_realtime_backend_prompt(
    prompt: Option<Option<String>>,
    config_prompt: Option<String>,
) -> String
```

**Priority**: `config_prompt` > request-level prompt > default template.

**Variable substitution**: `{{ user_first_name }}` is replaced with the real user's first name via:
```rust
fn current_user_first_name() -> String {
    [whoami::realname(), whoami::username()]
        .into_iter()
        .filter_map(|name| name.split_whitespace().next().map(str::to_string))
        .find(|name| !name.is_empty())
        .unwrap_or_else(|| "there".to_string())  // fallback
}
```

### 4. Personality System

Codex supports runtime personality injection via `{{ personality }}` in the model instructions template:

**Pragmatic** (`gpt-5.2-codex_pragmatic.md`):
- Core values: clarity, pragmatism, rigor
- Tone: concise, respectful, efficient
- "Acknowledge great work briefly; avoid cheerleading or artificial reassurance"

**Friendly** (`gpt-5.2-codex_friendly.md`):
- Core values: empathy, collaboration, ownership
- Tone: warm, encouraging, conversational
- "Use 'we' and 'let's'; affirm progress; replace judgment with curiosity"

The **model instructions template** (`gpt-5.2-codex_instructions_template.md`, 81 lines) wraps the personality with task-specific behavior: terminal I/O conventions, file reference formats, editing constraints, git safety, plan tool usage, and frontend design guidance.

### 5. Specialized Prompts

**Review prompt** (`review_prompt.md`, 87 lines):
- Bug determination criteria: must meaningfully impact accuracy, performance, security, or maintainability
- 8 guidelines for flagging (discrete/actionable, doesn't demand absent rigor, author would fix it)
- Output: strict JSON with `findings` array (title, body, confidence_score, priority 0-3, code_location)
- Overall verdict: "patch is correct" or "patch is incorrect"

**Collab prompt** (`collab/experimental_prompt.md`, 15 lines):
- "Treat user as equal co-builder; preserve intent and coding style"
- "Prefer multiple sub-agents to parallelize; wait for them before yielding"

**Realtime prompt** (`realtime/backend_prompt.md`, 50 lines):
- Voice-friendly: single short acknowledgement sentence to start every response
- "I'm asking the agent to check the repo..." for delegation
- Prefer "Do X, then run Y" sequences

### 6. Multi-Agent Coordination

**Orchestrator prompt** (`agents/orchestrator.md`, 43 lines):
- "Your role becomes orchestration, not actual work"
- Spawn sub-agents per step of multi-step plans
- "Wait for sub-agents before yielding" unless user asks directly
- When user in flow: stay succinct; when blocked: get animated with hypotheses

### 7. Two-Phase Memory System

**Phase 1** (`memories/stage_one_system.md`, 190+ lines) — Memory Writing Agent:
- Converts raw rollouts (execution traces) into structured memories
- NO-OP gate: "Will a future agent plausibly act better?" — if not, return empty
- High-signal buckets: stable preferences, procedural knowledge, task maps, environment evidence
- Output: JSON with `rollout_summary`, `rollout_slug`, `raw_memory`
- Raw memory has YAML frontmatter: description, task, task_group, task_outcome, cwd, keywords

**Phase 2** (`memories/consolidation.md`, 50+ lines) — Consolidation Agent:
- Merges Phase 1 raw memories into persistent folder structure:
  - `memory_summary.md` — always loaded, navigational
  - `MEMORY.md` — handbook entries, grep-searchable
  - `raw_memories.md` — merged Phase 1 input
  - `skills/` — reusable procedures with `SKILL.md` entrypoint
  - `rollout_summaries/` — distilled recaps
- Safety: raw rollouts immutable; no large tool output copies

### 8. User Instructions (`instructions/mod.rs`)

A single re-export: `pub(crate) use codex_instructions::UserInstructions;`

The `codex_instructions` crate handles repo-level AGENTS.md files — the Codex equivalent of CLAUDE.md. These are user-authored markdown files that scope agent behavior to specific directories. More-deeply-nested files take precedence.

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| Model selection | Which `gpt_5*_prompt.md` variant is used |
| `config_prompt` | Overrides realtime backend prompt |
| `{{ personality }}` injection | Pragmatic vs friendly tone |
| `{{ user_first_name }}` | Personalizes realtime prompt |
| AGENTS.md files (per-repo) | Scoped behavioral directives |

## Design Decisions

- **Compile-time embedding guarantees prompt stability** — `include_str!()` means the prompt is literally part of the binary. No filesystem reads, no race conditions, no cache invalidation between turns. The cost is that prompt iteration requires recompilation.
- **Model-versioned prompts over conditional logic** — Rather than one prompt with `if (model === "gpt-5.2")` branches, Codex maintains separate complete files. This makes each prompt self-contained and auditable, at the cost of some duplication across versions.
- **Personality as injectable template** — Personality is a swappable layer, not hard-coded. The same task instructions apply regardless of tone, keeping behavioral guarantees consistent.
- **Two-phase memory with NO-OP gate** — Phase 1 includes an explicit "is this worth remembering?" gate to prevent memory accumulation of low-value observations. This is more aggressive filtering than Claude Code's approach.

## See Also

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — How the prompt feeds into Codex's event-driven execution
- [04 - Context Management](references/dimensions/04-context-management.md) — AGENTS.md convention and its role in context
- [Codex — Context Management](references/code-analysis/context/codex-context-management.md) — How the compaction prompt is executed
- [Codex — Agent Loop](references/code-analysis/loops/codex-agent-loop.md) — How prompts flow through the `Session` → `TurnContext` pipeline

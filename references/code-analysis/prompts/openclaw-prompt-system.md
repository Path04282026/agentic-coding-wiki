---
tags: [code-analysis]
product: OpenClaw
pillar: prompts
status: complete
created: 2026-04-12
key_files:
  - code/openclaw/src/agents/system-prompt.ts
  - code/openclaw/src/agents/system-prompt-params.ts
  - code/openclaw/src/agents/system-prompt-contribution.ts
  - code/openclaw/src/agents/system-prompt-cache-boundary.ts
  - code/openclaw/src/agents/system-prompt-override.ts
  - code/openclaw/src/agents/system-prompt-report.ts
  - code/openclaw/src/auto-reply/reply/prompt-prelude.ts
  - code/openclaw/src/auto-reply/reply/commands-system-prompt.ts
---

# OpenClaw — Prompt System

## Purpose

OpenClaw builds its system prompt through a **multi-layer assembly** driven by workspace context files, provider contributions, and a cache-aware boundary that splits stable from volatile content. The prompt supports three rendering modes (full/minimal/none), tool-driven conditional sections, and a workspace-as-identity pattern where files like `soul.md` and `identity.md` define the agent's personality.

## Architecture

```
resolveCommandsSystemPromptBundle()     ← commands-system-prompt.ts
        │
        ├── Load bootstrap + context files from workspace
        ├── Detect sandbox runtime
        ├── Build skills snapshot
        ├── Create available tools list
        ├── Resolve model, TTS hints
        │
        ▼
buildAgentSystemPrompt()                ← system-prompt.ts (868 lines)
        │
        ├── Identity line
        ├── Tooling (conditional on available tools)
        ├── Tool Call Style ◄── overridable by provider
        ├── Execution Bias  ◄── overridable by provider
        ├── Provider Stable Prefix ◄── provider contribution
        ├── Safety rules
        ├── Skills, Memory, Self-Update, Model Aliases
        ├── Workspace + Documentation
        ├── Sandbox configuration
        ├── Reply Tags, Messaging, Voice, Reactions
        ├── Reasoning format
        ├── Stable context files (agents.md → soul.md → ... → memory.md)
        ├── Silent reply rules
        │
        ├── ── CACHE BOUNDARY ── <!-- OPENCLAW_CACHE_BOUNDARY --> ──
        │
        ├── Dynamic context files (heartbeat.md)
        ├── Group Chat / Subagent context
        ├── Provider Dynamic Suffix ◄── provider contribution
        ├── Heartbeats
        └── Runtime line (agent/host/OS/model/channel)
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `system-prompt.ts` | Main assembly — 30+ sections, PromptMode, context file ordering | 868 |
| `system-prompt-params.ts` | Runtime parameter resolution (timezone, repo root, time format) | 95 |
| `system-prompt-contribution.ts` | Provider contribution types (stablePrefix, dynamicSuffix, sectionOverrides) | 28 |
| `system-prompt-cache-boundary.ts` | Cache boundary marker, split/prepend operations | 47 |
| `system-prompt-override.ts` | Per-agent and global prompt override resolution | 27 |
| `system-prompt-report.ts` | Diagnostic report: char counts, tool stats, file stats | 129 |
| `prompt-prelude.ts` | Reply body assembly with media hints and event context | 47 |
| `commands-system-prompt.ts` | Bundle resolver: loads files, tools, sandbox → calls buildAgentSystemPrompt | 162 |

## Implementation Walkthrough

### 1. PromptMode — Three Rendering Modes

```typescript
type PromptMode = "full" | "minimal" | "none";
```

| Mode | Used By | Includes | Excludes |
|------|---------|----------|----------|
| `full` | Main agent | All 30+ sections | Nothing |
| `minimal` | Sub-agents | Tooling, Safety, Workspace, Sandbox, Runtime | Skills, Memory, Self-Update, Model Aliases, Reply Tags, Messaging, Silent Replies, Heartbeats |
| `none` | Bare context | Only: "You are a personal assistant operating inside OpenClaw." | Everything else |

### 2. Context File Pipeline

Workspace context files define the agent's identity and behavior. They are loaded, sorted, split by stability, and injected at specific positions in the prompt.

**Ordering** — `CONTEXT_FILE_ORDER` assigns a numeric priority:

| File | Order | Purpose |
|------|-------|---------|
| `agents.md` | 10 | Session startup sequence, memory conventions, red lines, group chat behavior |
| `soul.md` | 20 | Personality, tone, opinions, humor |
| `identity.md` | 30 | Agent name, theme, emoji, avatar |
| `user.md` | 40 | Who you're helping, user preferences |
| `tools.md` | 50 | Local tool notes (camera names, SSH details, voice) |
| `bootstrap.md` | 60 | First-run setup (deleted after first run) |
| `memory.md` | 70 | Long-term memory (main session only) |
| (unknown files) | MAX_SAFE_INTEGER | Sorted last, then alphabetically |

**Sorting function** (`sortContextFilesForPrompt`):
1. Normalize paths to forward slashes, lowercase basename
2. Compare by order number from map
3. Tie-break: basename alphabetically, then full path

**Stability split**:
- `DYNAMIC_CONTEXT_FILE_BASENAMES = new Set(["heartbeat.md"])` — only heartbeat is dynamic
- Stable files → injected ABOVE cache boundary (cached across turns)
- Dynamic files → injected BELOW cache boundary (recomputed per turn)

**Content sanitization** (`sanitizeContextFileContentForPrompt`):
- Strips the default heartbeat prompt context block (conflicts with Claude subscription policy)
- Collapses 3+ newlines to 2 newlines

When `soul.md` is present, a personality directive is injected: "If SOUL.md is present, embody its persona and tone. Avoid stiff, generic replies; follow its guidance unless higher-priority instructions override it."

### 3. Provider Contributions

Model providers (e.g., Anthropic, OpenAI) can customize the prompt without replacing it:

```typescript
type ProviderSystemPromptContribution = {
  stablePrefix?: string;      // Above cache boundary
  dynamicSuffix?: string;     // Below cache boundary
  sectionOverrides?: Partial<Record<
    ProviderSystemPromptSectionId,  // "interaction_style" | "tool_call_style" | "execution_bias"
    string
  >>;
};
```

| Contribution | Position | Purpose |
|-------------|----------|---------|
| `stablePrefix` | Above cache boundary | Static model-family instructions |
| `dynamicSuffix` | Below cache boundary | Per-turn volatile content |
| `sectionOverrides` | Replaces named sections | Model-specific interaction style, tool call style, or execution bias |

All contributions pass through `normalizeStructuredPromptSection()` for deterministic whitespace.

### 4. Cache Boundary

```typescript
const SYSTEM_PROMPT_CACHE_BOUNDARY = "\n<!-- OPENCLAW_CACHE_BOUNDARY -->\n";
```

An HTML comment marker (transparent to the model) that splits the prompt for provider transport shaping. The `splitSystemPromptCacheBoundary()` function returns `{ stablePrefix, dynamicSuffix }`, both trimmed.

**What's above** (cached): Tooling, Safety, Skills, Workspace, Docs, Sandbox, Senders, Timezone, Reply Tags, Messaging, Voice, Reactions, Reasoning, stable context files, Silent Replies.

**What's below** (volatile): Dynamic context files (heartbeat.md), Group Chat/Subagent context, Provider dynamic suffix, Heartbeats, Runtime line.

### 5. Tool-Driven Conditional Sections

The prompt includes or excludes sections based on which tools are available:

| Tool Present | Section Enabled |
|-------------|----------------|
| `cron` | "Use cron for scheduling instead of sleep loops" |
| `update_plan` | Multi-step work planning guidance |
| `sessions_spawn` | ACP harness request handling |
| `gateway` | OpenClaw self-update section |
| `message` | Messaging tool usage guidance |

Tool names are deduplicated by lowercase normalization while preserving original casing:
```typescript
const canonicalByNormalized = new Map<string, string>();
for (const name of canonicalToolNames) {
  const normalized = normalizeLowercaseStringOrEmpty(name);
  if (!canonicalByNormalized.has(normalized)) {
    canonicalByNormalized.set(normalized, name);
  }
}
```

### 6. Prompt Overrides

`system-prompt-override.ts` provides a two-level fallback:
1. Agent-specific: `agents.list[id].systemPromptOverride`
2. Global default: `agents.defaults.systemPromptOverride`

Both are trimmed; empty strings return undefined (no override).

### 7. Bundle Resolution (`commands-system-prompt.ts`)

The `resolveCommandsSystemPromptBundle()` function is the top-level orchestrator that gathers all inputs before calling `buildAgentSystemPrompt()`:

1. Resolve session agent ID
2. Load bootstrap and context files from workspace
3. Detect sandbox runtime status
4. Build skills snapshot
5. Create available tools list (respects provider, group context, sender)
6. Resolve default model reference (with fallback chain)
7. Determine sandbox/elevated access state
8. Build TTS hints
9. Call `buildAgentSystemPrompt()` with all resolved parameters

Returns a bundle: `{ systemPrompt, tools, skillsPrompt, bootstrapFiles, injectedFiles, sandboxRuntime }`.

### 8. Diagnostic Reporting (`system-prompt-report.ts`)

`buildSystemPromptReport()` generates a full diagnostic snapshot:
- Character counts: total system prompt, project context portion, non-project portion
- Per-file stats: each injected workspace file with size and truncation status
- Skills entries: name and character count per skill block
- Tools entries: name, summary chars, schema chars, property count
- Bootstrap metadata: max chars, truncation mode

This enables cost/budget analysis of prompt composition.

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| Workspace context files | Agent personality (`soul.md`), identity, tools, memory |
| `agents.defaults.systemPromptOverride` | Global prompt override |
| `agents.list[id].systemPromptOverride` | Per-agent prompt override |
| Provider plugin contributions | Model-family specific sections |
| `agents.defaults.userTimezone` | Timezone in prompt |
| `agents.defaults.timeFormat` | Time display format (auto/12/24) |
| Tool availability | Conditional section inclusion |
| PromptMode parameter | full/minimal/none rendering |

## Design Decisions

- **Workspace-as-identity** — Unlike Claude Code (where identity is in the system prompt) or Codex (where it's in compiled templates), OpenClaw's agent identity lives in workspace files (`soul.md`, `identity.md`). This means the same codebase can power radically different agents by changing workspace files.
- **Provider contributions as seams, not overrides** — Providers can't replace the whole prompt. They get three injection points (stablePrefix, dynamicSuffix, sectionOverrides for 3 named sections). This prevents providers from accidentally breaking safety rules or workspace context.
- **Cache boundary as HTML comment** — Transparent to the model but usable by transport layers (API clients) to set cache breakpoints. Lets OpenClaw optimize API costs per provider without changing prompt semantics.
- **Deterministic assembly for cache stability** — All maps, sets, and arrays are processed in stable order. Tool lists deduplicated. Context files sorted by explicit priority. Whitespace normalized. This is treated as a correctness/perf-critical concern, not cosmetic.
- **Time in timezone, not timestamp** — The prompt includes the user's timezone but NOT the current time (which would break cache every turn). The agent gets current time via `session_status` tool call — a deliberate cache-stability tradeoff.

## See Also

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Design philosophy behind the prompt/loop interaction
- [04 - Context Management](references/dimensions/04-context-management.md) — Context file conventions and their role
- [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md) — How context tokens are resolved and managed
- [OpenClaw — Agent Loop](references/code-analysis/loops/openclaw-agent-loop.md) — How the assembled prompt enters the agent runner

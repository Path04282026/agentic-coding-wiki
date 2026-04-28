---
tags: [code-analysis]
product: Claude Code
pillar: prompts
status: complete
created: 2026-04-12
key_files:
  - code/ClaudCode/utils/systemPrompt.ts
  - code/ClaudCode/constants/systemPromptSections.ts
  - code/ClaudCode/constants/prompts.ts
  - code/ClaudCode/utils/systemPromptType.ts
  - code/ClaudCode/tools/BashTool/prompt.ts
  - code/ClaudCode/tools/AgentTool/prompt.ts
  - code/ClaudCode/services/compact/prompt.ts
  - code/ClaudCode/services/extractMemories/prompts.ts
---

# Claude Code — Prompt System

## Purpose

Claude Code assembles its system prompt through a **priority cascade** that selects one of five prompt sources, then layers on cached and uncached sections for environment, tools, and user context. Every tool also carries its own `prompt.ts` file that defines how the model should use it.

## Architecture

```
User starts session
        │
        ▼
buildEffectiveSystemPrompt()        ← utils/systemPrompt.ts
  │
  ├─ Override set?  ──────────────► Use override (loop mode)
  ├─ Coordinator mode active? ────► getCoordinatorSystemPrompt()
  ├─ Agent definition exists?
  │   ├─ Proactive/KAIROS? ───────► Default + "\n# Custom Agent Instructions\n" + agent prompt
  │   └─ Normal? ─────────────────► Agent prompt REPLACES default
  ├─ Custom --system-prompt? ─────► Use custom
  └─ None of the above ──────────► getSystemPrompt()  ← constants/prompts.ts
        │
        ▼
  appendSystemPrompt (always added unless override is set)
        │
        ▼
  SystemPrompt (branded readonly string[])
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `utils/systemPrompt.ts` | Priority cascade — selects which prompt to use | 123 |
| `utils/systemPromptType.ts` | Branded `SystemPrompt` type (readonly string[] with `__brand`) | 14 |
| `constants/prompts.ts` | Main prompt builder — sections, feature flags, env info | 914 |
| `constants/systemPromptSections.ts` | Memoization layer for cacheable vs volatile sections | 68 |
| `tools/*/prompt.ts` | Per-tool usage instructions (one file per tool) | varies |
| `services/compact/prompt.ts` | Compaction summarization prompts (3 variants) | 374 |
| `services/extractMemories/prompts.ts` | Memory extraction prompts (auto + team modes) | 154 |

## Implementation Walkthrough

### 1. The Priority Cascade (`buildEffectiveSystemPrompt`)

The entry point in `utils/systemPrompt.ts` accepts six parameters and returns a `SystemPrompt`. The cascade is strict — higher priorities short-circuit lower ones:

| Priority | Source | When Active | Behavior |
|----------|--------|-------------|----------|
| 0 | `overrideSystemPrompt` | Loop mode | **Replaces everything**, no append |
| 1 | Coordinator prompt | `CLAUDE_CODE_COORDINATOR_MODE` env var + feature flag | Lazy-loaded from `coordinator/coordinatorMode.js` |
| 2 | Agent prompt | `mainThreadAgentDefinition` is set | In KAIROS: appended to default. Otherwise: replaces default |
| 3 | `customSystemPrompt` | `--system-prompt` CLI flag | Replaces default |
| 4 | `defaultSystemPrompt` | Fallback | Standard Claude Code prompt |

The `appendSystemPrompt` string is always added at the end (except under override). This allows hooks and extensions to inject persistent instructions.

Feature flags (`feature('PROACTIVE')`, `feature('KAIROS')`, `feature('COORDINATOR_MODE')`) use Bun's dead-code elimination — modules behind disabled flags are never bundled.

### 2. The Default Prompt Builder (`getSystemPrompt`)

`constants/prompts.ts` (914 lines) builds the default prompt. It has two code paths:

**Proactive/KAIROS mode**: Minimal prompt with autonomous agent identity, memory, environment, MCP instructions, and scratchpad guidance.

**Normal mode**: Full structured prompt split at a cache boundary:

```
┌─────────────────────────────────────────┐
│  STATIC CONTENT (cacheable across orgs) │
│  - Agent role framing                   │
│  - System instructions                  │
│  - Task guidelines                      │
│  - Action safety rules                  │
│  - Tool usage preferences               │
│  - Tone and style                       │
│  - Output efficiency                    │
├── SYSTEM_PROMPT_DYNAMIC_BOUNDARY ───────┤
│  DYNAMIC CONTENT (user-specific)        │
│  - Session guidance                     │
│  - Memory                               │
│  - Environment info                     │
│  - Language preferences                 │
│  - MCP instructions                     │
│  - Scratchpad path                      │
│  - Token budget                         │
└─────────────────────────────────────────┘
```

The boundary marker `__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__` is used by `utils/api.ts` (`splitSysPromptPrefix`) and `services/api/claude.ts` (`buildSystemPromptBlocks`) to create separate API cache blocks. Static content shares cache across the organization; dynamic content is user-specific.

### 3. Section Memoization (`systemPromptSections.ts`)

Each dynamic section is wrapped in one of two constructors:

**`systemPromptSection(name, compute)`** — Computed once, cached until `/clear` or `/compact`. Sets `cacheBreak: false`, so changes don't invalidate the API prompt cache.

**`DANGEROUS_uncachedSystemPromptSection(name, compute, reason)`** — Recomputed every turn. **Will break the prompt cache when the value changes.** Requires an explicit `_reason` string documenting why caching is unsafe.

Example from `prompts.ts`:
- `systemPromptSection('session_guidance', ...)` — stable tool tips
- `systemPromptSection('memory', ...)` — persistent user memory
- `DANGEROUS_uncachedSystemPromptSection('mcp_instructions', ..., 'MCP servers connect/disconnect between turns')` — volatile

Resolution happens via `resolveSystemPromptSections()` which resolves all sections in parallel, checking cache first.

### 4. Named Sections in the Default Prompt

| Section Builder | Content | Cache Status |
|----------------|---------|--------------|
| `getSimpleIntroSection()` | Agent role, capabilities | Static |
| `getSimpleSystemSection()` | Tool execution rules, permissions, compression | Static |
| `getSimpleDoingTasksSection()` | Code style, completeness, verification | Static |
| `getActionsSection()` | Reversibility, risk assessment, destructive actions | Static |
| `getUsingYourToolsSection(tools)` | Tool preferences (Read > cat, Edit > sed) | Static |
| `getSimpleToneAndStyleSection()` | Emoji policy, formatting, file references | Static |
| `getOutputEfficiencySection()` | Communication patterns | Static |
| `getSessionSpecificGuidanceSection()` | Tool-specific tips, agent guidance | Static |
| `session_guidance` | Conditional tool tips | Dynamic (cached) |
| `memory` | Persistent user memory | Dynamic (cached) |
| `env_info_simple` | Platform, shell, git, model info | Dynamic (cached) |
| `language` | User's language preference | Dynamic (cached) |
| `mcp_instructions` | MCP server instructions | Dynamic (uncached) |

### 5. Per-Tool Prompt Files

Every tool in `tools/` has its own `prompt.ts` that defines how the model should interact with it. These are not part of the system prompt — they're passed as tool descriptions in the API call.

**`BashTool/prompt.ts`** (369 lines) — the most complex. Includes:
- Tool preference rules (use Read not cat, Edit not sed, etc.)
- Git safety protocol: never amend, never force-push, never skip hooks, always create new commits
- Commit workflow (4-step: status/diff/log → draft message → stage + commit → verify)
- PR creation workflow (3-step: status/diff/log → draft → push + create)
- Sandbox filesystem/network rules
- Command execution guidance (parallel vs sequential, timeouts, background)

**`FileReadTool/prompt.ts`** (49 lines) — constraints: absolute paths, 2000-line default, supports images/PDFs/notebooks.

**`AgentTool/prompt.ts`** (287 lines) — two modes:
- **Coordinator mode**: slim shared section (coordinator system prompt has details)
- **Normal mode**: full guidance with fork semantics (if enabled), prompt-writing tips, when-not-to-use rules, parallel agent launch patterns

### 6. Specialized Prompts

**Compaction prompt** (`services/compact/prompt.ts`, 374 lines) — three variants:
- `BASE_COMPACT_PROMPT`: Full conversation summary with 9 required sections (intent, concepts, files, errors, problem solving, user messages, pending tasks, current work, next step)
- `PARTIAL_COMPACT_PROMPT`: Summary of recent messages only (earlier context retained)
- `PARTIAL_COMPACT_UP_TO_PROMPT`: Prefix for continuing work after partial compaction

All use `<analysis>` drafting blocks (stripped post-summary) and `<summary>` output blocks. Include `NO_TOOLS_PREAMBLE` + `NO_TOOLS_TRAILER` to enforce text-only output.

> Compaction *execution* is covered in [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md).

**Memory extraction prompt** (`services/extractMemories/prompts.ts`, 154 lines) — two variants:
- `buildExtractAutoOnlyPrompt()`: Four memory types, private directory only
- `buildExtractCombinedPrompt()`: Four types with per-type scope guidance (private vs team directory). Falls back to auto-only if `TEAMMEM` feature disabled.

Provides tools (FILE_READ, GREP, GLOB, read-only BASH, FILE_EDIT/FILE_WRITE for memory dir only) and a budget: read all files in turn 1, write all in turn 2, no interleaving.

> Memory *system* is covered in [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md).

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| `--system-prompt` CLI flag | Custom system prompt (priority 3) |
| `appendSystemPrompt` parameter | Always-appended instructions |
| `CLAUDE_CODE_COORDINATOR_MODE` env var | Coordinator mode (priority 1) |
| Feature flags (compile-time) | PROACTIVE, KAIROS, COORDINATOR_MODE, CACHED_MICROCOMPACT, TOKEN_BUDGET, etc. |
| `mainThreadAgentDefinition` | Agent-specific prompt (priority 2) |

## Design Decisions

- **Branded type prevents accidental string[] usage** — `SystemPrompt` is a compile-time branded type (`readonly string[] & { __brand: 'SystemPrompt' }`). Forces all prompt construction through `asSystemPrompt()`.
- **Cache boundary maximizes API cache hits** — Static sections shared across all users in an org; only dynamic sections vary per session. This maps directly to Anthropic's prompt caching pricing.
- **Per-tool prompts keep the system prompt focused** — Tool-specific instructions travel as tool descriptions, not system prompt bloat. The model sees them only when the tool is available.
- **Memoization with explicit cache-break labeling** — `DANGEROUS_uncachedSystemPromptSection` forces developers to justify why a section can't be cached, creating social pressure toward stability.

## See Also

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Design rationale for the prompt/loop interaction
- [04 - Context Management](references/dimensions/04-context-management.md) — How CLAUDE.md files feed into the prompt
- [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) — How compaction prompts are executed
- [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md) — How memory extraction prompts are executed
- [Claude Code — Agent Loop](references/code-analysis/loops/claude-code-agent-loop.md) — How the assembled prompt enters the API call

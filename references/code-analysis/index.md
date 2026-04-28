---
tags: [code-analysis, index]
type: index
status: complete
created: 2026-04-12
---

# Code Analysis — Prompts, Context & Agent Loops

Deep dives into the source code of all three projects. Each page traces how the code actually works — file paths, data flows, key functions — complementing the [dimension pages](references/dimensions/01-agent-loop.md) that cover design rationale from interviews and blogs.

**Rule**: each source file is analyzed on exactly one page. Other pages link, never re-explain.

---

## Prompt Systems

How each project assembles the system prompt before calling the LLM.

| Page | Project | Key Files |
|------|---------|-----------|
| [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) | Claude Code | `systemPrompt.ts`, `systemPromptSections.ts`, `tools/*/prompt.ts` |
| [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md) | Codex | `gpt_5*_prompt.md`, `realtime_prompt.rs`, `instructions/mod.rs` |
| [OpenClaw — Prompt System](references/code-analysis/prompts/openclaw-prompt-system.md) | OpenClaw | `system-prompt.ts`, `system-prompt-*.ts`, context files pipeline |

## Context Management

How each project manages the context window — compaction, token counting, message history.

| Page | Project | Key Files |
|------|---------|-----------|
| [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) | Claude Code | `compact/compact.ts`, `autoCompact.ts`, `tokens.ts`, `messages.ts` |
| [Codex — Context Management](references/code-analysis/context/codex-context-management.md) | Codex | `compact.rs`, `environment_context.rs`, `session_prefix.rs` |
| [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md) | OpenClaw | `context.ts`, `agent-runner-memory.ts`, `sessions.ts` |

## Agent Loops

The core execution loop: receive input, call the model, execute tools, repeat.

| Page | Project | Key Files |
|------|---------|-----------|
| [Claude Code — Agent Loop](references/code-analysis/loops/claude-code-agent-loop.md) | Claude Code | `query.ts`, `REPL.tsx`, `toolOrchestration.ts`, `toolExecution.ts` |
| [Codex — Agent Loop](references/code-analysis/loops/codex-agent-loop.md) | Codex | `codex.rs`, `codex_thread.rs`, `tasks/*.rs`, `tools/*.rs` |
| [OpenClaw — Agent Loop](references/code-analysis/loops/openclaw-agent-loop.md) | OpenClaw | `agent-runner.ts`, `agent-runner-execution.ts`, `pi-embedded.ts` |

## Memory

Subsystems distinct from context-window management — persistent memory across sessions.

| Page | Project | Key Files |
|------|---------|-----------|
| [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md) | Claude Code | `SessionMemory/`, `memdir/`, `extractMemories/`, Dream system |

> Codex has no standalone memory subsystem — session state is covered in [Codex — Context Management](references/code-analysis/context/codex-context-management.md).
> OpenClaw's plugin-based memory is covered in [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md).

---

## How This Connects to the Wiki

- **[01 - Agent Loop](references/dimensions/01-agent-loop.md)** (dimension) explains *why* each loop works the way it does (designer rationale from interviews). The Agent Loop code pages here show *how* (source code walkthrough).
- **[04 - Context Management](references/dimensions/04-context-management.md)** (dimension) covers the design philosophy of compaction and CLAUDE.md files. The Context Management code pages here trace the implementation.
- **[02 - Tool Systems](references/dimensions/02-tool-systems.md)** (dimension) covers tool design and MCP. Tool *execution* mechanics are part of the Agent Loop code pages here.

```dataview
TABLE status, product, pillar
FROM "wiki/code-analysis"
WHERE type != "index"
SORT pillar, file.name ASC
```

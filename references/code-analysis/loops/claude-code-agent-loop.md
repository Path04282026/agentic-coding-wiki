---
tags: [code-analysis]
product: Claude Code
pillar: loops
status: complete
created: 2026-04-12
key_files:
  - code/ClaudCode/query.ts
  - code/ClaudCode/QueryEngine.ts
  - code/ClaudCode/screens/REPL.tsx
  - code/ClaudCode/services/tools/toolOrchestration.ts
  - code/ClaudCode/services/tools/toolExecution.ts
  - code/ClaudCode/services/tools/StreamingToolExecutor.ts
  - code/ClaudCode/Tool.ts
---

# Claude Code — Agent Loop

## Purpose

Claude Code's agent loop is a **streaming query cycle** driven by `query.ts`: the user sends a message, the system assembles the prompt, calls the Anthropic API with streaming, executes any tool calls returned by the model, feeds results back, and repeats until the model produces a final response with no tool calls. The REPL (`REPL.tsx`) manages the user-facing terminal UI; the `QueryEngine` bridges REPL and query loop.

## Architecture

```
User types in terminal
        │
        ▼
REPL.tsx (React/Ink)
  └─ Captures input, attachments, slash commands
        │
        ▼
QueryEngine.ts
  └─ Orchestrates: builds messages, manages state, calls query()
        │
        ▼
query() ← query.ts                    ┌──────────────────────┐
  │                                    │  INNER LOOP          │
  ├─ buildEffectiveSystemPrompt()      │  (tool call cycle)   │
  ├─ normalizeMessagesForAPI()         │                      │
  ├─ microcompactMessages()            │  API response         │
  ├─ Call Anthropic API (streaming) ──►│    ├─ text_delta     │
  │                                    │    ├─ tool_use       │
  │   ◄──── Stream events ────────────│    └─ stop_reason    │
  │                                    │                      │
  ├─ If tool_use blocks present:       │  Execute tools       │
  │   └─ runTools() ──────────────────►│    ├─ concurrent     │
  │       └─ runToolUse() per tool     │    └─ serial         │
  │           └─ canUseTool check      │                      │
  │           └─ Tool.call()           │  Feed results back   │
  │           └─ Return tool_result    │    └─ Append to msgs │
  │                                    │                      │
  │   Loop back to API call ──────────►│                      │
  │                                    └──────────────────────┘
  ├─ If stop_reason == "end_turn":
  │   └─ Check auto-compact threshold
  │   └─ Return final response
  │
  └─ Error handling:
      ├─ FallbackTriggeredError → retry with fallback model
      ├─ PROMPT_TOO_LONG → trigger compaction, retry
      └─ ImageSizeError/ImageResizeError → strip images, retry
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `query.ts` | Core query loop — prompt assembly, API call, tool cycle, auto-compact | ~2,000+ |
| `QueryEngine.ts` | High-level orchestration — state management, message building | ~1,000+ |
| `screens/REPL.tsx` | React/Ink terminal UI — input capture, display, slash commands | varies |
| `services/tools/toolOrchestration.ts` | `runTools()` — partitions concurrent vs serial, dispatches | varies |
| `services/tools/toolExecution.ts` | `runToolUse()` — individual tool execution with permission checks | varies |
| `services/tools/StreamingToolExecutor.ts` | Handles streaming tool results (progressive output) | varies |
| `Tool.ts` | Tool type definitions, `findToolByName()` | varies |

## Implementation Walkthrough

### 1. The REPL Layer (`REPL.tsx`)

The terminal UI is built with React and Ink (a React renderer for CLIs). It manages:
- User text input with history
- File/image attachments
- Slash command parsing (e.g., `/compact`, `/clear`, `/help`)
- Interruption handling (Ctrl+C)
- Streaming response display
- Permission prompts for tool approval

When the user submits a message, REPL packages it as a `UserMessage` and passes it to the QueryEngine.

### 2. The QueryEngine (`QueryEngine.ts`)

The QueryEngine is the bridge between UI and query loop. Its imports reveal the system's complexity — 100+ imports spanning prompts, tools, compaction, analytics, memory, hooks, and more.

Key responsibilities:
- Building the initial message array from user input + attachments
- Managing conversation state across turns
- Calling `query()` and handling its return value
- Updating the UI with streaming events
- Persisting session state

### 3. The Query Loop (`query.ts`)

This is the heart of the agent. The main function `query()` implements the streaming tool-call cycle:

**Step 1: Prompt assembly**
- `buildEffectiveSystemPrompt()` selects the prompt variant (see [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md))
- Messages are processed through `normalizeMessagesForAPI()` (see [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md))
- `microcompactMessages()` may clear old tool results before sending

**Step 2: API call**
- Calls the Anthropic API with streaming enabled
- Receives `StreamEvent` objects: `text_delta`, `tool_use`, `input_json_delta`, `content_block_stop`, `message_stop`

**Step 3: Tool call detection**
- When the model's response contains `tool_use` blocks, these are extracted
- The response's `stop_reason` determines flow:
  - `"end_turn"` → model is done, no more tools
  - `"tool_use"` → model wants tool results before continuing

**Step 4: Tool execution** (see below)

**Step 5: Feed results back**
- Tool results are appended to the message array as `tool_result` blocks
- Loop returns to Step 2 — the model sees its previous response + tool results

**Step 6: Auto-compact check**
- After the model produces a final response (no more tool calls), `calculateTokenWarningState()` checks if context is approaching the limit
- If above threshold, `autoCompactIfNeeded()` fires (see [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md))

**Error recovery paths:**
- `FallbackTriggeredError`: retry the entire query with a fallback model
- `PROMPT_TOO_LONG_ERROR_MESSAGE`: trigger compaction to shrink context, then retry
- `ImageSizeError` / `ImageResizeError`: strip problematic images and retry
- `reactiveCompact` (feature-gated): dynamic compaction during streaming if context grows too large mid-turn
- `contextCollapse` (feature-gated): context collapse as an alternative to full compaction

### 4. Tool Orchestration (`toolOrchestration.ts`)

**`runTools(toolUseBlocks, context)`** — a generator function that partitions and executes tool calls:

**Concurrent execution**: tools that don't conflict can run in parallel. The orchestrator groups independent tools and awaits them together.

**Serial execution**: tools that modify shared state (e.g., file writes) must run sequentially to avoid conflicts.

**The partitioning decision** is based on tool properties — read-only tools (Grep, Glob, Read) can be concurrent; mutating tools (Edit, Write, Bash) are serial.

Each tool call goes through `runToolUse()`.

### 5. Tool Execution (`toolExecution.ts`)

**`runToolUse(toolUseBlock, context)`** — executes a single tool:

1. **Find the tool**: `findToolByName(toolUseBlock.name)` looks up the tool in the registry
2. **Permission check**: `canUseTool()` hook determines if the tool is allowed:
   - Automatic approval (configured in permissions)
   - User prompt (asks for confirmation)
   - Deny (tool blocked)
3. **Execution**: calls `tool.call(toolUseBlock.input, context)`
4. **Result formatting**: wraps the tool's output in a `ToolResultBlockParam`
5. **Streaming**: `StreamingToolExecutor` handles tools that produce progressive output (e.g., long Bash commands)

### 6. The Tool Registry (`Tool.ts`)

Each tool implements the `Tool` interface:
- `name`: tool name as the model sees it
- `description`: tool description (from `prompt.ts`)
- `inputSchema`: JSON schema for parameters
- `call(input, context)`: execution function
- Various flags: `isReadOnly`, `isBuiltIn`, `requiresApproval`, etc.

**`findToolByName(name)`** searches the enabled tool list. Tools can be dynamically added (MCP tools) or removed (permission restrictions).

Tools are organized in `tools/` with each tool getting its own directory containing the implementation and a `prompt.ts` (see [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) for prompt details).

### 7. Feature-Gated Loop Variants

Several loop variants are behind compile-time feature flags:

**`REACTIVE_COMPACT`**: monitors token growth during streaming. If context is about to overflow mid-turn, triggers compaction without waiting for the turn to complete.

**`CONTEXT_COLLAPSE`**: an alternative to full compaction. Instead of summarizing the entire conversation, selectively collapses sections (e.g., old tool results, completed sub-tasks). Uses different thresholds: 90% commit, 95% blocking.

**`CACHED_MICROCOMPACT`**: uses Anthropic's `cache_edits` API to delete old tool results server-side without invalidating the local prompt cache.

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| Tool registry | Which tools are available to the model |
| Permission hooks (`canUseTool`) | Approval policy per tool |
| Feature flags (compile-time) | REACTIVE_COMPACT, CONTEXT_COLLAPSE, CACHED_MICROCOMPACT |
| `ESCALATED_MAX_TOKENS` (64K) | Output token limit for query retries |
| `CAPPED_DEFAULT_MAX_TOKENS` (8K) | Default output token cap (slot reservation) |

## Design Decisions

- **Generator-based tool orchestration** — `runTools()` is a generator, yielding results as they complete. This allows the UI to show progressive tool execution rather than waiting for all tools to finish before displaying anything.
- **Concurrent read, serial write** — Read-only tools (Grep, Glob, Read) run in parallel for speed. Mutating tools (Edit, Write, Bash) run sequentially to prevent file corruption. This is a pragmatic middle ground between full parallelism and full serialization.
- **Streaming-first** — The entire loop is built around streaming. Text deltas are displayed as they arrive; tool calls are extracted from streaming blocks; even compaction streams its summary. This makes the system feel responsive even on slow turns.
- **Multiple error recovery strategies** — Rather than a single retry mechanism, the loop has specialized recovery for each failure mode: model fallback, compaction for context overflow, image stripping for size errors. Each path is narrowly targeted.
- **React/Ink for terminal UI** — The REPL uses React's component model for the terminal, enabling complex UI state management (loading indicators, permission prompts, streaming text) with the same patterns used in web UIs.

## See Also

- [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) — How the system prompt is assembled before the API call
- [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) — Auto-compact triggers and compaction execution
- [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md) — Memory extraction triggered after turns
- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Design rationale from Boris Cherny's interviews

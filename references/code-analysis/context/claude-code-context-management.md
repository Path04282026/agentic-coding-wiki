---
tags: [code-analysis]
product: Claude Code
pillar: context
status: complete
created: 2026-04-12
key_files:
  - code/ClaudCode/services/compact/compact.ts
  - code/ClaudCode/services/compact/autoCompact.ts
  - code/ClaudCode/services/compact/microCompact.ts
  - code/ClaudCode/services/compact/postCompactCleanup.ts
  - code/ClaudCode/utils/tokens.ts
  - code/ClaudCode/utils/context.ts
  - code/ClaudCode/utils/messages.ts
  - code/ClaudCode/utils/contextAnalysis.ts
---

# Claude Code — Context Management

## Purpose

Claude Code manages context through a **tiered compaction system**: micro-compaction clears old tool results without summarizing; auto-compaction triggers a full LLM-generated summary when tokens approach the limit; and a circuit breaker prevents infinite compaction loops. Token counting piggybacks on API usage data rather than running a local tokenizer.

## Architecture

```
Each turn: tokenCountWithEstimation()
    │
    ▼
calculateTokenWarningState(tokens, model)
    │
    ├─ < warningThreshold ─────────► Normal operation
    ├─ > warningThreshold ─────────► Show warning UI
    ├─ > errorThreshold ───────────► Show error UI
    ├─ > autoCompactThreshold ─────► Trigger auto-compaction
    │   │
    │   ├─ trySessionMemoryCompaction() ──► Prune old messages (lightweight)
    │   │   └─ Success? ──► Done
    │   │
    │   └─ compactConversation() ──► Full LLM summary
    │       ├─ streamCompactSummary() ──► Call model with 9-section prompt
    │       ├─ createPostCompactFileAttachments() ──► Restore top 5 recent files
    │       └─ buildPostCompactMessages() ──► [boundary, summary, attachments]
    │
    └─ > blockingLimit ────────────► Block user input until /compact
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `services/compact/compact.ts` | Main compaction orchestrator — summary, attachments, hooks | 1,705 |
| `services/compact/autoCompact.ts` | Trigger logic — thresholds, circuit breaker, decision | 351 |
| `services/compact/microCompact.ts` | Lightweight tool result clearing (time-based + cached) | 530 |
| `services/compact/postCompactCleanup.ts` | State reset after compaction | 77 |
| `utils/tokens.ts` | Token counting using API usage data | 261 |
| `utils/context.ts` | Context window constants and model-specific limits | 221 |
| `utils/messages.ts` | Message types, normalization, boundary markers | 5,512 |
| `utils/contextAnalysis.ts` | Token breakdown analysis for telemetry | 273 |

## Implementation Walkthrough

### 1. Token Counting (`tokens.ts`)

Claude Code does **not** run a local tokenizer. Instead, it uses a hybrid approach:

**`tokenCountWithEstimation(messages)`** — the core estimator:
1. Walk backward from the last message to find the most recent assistant message with `usage` data (returned by the API)
2. Handle parallel tool calls: when the model emits multiple tool_calls, streaming splits them into separate assistant records with the same `message.id`. Walk back to the FIRST sibling so all interleaved tool_results are counted.
3. Return: `getTokenCountFromUsage(usage) + roughTokenCountEstimation(newMessages)`
   - API's actual usage (input + output + cache tokens) = ground truth up to that point
   - Rough character-based estimate of messages added since = cheap approximation
4. Fallback: if no message has usage data, estimate everything from scratch

**`getTokenCountFromUsage(usage)`**: `input_tokens + cache_creation_input_tokens + cache_read_input_tokens + output_tokens`

**`finalContextTokensFromLastResponse(messages)`**: Used for server-side task budget tracking. Returns `input_tokens + output_tokens` (excludes cache tokens to match server calculation).

### 2. Context Window Sizing (`context.ts`)

**`getContextWindowForModel(model, betas?)`** — resolution order:
1. `CLAUDE_CODE_MAX_CONTEXT_TOKENS` env var (internal override)
2. Model suffix: `[1m]` → 1M context
3. Model capabilities API: `max_input_tokens >= 100K`
4. SDK beta header: `CONTEXT_1M_BETA_HEADER`
5. Internal model definitions
6. Default: 200,000

**Key constants**:

| Constant | Value | Purpose |
|----------|-------|---------|
| `MODEL_CONTEXT_WINDOW_DEFAULT` | 200,000 | Base window for all models |
| `COMPACT_MAX_OUTPUT_TOKENS` | 20,000 | Reserved for compaction summaries |
| `ESCALATED_MAX_TOKENS` | 64,000 | Max tokens when escalating a query retry |
| `CAPPED_DEFAULT_MAX_TOKENS` | 8,000 | Slot-reservation optimization cap |

**Model max output tokens** (model-specific):
- Opus 4.6: default 64K, upper 128K
- Sonnet 4.6: default 32K, upper 128K
- Haiku 4: default 32K, upper 64K

### 3. Auto-Compaction Triggers (`autoCompact.ts`)

**`getAutoCompactThreshold(model)`**:
- `effectiveWindow = contextWindow - reservedForSummary` (reserves min(modelMaxOutput, 20K))
- `threshold = effectiveWindow - 13,000` (the AUTOCOMPACT_BUFFER)
- Override via `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` env (0-100%)

**`calculateTokenWarningState(tokenUsage, model)`** — returns:

| State | Condition | Effect |
|-------|-----------|--------|
| `isAboveWarningThreshold` | tokens > threshold - 20K | Warning UI shown |
| `isAboveErrorThreshold` | tokens > threshold - 20K | Error UI shown |
| `isAboveAutoCompactThreshold` | tokens > threshold | Auto-compaction fires |
| `isAtBlockingLimit` | tokens > effectiveWindow - 3K | User input blocked |
| `percentLeft` | (threshold - tokens) / threshold * 100 | Progress indicator |

**`shouldAutoCompact(messages, model, querySource)`** — guards before triggering:
- Returns false if querySource is `'session_memory'` or `'compact'` (recursion guard)
- Returns false if reactive-compact-only or context-collapse is active
- Returns false if autocompact is disabled (env/config)
- Returns true if `tokenCountWithEstimation(messages) >= threshold`

**`autoCompactIfNeeded()`** — the actual trigger:
- **Circuit breaker**: skips if `consecutiveFailures >= 3` (prevents infinite retry)
- Tries session memory compaction first (lightweight message pruning)
- Falls back to full `compactConversation()` if session memory insufficient
- Resets failure counter on success; increments on failure

### 4. Full Compaction (`compact.ts`)

**`compactConversation()`** — the 1,705-line orchestrator. Steps:

**Phase 1: Pre-compaction**
- Validate: at least 1 message exists
- Capture `preCompactTokenCount`
- Execute `executePreCompactHooks()` — user-defined hooks can inject instructions

**Phase 2: Summary generation**
- Call `streamCompactSummary()` with the compaction prompt (see [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md))
- **Prompt cache sharing path**: if enabled, runs a `forkedAgent` that piggybacks on the main thread's cached system prompt + tools prefix
- **Regular streaming path**: calls `queryModelWithStreaming()` with messages post-last-boundary, max 20K output tokens
- **Prompt-too-long retry**: if model returns PTL error, `truncateHeadForPTLRetry()` drops the oldest API-round groups (20% fallback if gap unparseable), up to 3 retries
- Images stripped from compaction input (prevents PTL from large images)

**Phase 3: Post-compact attachments**
- `createPostCompactFileAttachments()`: selects top 5 recently-read files (by timestamp), re-reads each with 5K token limit, accumulates within 50K total budget. Skips files already in preserved tail messages.
- Plan attachment if a plan is active
- Skill attachment if skills were invoked
- Re-announces tool/agent deltas and MCP instructions (assumes cold cache)

**Phase 4: Boundary + summary message**
- `createCompactBoundaryMessage()`: system message with `subtype: 'compact_boundary'`, captures trigger, pre-compact token count, discovered tools
- Summary wrapped in user message with `isCompactSummary: true`, `isVisibleInTranscriptOnly: true` (hidden from REPL)

**Phase 5: Cleanup + analytics**
- `runPostCompactCleanup()`: resets microcompact state, context collapse, memory cache, classifier approvals, speculative checks
- Logs `tengu_compact` event with comprehensive telemetry
- Re-appends session metadata to keep it in 16KB tail window

**`buildPostCompactMessages(result)`** — assembles the new conversation:
```
[boundaryMarker, ...summaryMessages, ...messagesToKeep, ...attachments, ...hookResults]
```

### 5. Micro-Compaction (`microCompact.ts`)

Lighter than full compaction — clears old tool results without calling the LLM.

**Two paths:**

**Time-based micro-compaction**: if gap since last assistant message > 180 minutes (default):
- Cache is already cold (user returned after long break)
- Keep last N tool results (configurable, default 1-5)
- Replace older tool_result content with `'[Old tool result content cleared]'`
- Mutates messages directly (no cache concern since cache is cold)

**Cached micro-compaction** (feature-gated `CACHED_MICROCOMPACT`):
- Uses Anthropic's `cache_edits` API for server-side deletion
- Tracks which tool results are registered in state
- Computes which to delete based on count thresholds (from GrowthBook)
- Queues `cache_edits` block — API layer applies it without mutating local messages
- Preserves prompt cache while removing bloat

`microcompactMessages()` tries time-based first (fires if cache cold), then cached path. Returns messages unchanged if neither triggers.

### 6. Message Types (`messages.ts`)

The 5,512-line message system defines the conversation's internal representation:

**Key message types**: `UserMessage`, `AssistantMessage`, `SystemMessage`, `AttachmentMessage`, `HookResultMessage`, `ToolUseSummaryMessage`, `TombstoneMessage`

**Special subtypes**: `SystemCompactBoundaryMessage` (marks compaction point), `SystemStatusMessage` (UI status updates)

**`normalizeMessagesForAPI(messages, tools)`** — transforms internal messages to API-compatible format:
- Reorders attachments to bubble up
- Strips virtual/display-only messages
- Handles PDF/image size errors (strips blocks preceded by errors)
- Merges consecutive user messages (Bedrock requirement)
- Normalizes tool inputs (removes non-API fields)
- Merges parallel tool call siblings (same `message.id`)
- Adds tool reference turn boundaries (capybara stop-sequence workaround)

**`createCompactBoundaryMessage()`**: creates the system message that marks where compaction occurred, storing:
- `trigger`: 'manual' or 'auto'
- `preTokens`: token count before compaction
- `preCompactDiscoveredTools`: tools known at compaction time (for tool search re-announcement)
- `preservedSegment`: info about any preserved message tail

### 7. Context Analysis (`contextAnalysis.ts`)

**`analyzeContext(messages)`** — breaks down token usage:
- `toolRequests`: Map<toolName, tokens> — how much each tool costs to invoke
- `toolResults`: Map<toolName, tokens> — how much each tool result consumes
- `humanMessages`, `assistantMessages`, `localCommandOutputs`, `other`: token totals
- `attachments`: Map<type, count>
- `duplicateFileReads`: Map<filePath, { count, tokens }> — detects redundant file reads

Used for telemetry (`tokenStatsToStatsigMetrics`) and post-compact analytics.

## What Gets Kept vs Discarded

| Compaction Type | Kept | Discarded |
|----------------|------|-----------|
| **Full compaction** | Boundary marker, LLM summary, top 5 recent files (50K budget), active plan, invoked skills, hook results | All original messages before boundary |
| **Partial compaction (from)** | Earlier messages intact, summary of recent | Recent messages replaced by summary |
| **Partial compaction (up_to)** | Recent messages intact, summary of older | Older messages replaced by summary |
| **Micro-compact (time)** | Last N tool results, all message structure | Older tool result *content* (structure preserved) |
| **Micro-compact (cached)** | Entire local message array, prompt cache prefix | Old tool results via server-side `cache_edits` |

## Design Decisions

- **API-based token counting avoids tokenizer dependency** — No need to ship or maintain a tokenizer. The API returns exact usage; new messages are roughly estimated until the next API call confirms. This is accurate enough for threshold decisions.
- **Tiered compaction reduces unnecessary model calls** — Micro-compaction handles the common case (old tool results bloating context) without an LLM call. Full compaction is the expensive fallback.
- **Circuit breaker at 3 consecutive failures** — Prevents runaway compaction loops where each compaction attempt fails (e.g., prompt-too-long cascade). After 3 failures, compaction is disabled until the next successful turn.
- **Post-compact file restoration** — After discarding all messages, the model loses awareness of recently-read files. Restoring the top 5 (within a 50K token budget) gives the model enough context to continue without the user having to re-read everything.
- **Context collapse interaction** — When context collapse is enabled, auto-compaction is suppressed. Collapse's 90% commit / 95% blocking flow owns headroom management; autocompact at ~93% would race it.

## See Also

- [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) — Compaction prompt templates (9-section summary format)
- [Claude Code — Agent Loop](references/code-analysis/loops/claude-code-agent-loop.md) — Where auto-compaction checks are called within the query loop
- [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md) — Session memory compaction as lightweight alternative
- [04 - Context Management](references/dimensions/04-context-management.md) — Design rationale from interviews (Boris on compaction origins)

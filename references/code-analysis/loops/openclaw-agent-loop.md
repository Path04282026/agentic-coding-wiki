---
tags: [code-analysis]
product: OpenClaw
pillar: loops
status: complete
created: 2026-04-12
key_files:
  - code/openclaw/src/auto-reply/reply/agent-runner.ts
  - code/openclaw/src/auto-reply/reply/agent-runner-execution.ts
  - code/openclaw/src/agents/pi-embedded.ts
  - code/openclaw/src/agents/pi-embedded-runner/run.ts
  - code/openclaw/src/agents/tool-loop-detection.ts
  - code/openclaw/src/agents/tool-policy.ts
---

# OpenClaw — Agent Loop

## Purpose

OpenClaw's agent loop is a **multi-layer runner** with model fallback: `runReplyAgent()` orchestrates the full reply lifecycle (preflight, execution, cleanup); `runAgentTurnWithFallback()` implements the model-call loop with error classification and candidate switching; and `runEmbeddedPiAgent()` handles the low-level inference integration with the Pi SDK. Tool loop detection prevents stuck polling patterns, and tool policies gate access by owner, allowlist, and plugin origin.

## Architecture

```
Incoming message (from any channel: Telegram, Discord, Slack, Web, Voice, ...)
        │
        ▼
runReplyAgent()                          ← agent-runner.ts (874 lines)
  │
  ├─ Phase 1: Setup
  │   └─ Typing signals, session entry, tool emit guards
  │
  ├─ Phase 2: Preflight
  │   └─ runPreflightCompactionIfNeeded()  (see Context Management page)
  │
  ├─ Phase 3: Execution
  │   └─ runAgentTurnWithFallback()      ← agent-runner-execution.ts (1,572 lines)
  │       │
  │       └─ For each fallback candidate:
  │           ├─ runEmbeddedPiAgent()    ← pi-embedded-runner/run.ts (1,772 lines)
  │           │   │
  │           │   ├─ Load plugins, resolve auth, prepare tools
  │           │   ├─ Build request with system prompt + history
  │           │   ├─ Stream inference response
  │           │   ├─ For each tool call:
  │           │   │   ├─ detectToolCallLoop() ← tool-loop-detection.ts
  │           │   │   ├─ Check tool policy    ← tool-policy.ts
  │           │   │   └─ Execute tool, feed result back
  │           │   ├─ If context overflow → compact + reload
  │           │   └─ Return result with usage metadata
  │           │
  │           ├─ On success → return
  │           ├─ On retriable error → backoff + retry
  │           └─ On failover → next candidate
  │
  ├─ Phase 4: Memory Flush
  │   └─ runMemoryFlushIfNeeded()  (see Context Management page)
  │
  ├─ Phase 5: Reply Delivery
  │   └─ Block-reply pipeline → stream/send to channel
  │
  └─ Phase 6: Session Cleanup
      └─ Persist usage, handle followups, reset if needed
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `agent-runner.ts` | Top-level orchestration — 6 phases from setup to cleanup | 874 |
| `agent-runner-execution.ts` | Execution loop with fallback — error classification, candidate switching | 1,572 |
| `pi-embedded-runner/run.ts` | Low-level inference — Pi SDK integration, streaming, tool handling | 1,772 |
| `tool-loop-detection.ts` | Detects stuck tool patterns (repeat, ping-pong, polling) | 623 |
| `tool-policy.ts` | Access control: owner-only, allowlist/denylist, plugin groups | 216 |

## Implementation Walkthrough

### 1. Top-Level Runner (`agent-runner.ts`)

**`runReplyAgent(params)`** orchestrates the complete reply lifecycle in 6 phases:

**Phase 1: Setup**
- Initialize typing signals (show "typing..." in channel)
- Create `shouldEmitToolOutput()` and `shouldEmitToolResult()` guards
- Refresh session entry from persistent store
- Determine: `shouldSteer`, `blockStreamingEnabled`, `isNewSession`

**Phase 2: Preflight Compaction**
- `runPreflightCompactionIfNeeded()` — prevent context overflow before inference
- Details in [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md)

**Phase 3: Execution**
- Calls `runAgentTurnWithFallback()` (see below)
- Returns agent result with text, media, usage, tool calls

**Phase 4: Memory Flush**
- `runMemoryFlushIfNeeded()` — consolidate memory if tokens still tight
- Details in [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md)

**Phase 5: Reply Delivery**
- Build block-reply pipeline via `createBlockReplyPipeline()`
- Stream or send results to the originating channel
- Estimate cost via `estimateUsageCost()` and append usage line
- Handle media path normalization for cross-channel delivery

**Phase 6: Session Cleanup**
- Persist updated session entry (totalTokens, compactionCount)
- Queue followup runs if needed (`enqueueFollowupRun()`)
- Refresh queued followup sessions
- Reset session if `resetTriggered` (compaction failure recovery)

### 2. Execution Loop with Fallback (`agent-runner-execution.ts`)

**`runAgentTurnWithFallback(params)`** — the core inference loop (1,572 lines):

```
for each fallback candidate (primary model → fallback models):
  try:
    result = runEmbeddedPiAgent(candidate.provider, candidate.model, ...)
    if success: apply post-processing, return
    if retriable: backoff + retry same candidate (max 1)
    if failover: move to next candidate
  catch error:
    classify error → decide action
```

**Error classification and handling:**

| Error Type | Detection | Action |
|------------|-----------|--------|
| Context Overflow | `isLikelyContextOverflowError()` | Try next model with lower context, or force compaction |
| Rate Limit / Overloaded | Pattern match on message | Cooldown + retry, or queue |
| Billing | "billing" in error | User-facing message, no retry |
| Auth Failure | OAuth refresh failure | Rebuild auth or abort |
| Transient HTTP | 5xx, connection reset | Backoff + retry same model (max 1 attempt) |
| Tool/Compaction Error | Specific error codes | Reset session or retry with fallback |

**Fallback state management:**
- `applyFallbackCandidateSelectionToEntry()` persists `providerOverride`, `modelOverride`, `authProfileOverride` to the session entry
- On success: clear overrides (return to primary model)
- On continued failure: keep fallback sticky for next turn
- `MAX_LIVE_SWITCH_RETRIES = 2` prevents infinite model ping-pong

**Output post-processing:**
- GPT-5 chat brevity guard: shorten verbose final replies on short user prompts
- Markdown capability check: filter markdown if channel doesn't support it
- Heartbeat token stripping: remove `HEARTBEAT_OK` tokens before delivery

### 3. Embedded Pi Runner (`pi-embedded-runner/run.ts`)

**`runEmbeddedPiAgent(params)`** — low-level inference integration (1,772 lines):

**Parameters include**: sessionId, provider, model, groupId, channel, prompt, extraSystemPrompt, trigger ("user"/"heartbeat"/"followup"/"budget"/"memory"), silentExpected, abortSignal, onAgentEvent callback.

**Core logic:**

1. **Initialization**: load runtime plugins, resolve auth, ensure context engine ready
2. **Message queue lane**: resolve execution lane (session-specific or global, prevents concurrent turns in same session)
3. **Model resolution**: fetch model metadata, apply auth headers
4. **Tool registration**: discover tools from plugins + built-ins, apply tool policies (see below)
5. **Inference loop**: call backend with request, stream responses
6. **Tool handling**: execute tool calls as they arrive, stream results back to model
7. **Compaction**: if session history too large during turn, compact + reload
8. **Finalization**: persist session, emit metadata, return result

**Return types:**
- `EmbeddedPiRunResult`: success with metadata (sessionId, usage, toolCalls, messageCount)
- `EmbeddedPiCompactResult`: compaction outcome (sessionId before/after, tokensAfter)

**Streaming events emitted:**
- `stream: "chunk"` — text chunk, tool calls, thinking
- `stream: "compaction"` — compaction phase events
- `stream: "error"` — classified error info

### 4. Tool Loop Detection (`tool-loop-detection.ts`)

**`detectToolCallLoop(state, toolName, params, config)`** — prevents stuck patterns:

| Pattern | Detector | Warning Threshold | Critical Threshold |
|---------|----------|-------------------|--------------------|
| **Generic Repeat** | Same tool + params, no progress | 10 calls | 20 calls |
| **Known Poll** | `process poll/log` or `command_status` repeated | 10 calls | 20 calls |
| **Ping-Pong** | Alternating patterns (A→B→A→B) | 10 calls | 20 calls |
| **Global Circuit Breaker** | Any pattern 30+ times | N/A | 30 calls (hard block) |

**Hashing strategy**: tool calls are hashed as `SHA256(toolName + stableStringify(params))`. Tool outcomes are hashed as `SHA256({status, exitCode, text, ...})`. Comparing argument hash + result hash detects no-progress repeats (same call, same result).

**Recording**:
- `recordToolCall()`: adds to sliding window (default 30 calls)
- `recordToolCallOutcome()`: annotates call with result hash after execution
- `getToolCallStats()`: returns totalCalls, uniquePatterns, mostFrequent

**Actions**: at WARNING level, inject a diagnostic hint to the model suggesting it stop the pattern. At CRITICAL level, abort the turn entirely.

### 5. Tool Policy (`tool-policy.ts`)

**Three policy mechanisms:**

**Owner-only tools**: restricted to the message originator (verified sender):
- `control_plane` class: cron, gateway management
- `exec_capable` class: node execution
- `interactive` class: login, pairing

**Allowlist / Denylist**:
- `allow?: string[]` — whitelist (all others denied)
- `deny?: string[]` — blacklist (all others allowed)
- Supports tool groups: `group:plugins`, `plugin-id/*`

**Plugin tool groups**:
- `buildPluginToolGroups()` maps tools to their owning plugins
- `expandPluginGroups()` resolves group aliases like `group:plugins` → all plugin tools

**Composition**: `mergeAlsoAllowPolicy()` combines multiple allowlists. Policies are expanded, filtered, and applied before tools are passed to the model.

### 6. Follow-Up System

After a turn completes, OpenClaw can queue follow-up runs:

**`enqueueFollowupRun()`**: adds a follow-up to the queue with delay, trigger reason, and context
**`refreshQueuedFollowupSession()`**: updates queued follow-ups with latest session state
**`createFollowupRunner()`**: produces a runner that executes queued follow-ups

Use cases: heartbeat checks, scheduled reminders, multi-step workflows where the agent needs to "come back" after waiting.

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| Fallback candidate list | Model failover order |
| Tool policy (config) | Allow/deny lists per session or agent |
| Owner-only tool classes | Which tools require sender verification |
| Tool loop detection config | Thresholds for warning/critical/circuit-breaker |
| Follow-up queue settings | Delay, max queue depth |
| Block streaming config | Chunked reply delivery behavior |

## Design Decisions

- **Three-layer runner architecture** — The separation between `runReplyAgent` (lifecycle), `runAgentTurnWithFallback` (error recovery), and `runEmbeddedPiAgent` (inference) keeps each layer focused. Lifecycle concerns (typing, delivery) don't leak into inference. Error recovery doesn't leak into tool execution.
- **Model fallback with sticky state** — When a model fails, the fallback is persisted to the session entry. This means the next message uses the fallback model without retrying the failed primary first. On success, overrides are cleared — a self-healing pattern.
- **Tool loop detection as a first-class concern** — Unlike Claude Code (which doesn't have explicit loop detection) or Codex (which relies on the model's judgment), OpenClaw actively monitors tool call patterns and intervenes. This is particularly important for the gateway's always-on use case where a stuck agent could waste resources indefinitely.
- **Plugin-based tool policies** — Tool access isn't just allow/deny — it's scoped to plugin groups. A plugin can register tools that are only available to authorized senders, and the policy system expands group aliases automatically. This scales to OpenClaw's 130+ extensions.
- **Channel-agnostic delivery** — The runner doesn't know which channel (Telegram, Discord, web, voice) originated the message. Reply delivery is handled by the block-reply pipeline, which adapts formatting (markdown stripping, media path normalization) per channel capability.

## See Also

- [OpenClaw — Prompt System](references/code-analysis/prompts/openclaw-prompt-system.md) — How the system prompt is assembled for the Pi runner
- [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md) — Preflight compaction and memory flush
- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Design rationale from Peter Steinberger ("closing the loop")

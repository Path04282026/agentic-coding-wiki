---
tags: [code-analysis]
product: OpenClaw
pillar: context
status: complete
created: 2026-04-12
key_files:
  - code/openclaw/src/agents/context.ts
  - code/openclaw/src/agents/defaults.ts
  - code/openclaw/src/auto-reply/reply/agent-runner-memory.ts
  - code/openclaw/src/auto-reply/reply/post-compaction-context.ts
  - code/openclaw/src/plugins/memory-state.ts
  - code/openclaw/src/config/sessions.ts
---

# OpenClaw — Context Management

## Purpose

OpenClaw manages context through a **pre-turn/post-turn compaction** strategy driven by plugin-based memory. Before each turn, a preflight check projects whether the upcoming turn will overflow the context window; after each turn, a memory flush checks whether persistent memory should be consolidated. Session tokens are tracked per-entry with a staleness flag, and post-compaction context is refreshed from workspace `AGENTS.md` sections.

## Architecture

```
Incoming message
        │
        ▼
runPreflightCompactionIfNeeded()         ← agent-runner-memory.ts
  │
  ├─ Read last usage from session transcript (backward file scan)
  ├─ Project: base_prompt + last_output + estimate
  ├─ If projected > contextWindow - reserve - softThreshold:
  │   └─ compactEmbeddedPiSession(trigger: "budget")
  │       └─ Injects post-compaction context from AGENTS.md
  │
  ▼
runAgentTurnWithFallback()               ← Main inference (see Agent Loop page)
        │
        ▼
runMemoryFlushIfNeeded()                 ← agent-runner-memory.ts
  │
  ├─ Read latest transcript for actual token count
  ├─ Project next input: prompt + lastOutput + nextEstimate
  ├─ If projected > threshold OR transcript > forceFlushSize:
  │   └─ compactEmbeddedPiSession(trigger: "memory", silentExpected: true)
  │       └─ Uses resolveMemoryFlushPlan() for prompt + config
  │
  ▼
Update session entry (totalTokens, compactionCount)
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `agents/context.ts` | Context token resolution — config, discovery, model-specific | 470 |
| `agents/defaults.ts` | `DEFAULT_CONTEXT_TOKENS = 200_000` | 6 |
| `auto-reply/reply/agent-runner-memory.ts` | Pre-turn compaction + post-turn memory flush | 833 |
| `auto-reply/reply/post-compaction-context.ts` | Reads AGENTS.md sections to refresh agent state | 236 |
| `plugins/memory-state.ts` | Plugin-based memory registration and prompt building | 327 |
| `config/sessions.ts` | Session entry persistence (tokens, compaction count, overrides) | barrel |

## Implementation Walkthrough

### 1. Context Token Resolution (`context.ts`)

**`resolveContextTokensForModel(params)`** — resolution chain:
1. Explicit `contextTokensOverride` (if provided)
2. `context1m` flag for Anthropic 1M models → `ANTHROPIC_CONTEXT_1M_TOKENS` (1,048,576)
3. Configured per-provider/model: `cfg.models.providers[provider][model].contextTokens`
4. Qualified cache key: `provider/model`
5. Bare model ID cache
6. Fallback `contextTokens` parameter or `DEFAULT_CONTEXT_TOKENS` (200,000)

**1M model detection**: `ANTHROPIC_1M_MODEL_PREFIXES = ["claude-opus-4", "claude-sonnet-4"]` — any model starting with these prefixes gets 1M context when `context1m: true`.

**Discovery pipeline**: OpenClaw lazy-loads model metadata from `models-config.runtime.js`, scanning `models.json` for context window sizes. Results are cached per `provider/model` key. CLI commands trigger eager warmup; library imports skip it.

### 2. Session Tracking (`config/sessions.ts`)

Each session entry tracks:

| Field | Purpose |
|-------|---------|
| `sessionId` | Current session identifier (changes on compaction) |
| `totalTokens` | Last known token count |
| `totalTokensFresh` | Whether `totalTokens` came from the latest turn (vs stale estimate) |
| `compactionCount` | How many times this session has been compacted |
| `memoryFlushCompactionCount` | Compactions triggered by memory flush specifically |
| `providerOverride`, `modelOverride` | Sticky fallback model (from failed inference) |
| `authProfileOverride` | Sticky auth profile override |
| `skillsSnapshot` | Skills state at last update |
| `systemPromptReport` | Diagnostic snapshot of last prompt assembly |

**`totalTokensFresh`** is the key staleness indicator. After a turn completes, it's set to `true` with the actual token count. Between turns (or after crash recovery), it may be `false`, forcing the system to read the transcript file backward to discover the real count.

### 3. Pre-Turn Compaction (`runPreflightCompactionIfNeeded`)

Called BEFORE the main agent turn to prevent context overflow mid-inference.

**Decision logic**:
1. Check `totalTokensFresh` — if stale, must read transcript
2. Read last non-zero usage from session transcript (backward file scan with `TRANSCRIPT_OUTPUT_READ_BUFFER_TOKENS = 8,192`)
3. Estimate incoming prompt tokens from the new message
4. Project: `base_prompt + last_output + estimate`
5. If projected > `contextWindow - reserveTokensFloor - softThresholdTokens`:
   - Trigger `compactEmbeddedPiSession(trigger: "budget")`
   - On success: increment `compactionCount`, update `sessionId`
   - Inject `readPostCompactionContext()` for state refresh

**Reserve constants** (from memory flush plan):
- `reserveTokensFloor`: hard floor to keep free (default 20,000)
- `softThresholdTokens`: additional buffer (default 4,000)

### 4. Post-Turn Memory Flush (`runMemoryFlushIfNeeded`)

Called AFTER the agent turn to consolidate memory if context is still tight.

**Decision logic** (more complex than preflight):
1. Check if memory flush plan is configured (`resolveMemoryFlushPlan()`)
2. Validate sandbox write permissions (can't flush if read-only)
3. Decide whether to read transcript:
   - Always if `totalTokens` is stale
   - Conditionally if near threshold (projected >= threshold - buffer)
4. Derive actual output tokens from last transcript line
5. If transcript shows higher `totalTokens` than session entry, persist the higher value
6. Project next input: `promptTokens + lastOutputTokens + estimatedNextPrompt`
7. Trigger flush if:
   - Projected > threshold, OR
   - Transcript bytes > `forceFlushTranscriptBytes` (size-based trigger)

**Memory flush execution**:
- Runs embedded Pi agent with `trigger: "memory"`, `silentExpected: true`
- Uses `resolveMemoryFlushPlan()` for the compaction prompt + system prompt
- Post-compaction: inject context refresh via `readPostCompactionContext()`

### 5. Post-Compaction Context Refresh (`post-compaction-context.ts`)

After compaction, the agent loses awareness of workspace state. `readPostCompactionContext()` reinjects critical sections:

**Section discovery** from workspace `AGENTS.md`:
- Default sections: "Session Startup", "Red Lines"
- Legacy fallback: "Every Session", "Safety"
- Configurable via `cfg.agents.defaults.compaction.postCompactionSections`

**Processing**:
- Extracts H2/H3 headings (case-insensitive) and content until next heading
- Substitutes `YYYY-MM-DD` with actual runtime date (so the agent reads correct daily files)
- Appends cron-style timestamp
- Truncates to 3,000 chars max
- Adds note: "This is NOT a substitute for full manual startup"

### 6. Memory Plugin Architecture (`memory-state.ts`)

OpenClaw's memory system is **plugin-based** — memory capabilities are registered by plugins rather than hard-coded.

**Registration**:
```typescript
registerMemoryCapability(pluginId: string, capability: MemoryPluginCapability)
```

**`MemoryPluginCapability`** interface:
- `promptBuilder`: builds the memory recall section for the system prompt
- `flushPlanResolver`: returns `MemoryFlushPlan` (prompt, thresholds, reserve)
- `runtime`: memory search/get operations
- `publicArtifacts`: files to expose in the vault

**`buildMemoryPromptSection(params)`** — assembles the memory section:
1. Get primary prompt from the registered capability's `promptBuilder`
2. Collect supplements from all registered plugins (sorted by pluginId for determinism)
3. Combine into a single memory prompt section

**`resolveMemoryFlushPlan(cfg?, nowMs?)`** — returns:

| Field | Purpose |
|-------|---------|
| `prompt` | The instruction prompt for the memory agent |
| `systemPrompt` | System prompt override for the memory agent |
| `relativePath` | Where memory files are stored |
| `softThresholdTokens` | Buffer before triggering flush (default 4,000) |
| `forceFlushTranscriptBytes` | Size-based trigger for flush |
| `reserveTokensFloor` | Hard minimum free space (default 20,000) |

**State management**: single global `memoryPluginState` (module-scoped), with `restoreMemoryPluginState()` / `clearMemoryPluginState()` for testing. Supports legacy split registration for backward compatibility.

### 7. Heartbeat as Dynamic Context

The `heartbeat.md` workspace file is the only file marked as dynamic context (via `DYNAMIC_CONTEXT_FILE_BASENAMES` in `system-prompt.ts`). It:
- Is injected BELOW the cache boundary (recomputed per turn)
- Drives periodic agent check-ins (email, calendar, mentions)
- Content is sanitized to avoid subscription policy conflicts
- Changes between turns without breaking prompt cache for stable sections

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| `agents.defaults.compaction.postCompactionSections` | Which AGENTS.md sections to reinject after compaction |
| `cfg.models.providers[p][m].contextTokens` | Per-model context window override |
| `context1m` flag | Enable 1M context for Anthropic models |
| Memory plugin registration | Which plugin provides memory flush logic |
| `MemoryFlushPlan` thresholds | soft threshold, reserve floor, force-flush size |
| `totalTokensFresh` session flag | Controls whether transcript must be re-read |

## Design Decisions

- **Pre-turn + post-turn compaction vs single trigger** — Unlike Claude Code (which triggers mid-turn or between turns via auto-compact), OpenClaw separates the concerns: preflight prevents overflow, memory flush optimizes after the fact. This avoids mid-inference compaction which could disrupt streaming.
- **Transcript-based token recovery** — When `totalTokensFresh` is stale (crash, restart), OpenClaw reads the session transcript file backward to find the last usage record. This is more robust than trusting an in-memory counter that might be lost.
- **Plugin-based memory architecture** — Memory is not hard-coded into the core. Plugins register `MemoryPluginCapability` with their own prompt builders and flush plans. This allows different memory strategies per deployment without changing core code.
- **Post-compaction AGENTS.md injection** — Rather than re-reading the entire workspace, only "Session Startup" and "Red Lines" sections are injected (configurable). This is a targeted refresh — the agent gets critical behavioral reminders without bloating the compacted context.
- **Size-based flush trigger** — `forceFlushTranscriptBytes` triggers memory flush based on raw transcript file size, not just token count. Large transcripts (many tool calls, verbose outputs) need flushing even if token projections are within budget.

## See Also

- [OpenClaw — Prompt System](references/code-analysis/prompts/openclaw-prompt-system.md) — How context files and cache boundary interact with prompt assembly
- [OpenClaw — Agent Loop](references/code-analysis/loops/openclaw-agent-loop.md) — Where preflight compaction and memory flush are called
- [04 - Context Management](references/dimensions/04-context-management.md) — Design rationale from interviews (Peter on self-aware agents)

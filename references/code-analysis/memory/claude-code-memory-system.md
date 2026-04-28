---
tags: [code-analysis]
product: Claude Code
pillar: memory
status: complete
created: 2026-04-12
key_files:
  - code/ClaudCode/services/SessionMemory/sessionMemory.ts
  - code/ClaudCode/services/SessionMemory/prompts.ts
  - code/ClaudCode/services/extractMemories/extractMemories.ts
  - code/ClaudCode/services/extractMemories/prompts.ts
  - code/ClaudCode/services/autoDream/autoDream.ts
  - code/ClaudCode/memdir/memdir.ts
  - code/ClaudCode/memdir/memoryTypes.ts
  - code/ClaudCode/utils/memory/types.ts
---

# Claude Code — Memory System

## Purpose

Claude Code has **three distinct memory subsystems** that operate at different timescales: **Session Memory** maintains a live notes file during the current session (used by compaction to preserve state); **Auto Memory Extraction** captures user preferences and project facts to persistent files after each turn; and the **Dream System** consolidates those append-only extraction logs into a curated knowledge base across sessions.

## Architecture

```
During session (every model response)
┌──────────────────────────────────────┐
│  SESSION MEMORY                       │
│  sessionMemory.ts                     │
│  Trigger: post-sampling hook          │
│  Output: .session-memory/notes.md     │
│  Purpose: preserve state for compact  │
│  Cap: 12,000 tokens across 9 sections │
└──────────────┬───────────────────────┘
               │ Feeds into compaction prompt
               ▼

After each turn (query loop completes)
┌──────────────────────────────────────┐
│  AUTO MEMORY EXTRACTION               │
│  extractMemories.ts                   │
│  Trigger: stop hook                   │
│  Output: ~/.claude/projects/*/memory/ │
│    ├── MEMORY.md (index, 200 lines)   │
│    ├── user_role.md                   │
│    ├── feedback_testing.md            │
│    └── project_deadline.md            │
│  Purpose: persist preferences/facts   │
│  Cap: 5 turns, memory dir only        │
└──────────────┬───────────────────────┘
               │ Accumulated across sessions
               ▼

Nightly / on demand
┌──────────────────────────────────────┐
│  DREAM SYSTEM                         │
│  autoDream.ts                         │
│  Trigger: 24h + 5 sessions threshold  │
│  Input: session transcripts + memory  │
│  Output: consolidated MEMORY.md       │
│  Purpose: synthesis & pruning         │
│  Lock: mtime-based exclusive access   │
└──────────────────────────────────────┘
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `services/SessionMemory/sessionMemory.ts` | Live session notes extraction and management | 496 |
| `services/SessionMemory/prompts.ts` | Session memory templates (9 sections) | 325 |
| `services/extractMemories/extractMemories.ts` | Per-turn background memory extraction | 616 |
| `services/extractMemories/prompts.ts` | Extraction agent prompts (see [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)) | 155 |
| `services/autoDream/autoDream.ts` | Nightly consolidation orchestrator | 325 |
| `services/autoDream/consolidationPrompt.ts` | Consolidation agent prompt | varies |
| `services/autoDream/consolidationLock.ts` | mtime-based exclusive lock | varies |
| `memdir/memdir.ts` | Memory directory loading + truncation | 508 |
| `memdir/memoryTypes.ts` | 4-type taxonomy with frontmatter schema | 272 |
| `utils/memory/types.ts` | `MemoryType` enum (path resolution) | 13 |

## Implementation Walkthrough

### 1. Session Memory — Live State During a Session

**Purpose**: maintains a `.session-memory/notes.md` file with up-to-date session state so that compaction can preserve continuity.

**Location**: `~/.claude/session-memory/<session-id>/notes.md`

**Trigger**: `registerPostSamplingHook(extractSessionMemory)` — fires after every model response. Gated by `tengu_session_memory` feature flag.

**Extraction thresholds** (from `SessionMemoryConfig`):
- `minimumMessageTokensToInit`: 10,000 tokens (first extraction after session reaches this)
- `minimumTokensBetweenUpdate`: 5,000 tokens (minimum growth since last extraction)
- `toolCallsBetweenUpdates`: 3 tool calls minimum between updates

**Notes structure** — 9 standardized sections:
1. Session Title
2. Current State
3. Task Specification
4. Files and Functions
5. Workflow
6. Errors & Corrections
7. Codebase and System Documentation
8. Learnings
9. Key Results / Worklog

Each section has an italic description line that is **never modified** — only the content below it is updated. Each section is capped at ~2,000 tokens; total cap is 12,000 tokens.

**Execution**:
- Runs as a forked agent (`runForkedAgent`) with limited tool access: File Edit only, restricted to the session memory file
- Wrapped in `sequential()` to prevent overlapping extractions
- Shares parent's prompt cache via `createCacheSafeParams()`
- Integrated with compaction: session notes are injected into compact messages so the summary knows what state to preserve

### 2. Auto Memory Extraction — Persistent Facts After Each Turn

**Purpose**: captures user preferences, project context, and feedback to persistent files that survive across sessions.

**Location**: `~/.claude/projects/<project-slug>/memory/`
```
memory/
  MEMORY.md          ← Index (max 200 lines, 25KB)
  user_role.md       ← Individual topic files
  feedback_testing.md
  project_deadline.md
  team/              ← Team memory (if TEAMMEM enabled)
    MEMORY.md
    ...
```

**Trigger**: `executeExtractMemories()` fires as a stop hook after the model produces a final response with no tool calls. Gated by `tengu_passport_quail` + `isAutoMemoryEnabled()`.

**4-type memory taxonomy** (from `memdir/memoryTypes.ts`):

| Type | Purpose | When to Save | Storage |
|------|---------|-------------|---------|
| `user` | Role, preferences, knowledge | Profile details emerge | Private |
| `feedback` | What to do / not do, with reasons | Corrections OR confirmations | Private (team for project-wide) |
| `project` | Goals, deadlines, decisions not in code | Who/what/when/why learned | Bias team |
| `reference` | Pointers to external systems | Repeated external references | Usually team |

Each file requires YAML frontmatter:
```yaml
---
name: User is a data scientist
description: User role and current focus area
type: user
---

User is a data scientist focused on observability/logging...
```

**`feedback` and `project` types** use structured bodies:
- Rule/fact statement
- **Why:** — the reason (past incident, preference, constraint)
- **How to apply:** — when/where this guidance kicks in

**Execution**:
- Runs as a forked agent with restricted tools:
  - Full access: Read, Grep, Glob
  - Read-only Bash: ls, find, grep, cat, stat, wc, head, tail
  - Write access: Edit + Write for memory directory only
- Pre-injects existing memory manifest (avoids spending a turn on `ls`)
- **Efficiency strategy**: turn 1 = all reads in parallel, turn 2 = all writes in parallel
- Hard cap: 5 turns max
- **Mutual exclusion**: if the main agent already wrote to memory (via system prompt instructions), the forked extraction is skipped (`hasMemoryWritesSince()`)
- **Coalescing**: concurrent extraction calls stash context; only the latest runs to avoid thrashing
- No transcript recording (`skipTranscript: true`)

**What NOT to save** (enforced by prompt):
- Code patterns, architecture, file paths (derivable from code)
- Git history (canonical source: `git log`)
- Debugging solutions (the fix is in the commit)
- CLAUDE.md content (already documented)
- Ephemeral task details (use tasks/plans instead)

### 3. Dream System — Multi-Session Consolidation

**Purpose**: distills append-only extraction logs across multiple sessions into a curated `MEMORY.md` index + topic files. Prunes stale entries, merges related facts, resolves contradictions.

**Trigger thresholds** (from `tengu_onyx_plover` config):
- `minHours`: 24 hours since last consolidation
- `minSessions`: 5 sessions modified since last consolidation
- Scan throttle: 10-minute interval to avoid expensive session-count lookups
- Manual trigger: `/dream` skill command

**Execution**:
1. List all transcript files modified since `lastConsolidatedAt`
2. Build prompt with session list + existing memory manifest
3. Run forked agent with `createAutoMemCanUseTool()` (same restricted tool set as extraction)
4. Agent reviews logs, consolidates into topic files, updates `MEMORY.md` index
5. Update lock mtime on success; rollback on failure

**Lock mechanism** (`consolidationLock.ts`):
- Uses file mtime as a lock signal — prevents concurrent dream runs
- If interrupted: rollback reverts lock to prior mtime, next turn reschedules
- Simple but effective for single-machine operation

**UI integration**:
- Creates `DreamTaskState` for visibility (footer pill, Shift+Down dialog)
- Phase tracking: 'starting' → 'updating' (flips when first Edit/Write tool_use appears)
- Progress: collapses tool_use blocks to counts; collects touched file paths

### 4. Memory Directory Loading (`memdir/memdir.ts`)

**`buildMemoryPrompt()`** loads memory for injection into the system prompt:

**Truncation rules** (MEMORY.md):
- Line truncation: first 200 lines kept
- Byte truncation: first 25KB kept, cut at last newline
- Warning appended if truncated: "Lines after 200 will be truncated, so keep the index concise"

**Memory file validation**:
- Frontmatter must include `name`, `description`, `type`
- Type must be one of: `user`, `feedback`, `project`, `reference`
- Files without valid frontmatter are still loaded but won't match Dataview queries

### 5. Memory Paths (`utils/memory/types.ts` + `utils/config.ts`)

Two different `MemoryType` systems coexist:

**Path-level types** (config.ts `getMemoryPath()`):

| MemoryType | Path |
|------------|------|
| `User` | `~/.claude/CLAUDE.md` |
| `Project` | `{cwd}/CLAUDE.md` |
| `Local` | `{cwd}/CLAUDE.local.md` |
| `Managed` | `~/.claude/managed/CLAUDE.md` |
| `AutoMem` | `~/.claude/projects/{slug}/memory/MEMORY.md` |
| `TeamMem` | `~/.claude/projects/{slug}/memory/team/MEMORY.md` |

**Content-level types** (memdir/memoryTypes.ts): `user`, `feedback`, `project`, `reference` — these are the frontmatter `type` values within individual memory files.

## How Memories Flow Through the System

```
Turn 1: User says "I'm a data scientist focused on logging"
  │
  ├─ Model responds normally
  │
  ├─ Post-sampling hook → Session Memory
  │   └─ Updates notes.md: "Task Specification: investigate logging"
  │
  └─ Stop hook → Auto Extraction
      └─ Forked agent creates:
          memory/user_role.md (type: user)
          memory/MEMORY.md: "- [User Role](user_role.md) — data scientist, logging focus"

Turn 5: User says "don't mock the database in tests"
  │
  └─ Stop hook → Auto Extraction
      └─ Forked agent creates:
          memory/feedback_testing.md (type: feedback)
          "Integration tests must hit a real database.
           **Why:** Prior incident where mock/prod divergence masked a broken migration.
           **How to apply:** When writing or reviewing test code in this project."

24 hours + 5 sessions later:
  │
  └─ Dream System fires
      └─ Reviews all transcripts since last dream
      └─ Consolidates: merges stale entries, prunes outdated facts
      └─ Updates MEMORY.md index with cleaner organization

Next session:
  │
  └─ System prompt loads memory/MEMORY.md (≤200 lines)
      └─ Model has persistent context about user preferences, project state
```

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| `tengu_session_memory` feature flag | Enable/disable session memory |
| `SessionMemoryConfig` | Token thresholds, tool call minimum |
| `tengu_passport_quail` feature flag | Enable/disable auto extraction |
| `tengu_bramble_lintel` | Extraction frequency throttle |
| `tengu_onyx_plover` config | Dream thresholds (hours, sessions) |
| `TEAMMEM` feature flag | Enable team memory directory |
| `isAutoMemoryEnabled()` | User config check |
| `createAutoMemCanUseTool()` | Tool restrictions for forked agents |

## Design Decisions

- **Three timescales for three purposes** — Session memory (seconds) preserves state for compaction. Extraction (minutes) captures durable facts. Dream (days) curates and prunes. Each operates independently; failure in one doesn't affect the others.
- **Forked agents share prompt cache** — All three memory systems use `runForkedAgent` with `createCacheSafeParams()`. This means the memory agent piggybacks on the main thread's cached system prompt, avoiding expensive cache creation for what are essentially background tasks.
- **Mutual exclusion between main agent and extraction** — If the user asks "remember this" and the main agent writes to memory directly, the background extraction skips that turn. This prevents the two from writing conflicting entries to the same files.
- **Append-only extraction + periodic consolidation** — Extraction always adds; Dream consolidates. This avoids the complexity of real-time deduplication in the extraction agent (which runs under tight turn limits). Dream has more time and context to merge and prune.
- **Structured types with "why" and "how to apply"** — Feedback and project memories require explicit reasoning. This isn't just documentation — it helps the model decide when a memory applies to a new situation, and helps Dream decide what's still relevant.
- **Hard caps prevent memory bloat** — MEMORY.md capped at 200 lines / 25KB. Session notes capped at 12K tokens across 9 sections. Extraction capped at 5 turns. These constraints force the system to be selective rather than hoarding everything.

## See Also

- [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) — Memory extraction prompts and how memory is injected into the system prompt
- [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) — How session memory feeds into compaction
- [Claude Code — Agent Loop](references/code-analysis/loops/claude-code-agent-loop.md) — Where post-sampling hooks and stop hooks fire
- [04 - Context Management](references/dimensions/04-context-management.md) — Design rationale from Boris Cherny on CLAUDE.md origins

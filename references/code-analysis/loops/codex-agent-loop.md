---
tags: [code-analysis]
product: Codex
pillar: loops
status: complete
created: 2026-04-12
key_files:
  - code/codex/codex-rs/core/src/codex.rs
  - code/codex/codex-rs/core/src/codex_thread.rs
  - code/codex/codex-rs/core/src/tasks/mod.rs
  - code/codex/codex-rs/core/src/tasks/regular.rs
  - code/codex/codex-rs/core/src/tools/spec.rs
  - code/codex/codex-rs/core/src/tools/runtimes/shell.rs
  - code/codex/codex-rs/core/src/tools/handlers/agent_jobs.rs
---

# Codex — Agent Loop

## Purpose

Codex's agent loop is an **event-driven submission loop** built on Rust's async channels. The outer `Codex` struct exposes `submit()` / `next_event()` as a bidirectional stream. Internally, `submission_loop()` dispatches operations (user input, approvals, compaction, shutdown) to task-specific handlers. Regular turns call the model, extract tool calls, route them through the `ToolRouter`, and loop until the model stops requesting tools.

## Architecture

```
Client (CLI / App Server / IDE Extension)
  │
  ├─ submit(Op::UserInput{...}) ──────► tx_sub channel
  │                                          │
  │                                          ▼
  │                                    submission_loop()     ← codex.rs
  │                                      │
  │                                      ├─ Match Op variant:
  │                                      │   ├─ UserInput/UserTurn → run_turn()
  │                                      │   ├─ ExecApproval      → approve tool
  │                                      │   ├─ PatchApproval     → approve patch
  │                                      │   ├─ Compact           → CompactTask
  │                                      │   ├─ Interrupt         → cancel active turn
  │                                      │   └─ Shutdown          → exit loop
  │                                      │
  │                                      ▼
  │                                    RegularTask.run()     ← tasks/regular.rs
  │                                      │
  │                                      ├─ Emit TurnStarted event
  │                                      ├─ Consume prewarmed client session
  │                                      │
  │                                      ▼
  │                                    run_turn()            ← codex.rs
  │                                      │
  │                                      ├─ Build Prompt (system + history)
  │                                      ├─ Stream via client_session.stream()
  │                                      │
  │                                      ├─ For each ResponseEvent:
  │                                      │   ├─ OutputItemDone → extract tool calls
  │                                      │   ├─ ToolCall → ToolRouter dispatch
  │                                      │   │   ├─ ShellHandler
  │                                      │   │   ├─ ApplyPatchHandler
  │                                      │   │   ├─ SpawnAgentHandler
  │                                      │   │   ├─ McpHandler
  │                                      │   │   └─ (20+ more handlers)
  │                                      │   ├─ FunctionCallOutput → feed back
  │                                      │   └─ Completed → check pending input
  │                                      │
  │                                      └─ Loop if has_pending_input()
  │
  ◄── rx_event channel ────────────── Event stream (SSE/WebSocket)
```

## Key Files

| File | Role | Lines |
|------|------|-------|
| `codex.rs` | Core `Codex` and `Session` structs, submission loop, turn execution | 7,939 |
| `codex_thread.rs` | Public API wrapper: `submit()`, `steer_input()`, `shutdown_and_wait()` | 278 |
| `tasks/mod.rs` | Task type taxonomy and `SessionTask` trait | 23,098 |
| `tasks/regular.rs` | Regular turn implementation (model call + tool loop) | 84 |
| `tools/spec.rs` | Tool specification format and registry builder | 150+ |
| `tools/runtimes/shell.rs` | Shell command execution with sandbox + approval | 150+ |
| `tools/handlers/agent_jobs.rs` | Batch sub-agent spawning (CSV-driven) | 150+ |

## Implementation Walkthrough

### 1. The Codex Struct and Session

**`Codex`** is the public handle:
```rust
struct Codex {
    tx_sub: Sender<Submission>,         // Input: send operations
    rx_event: Receiver<Event>,          // Output: receive events
    agent_status: watch::Receiver<AgentStatus>,
    session: Arc<Session>,
    session_loop_termination: SessionLoopTermination,
}
```

**`Session`** holds all shared state:
```rust
struct Session {
    conversation_id: ThreadId,
    tx_event: Sender<Event>,             // Broadcasts events to consumers
    state: Mutex<SessionState>,          // Configuration + history
    active_turn: Mutex<Option<ActiveTurn>>,
    mailbox: Mailbox,                    // Inter-agent message queue
    mailbox_rx: Mutex<MailboxReceiver>,
    idle_pending_input: Mutex<Vec<ResponseInputItem>>,
    guardian_review_session: GuardianReviewSessionManager,
    services: SessionServices,           // Model client, analytics
    js_repl: Arc<JsReplHandle>,          // JavaScript runtime
    // ... more fields
}
```

**`TurnContext`** is created per-turn with 40+ fields capturing model info, security policies, tool config, timing, and execution constraints.

### 2. The Public API (`codex_thread.rs`)

`CodexThread` provides a thin wrapper for external consumers:

**`submit(op)`**: wraps an `Op` enum in a `Submission` with a UUID v7 ID, sends it to the `tx_sub` channel. Returns the submission ID.

**`steer_input(input, expected_turn_id)`**: injects input mid-turn during streaming. Validates the expected turn ID matches the active turn. Used for real-time voice/conversation input. Returns error if turn is non-steerable (compact, review).

**`shutdown_and_wait()`**: sends `Op::Shutdown`, waits for the submission loop's termination future.

**`next_event()`**: blocks until the session produces an event on `rx_event`. Returns `CodexErr::InternalAgentDied` if the channel closes.

### 3. The Submission Loop (`codex.rs`)

**`submission_loop(sess, config, rx_sub)`** — the main dispatch:

```rust
while let Ok(sub) = rx_sub.recv().await {
    match sub.op {
        Op::UserInput { .. } | Op::UserTurn { .. } => /* ... */,
        Op::ExecApproval { .. }   => /* approve pending tool */,
        Op::PatchApproval { .. }  => /* approve pending patch */,
        Op::Interrupt             => /* cancel active turn */,
        Op::Compact               => /* run CompactTask */,
        Op::Shutdown              => /* exit loop */,
        // 20+ more Op variants...
    }
}
```

Operations include: `UserInput`, `UserTurn`, `ExecApproval`, `PatchApproval`, `McpApproval`, `Interrupt`, `Compact`, `Shutdown`, `SteerInput`, `UpdateConfig`, `SubAgentNotification`, and more.

### 4. Task Types (`tasks/mod.rs`)

The `SessionTask` trait defines the interface:
```rust
trait SessionTask {
    fn kind(&self) -> TaskKind;
    fn span_name(&self) -> &'static str;
    async fn run(self: Arc<Self>, session, ctx, input, cancel) -> Option<String>;
}
```

| Task | Kind | Purpose |
|------|------|---------|
| `RegularTask` | Regular | Standard user turn — model call + tool loop |
| `CompactTask` | Compact | Context compaction (see [Codex — Context Management](references/code-analysis/context/codex-context-management.md)) |
| `ReviewTask` | Review | Guardian code review with approval gate |
| `UndoTask` | Undo | Rollback last turn (revert history) |
| `UserShellCommandTask` | UserShell | User-initiated shell command |
| `GhostSnapshotTask` | GhostSnapshot | Internal snapshot for fork points |

### 5. Regular Turn Execution (`tasks/regular.rs`)

The `RegularTask` is the standard user-turn handler (84 lines):

1. **Emit `TurnStarted`** with turn ID, timestamp, model context window, collaboration mode
2. **Reset server reasoning** flag (fresh turn)
3. **Consume prewarmed client session** — startup optimization: if the model client was prewarmed during initialization, reuse it for the first turn to skip cold-start latency
4. **Inner loop**:
   ```rust
   loop {
       let last_message = run_turn(sess, ctx, next_input, prewarmed, cancel).await;
       if !sess.has_pending_input().await {
           return last_message;  // Done
       }
       next_input = Vec::new();  // Process queued steer_input
   }
   ```
   The loop continues while `has_pending_input()` — this handles `steer_input()` calls that arrive mid-turn.

### 6. The Turn Engine (`run_turn` in `codex.rs`)

`run_turn()` is the model-call + tool-execution cycle within a single turn:

1. **Record context updates** — environment diffs, reference context changes
2. **Collect skills/plugins/connectors** for injection into the prompt
3. **Build `Prompt` struct**: system prompt (from template files) + conversation history + personality + developer instructions
4. **Stream model response** via `client_session.stream()` (calls the OpenAI API)
5. **Process `ResponseEvent` items**:
   - `OutputItemDone` → check for tool calls
   - Tool calls extracted, routed through `ToolRouter` to the appropriate handler
   - Handler executes, returns `FunctionCallOutput`
   - Output fed back as `tool_result` for the next model call
6. **Compaction check**: if `ContextWindowExceeded` during streaming, trigger inline auto-compaction (see [Codex — Context Management](references/code-analysis/context/codex-context-management.md))
7. **Repeat** until model produces a final response with no pending tool calls

### 7. Tool Registry and Handlers

**`build_specs_with_discoverable_tools()`** builds the tool registry mapping names to handlers:

| Handler | Tool | Function |
|---------|------|----------|
| `ShellHandler` | `shell` / `exec` | Execute commands in sandbox |
| `ApplyPatchHandler` | `apply_patch` | Apply unified diffs to files |
| `PlanHandler` | `plan` | Multi-step planning |
| `SpawnAgentHandler` | `spawn_agent` | Create sub-agent |
| `WaitAgentHandler` | `wait_agent` | Wait for sub-agent completion |
| `SendMessageHandler` | `send_message` | Inter-agent messaging (mailbox) |
| `McpHandler` | MCP tools | Delegate to MCP servers |
| `ViewImageHandler` | `view_image` | Display images in terminal |
| `DynamicToolHandler` | Custom tools | User-defined tool execution |
| `BatchJobHandler` | `spawn_agents_on_csv` | Batch sub-agent spawning |
| + 10 more | Various | Permissions, resources, JS REPL, etc. |

### 8. Shell Execution (`tools/runtimes/shell.rs`)

The shell handler wraps command execution with approval and sandboxing:

**`ShellRequest`** contains: command, cwd, timeout, env vars, sandbox permissions, approval requirement, justification.

**Three backends** (`ShellRuntimeBackend`):
- `Generic`: standard shell execution flow
- `ShellCommandClassic`: legacy `shell_command` tool compatibility
- `ShellCommandZshFork`: zsh-fork with escalation adapter

**Approval flow** (via `Approvable` trait):
1. Canonicalize command for approval (normalize representation)
2. Check approval cache (same command, cwd, permissions seen before?)
3. If not cached: request approval (user consent or Guardian review)
4. On approval: build sandbox command with transformed permissions
5. Execute, capture output stream, emit events

### 9. Sub-Agent Batch Jobs (`tools/handlers/agent_jobs.rs`)

The `BatchJobHandler` enables batch sub-agent processing:

**Constants**:
- Default concurrency: 16 agents
- Max concurrency: 64 agents
- Status poll: 250ms
- Progress emit: 1 second
- Per-item timeout: 30 minutes

**Workflow** (`spawn_agents_on_csv`):
1. Parse CSV file with row-level items
2. Spawn N sub-agents (capped at concurrency limit)
3. Each agent processes one row with the user's instruction
4. Agent reports back via `report_agent_job_result()`
5. Progress emitted every second: total/pending/running/completed/failed + ETA
6. Job completes when all agents finish or global timeout

### 10. The Mailbox (Inter-Agent Communication)

The `Session.mailbox` enables communication between the main agent and sub-agents:

- Sub-agents can send messages to the main agent's mailbox
- Main agent can send messages to sub-agents via `send_message` tool
- `SubAgentNotification` events trigger when sub-agent status changes
- Mailbox is checked after each turn via `has_pending_input()`

## Configuration Points

| Mechanism | What It Controls |
|-----------|-----------------|
| `Op` enum variants | Full list of supported operations |
| `ToolsConfig` | Which tools are exposed to the model |
| `SandboxPolicy` | Filesystem + network restrictions |
| `AskForApproval` | Auto/ask/deny per tool |
| `CollaborationMode` | Model + reasoning configuration |
| `Personality` | Pragmatic / friendly / custom |
| `DynamicToolSpec` | Custom user-defined tools |

## Design Decisions

- **Channel-based architecture** — `tx_sub` / `rx_event` channels decouple producers (CLI, app server, IDE) from the agent loop. Any client that can send `Op` and receive `Event` can drive Codex. This enables the app server (JSON-RPC) and CLI to share the same core.
- **Task system for polymorphic operations** — Regular turns, compaction, code review, and undo are all `SessionTask` implementations. This keeps the submission loop thin (just dispatch) while allowing each task type to have complex internal logic.
- **Prewarmed client sessions** — The first turn can reuse a model client session that was initialized during startup, eliminating cold-start latency. This is particularly valuable in the app server where sessions persist across IDE actions.
- **`steer_input` for mid-turn injection** — Rather than waiting for a turn to complete, `steer_input()` can inject new user input mid-streaming. This powers real-time conversation features where the user can redirect the model while it's working.
- **Guardian as code review task** — Code review is a first-class task type, not a bolt-on. The `ReviewTask` uses the review prompt (see [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md)) and gates on human approval before applying suggestions.

## See Also

- [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md) — How prompts are selected and assembled
- [Codex — Context Management](references/code-analysis/context/codex-context-management.md) — How compaction integrates as a task type
- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Design rationale from Greg Brockman ("the harness is your body")

---
tags: [source, documentation, runtime-context]
product: Hermes Agent
date: 2026-04-25
dimensions: [01, 02, 03, 04, 05, 06, 07, 08, 09]
raw: current-session-runtime-and-tool-context
---

# Hermes Agent Runtime Capabilities — 2026-04-25

> Source note for the initial [Hermes Agent](references/entities/hermes-agent.md) case study. This page summarizes the runtime/tool context available in the current Hermes Agent session, so later synthesis pages can cite a wiki source page rather than relying on unfiled conversation context.

## Scope and Evidence Boundary

This source page is based on the active Hermes Agent runtime context visible in the current session: platform context, project context, tool definitions, memory/skill behavior, scheduling, delegation, browser/file/terminal actions, and message delivery behavior. It is not a product announcement or external documentation source. Claims using this page should be treated as **runtime-observed capability notes** rather than a complete public product specification.

No files under `raw/` were created or modified for this source because the repository policy says `raw/` is immutable. This page therefore uses `raw: current-session-runtime-and-tool-context` as an explicit provenance marker.

## Key Takeaways by Dimension

### 01 — Agent Loop

Hermes Agent operates as an API-accessed assistant that receives user messages, reasons over developer/system/project context, and can execute tools directly in the same conversational turn. The runtime emphasizes action discipline: when the agent says it will check, read, run, create, or update something, it must immediately make the corresponding tool call. [01 - Agent Loop](references/dimensions/01-agent-loop.md)

The loop is not only chat response generation; it includes prerequisite discovery, tool execution, verification, and final delivery. For repository work, the agent reads governing project context before modifications and keeps working until the task is complete. **Agentic Coding Product Design Questions**

### 02 — Tool Systems

Hermes Agent exposes a broad tool system: file read/search/write/patch, terminal execution, process management, browser interaction, image generation, vision, text-to-speech, scheduled cron jobs, memory, session search, skills, messaging, and delegation. The tool layer is explicitly typed by namespace and schema. [02 - Tool Systems](references/dimensions/02-tool-systems.md)

Important tool-system traits:

- file operations prefer `read_file`, `search_files`, `write_file`, and `patch` over shell equivalents
- terminal is reserved for builds, installs, git, processes, network, package managers, and shell-specific actions
- browser tools support navigation, snapshots, clicks, typing, scrolling, screenshots, and console inspection
- higher-level tools include `cronjob`, `delegate_task`, `memory`, `session_search`, `skill_view`, and `skill_manage`

### 03 — Security and Sandboxing

Hermes Agent relies heavily on policy, scoped tools, working-directory context, and explicit dangerous-action discipline rather than presenting itself as a kernel sandbox. The current wiki project adds a strong local safety invariant: `raw/` is immutable and all generated work belongs in `wiki/`. [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)

The runtime also distinguishes tool capabilities by action and context. Some tools are read-only, while others create files, edit files, run shell commands, schedule jobs, or send messages. This supports a layered safety approach similar to the design pattern “Tool Metadata as Control Plane.” **Agentic Coding Product Design Patterns**

### 04 — Context Management

Hermes Agent receives layered context: system/developer instructions, project context files such as `CLAUDE.md`, current session context, user messages, persistent memory, and loaded skills. For cross-session recall, it has both durable memory and searchable past sessions. [04 - Context Management](references/dimensions/04-context-management.md)

The runtime separates several persistence mechanisms:

- `memory` stores compact durable facts and preferences
- `session_search` recalls prior conversations without putting everything in durable memory
- `skills` store procedural workflows and can be updated when workflows are discovered or corrected
- project context files govern repository-specific behavior

### 05 — Multi-Agent Architecture

Hermes Agent includes `delegate_task`, which spawns isolated subagents with their own context, terminal session, and toolsets. Delegation supports both a single subtask and a batch mode for parallel subtasks. Subagents can be `leaf` workers or, when enabled, bounded `orchestrator` agents. [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)

This positions Hermes close to a built-in orchestration layer: it can divide work across specialized toolsets, preserve the parent context, and receive only final summaries from children to avoid flooding the main context.

### 06 — UI and Distribution

The current session runs in a Feishu/Lark workspace. Hermes Agent can also deliver scheduled-task results back to the origin chat or local files, and the tool context mentions connected messaging delivery behavior. [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)

Hermes therefore appears less like a single CLI-only coding tool and more like a cross-platform action agent that can operate inside chat surfaces while also manipulating local files, browsers, shells, and external message delivery.

### 07 — Feature Gating and Internals

Tool availability can be shaped by enabled toolsets, project workdir, model override for scheduled jobs, and skill loading. Delegated agents can receive restricted toolsets, and cron jobs run in fresh sessions with self-contained prompts. [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)

Skills act as a procedural knowledge layer that can be loaded, patched, created, or deleted. This makes Hermes behavior partly configurable through skill modules rather than only through core runtime changes.

### 08 — Shared Principles

Hermes shares several industry patterns captured by the existing wiki: tool-first operation, layered context, subagent delegation, skills as reusable procedures, messaging surfaces, and source-backed knowledge maintenance. [08 - Shared Principles](references/dimensions/08-shared-principles.md)

A particularly Hermes-specific principle is “act, don’t describe”: the agent is expected to perform real operations through tools and verify results before finalizing.

### 09 — Philosophical Divergence

Hermes Agent’s initial positioning is best described as an **agent operating layer**: not only a coding assistant, but a cross-platform executor that can read/write files, run commands, browse websites, manage schedules, remember stable facts, load skills, delegate subtasks, and communicate through chat surfaces. [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

Relative to the three products, Hermes currently looks closest to a synthesis of:

- Claude Code’s tool-rich, skill-oriented, action-first loop
- Codex’s explicit harness/tool discipline and verification emphasis
- OpenClaw’s messaging-surface orientation and “agent as operator” feel

## Related Pages

- [Hermes Agent](references/entities/hermes-agent.md)
- **Hermes Agent Case Study**
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
- **Agentic Coding Product Design Questions**
- **Agentic Coding Product Design Patterns**

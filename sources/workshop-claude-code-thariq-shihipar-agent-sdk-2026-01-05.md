---
tags: [source, workshop]
product: Claude Code
date: 2026-01-05
dimensions: [01, 02, 03, 04, 05]
speaker: Thariq Shihipar
---

# Workshop — Claude Code — Thariq Shihipar: Claude Agent SDK (2026-01-05)

**Format:** Full technical workshop / masterclass
**Speaker:** Thariq Shihipar — Anthropic
**Video:** [YouTube](https://www.youtube.com/watch?v=TqC1qOfiVcQ)
**Official Docs:** [Agent SDK Overview](https://platform.claude.com/docs/en/agent-sdk/overview)
**Raw transcript:** `sources/interviews/claude-code-20260105-thariq-shihipar-agent-sdk-workshop.md`

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Shihipar defines the progression: LLM Features → Workflows → Agents. Agents manage their own context and trajectories autonomously.
- The agent loop is framed as: **Context → Thought → Action → Observation**, running continuously until the task is complete.
- The SDK's `query()` function provides a streaming async interface that abstracts the entire agent loop — developers just iterate over messages.
- The "Harness" concept encapsulates the loop, tool execution, and state management into a reusable infrastructure layer.

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- **"Bash is all you need"** — The SDK encourages Unix primitives (Bash, shell commands, file system) as the primary tool interface, rather than building many rigid custom tools.
- Built-in tools: Read, Write, Edit, Bash, Glob, Grep, plus custom tool support and MCP integration.
- Hooks provide "deterministic overrides" — developers inject constraints (e.g., "always read before writing") at tool execution boundaries without model retraining.
- Subagents can be defined inline with scoped tool access (e.g., a code-reviewer agent limited to read-only tools).
- Skills are loaded from `.claude/skills/*/SKILL.md` — the same format used by Claude Code CLI.

### [03 - Security & Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Introduces the **"Swiss Cheese" defense model** — multi-layered security where no single layer is perfect, but the combination provides robust protection:
  1. Model alignment (Anthropic internal)
  2. Harness permissioning (read-only vs. read-write zones)
  3. Sandboxing (containers, Cloudflare sandboxes)
- `allowed_tools` parameter provides fine-grained permission control at the SDK level.
- `permission_mode` options like `acceptEdits` give developers control over what the agent can do autonomously.

### [04 - Context Management](references/dimensions/04-context-management.md)
- **"File-system-pilled"** approach: Agents use the file system as external memory, rather than trying to fit everything in the context window.
- Progressive disclosure pattern: SKILL.md and CLAUDE.md provide context at different granularities, loaded only when relevant.
- Session management allows resuming conversations with full context from previous queries, enabling long-running multi-turn tasks.
- Context rot prevention: structured disclosure ensures agents pull in only necessary context at specific stages.

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- The SDK natively supports defining subagents as `AgentDefinition` objects with their own prompts, descriptions, and tool scopes.
- The `Agent` tool allows the main agent to delegate work to specialized subagents (e.g., code-reviewer, security-auditor).
- Subagents are identified by `parent_tool_use_id`, enabling hierarchical tracking.
- MCP integration allows connecting to external tool servers (e.g., Playwright MCP for browser automation).

## Notable Quotes

> "Bash is all you need." — **Thariq Shihipar**

> "If you can verify the agent's work, it is a strong candidate for autonomous development." — **Thariq Shihipar**

> "Building agents is not just about LLM capabilities; it is about infrastructure design." — **Thariq Shihipar**

## Relevant Dimensions

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Harness architecture, query() streaming loop, Context→Thought→Action→Observation
- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — "Bash is all you need", hooks, MCP, skills integration
- [03 - Security & Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Swiss Cheese defense, permission modes, sandboxing
- [04 - Context Management](references/dimensions/04-context-management.md) — File-system as memory, progressive disclosure, sessions
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — SDK subagents, AgentDefinition, MCP

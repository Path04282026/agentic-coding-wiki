---
tags: [entity, product, case-study]
product: Hermes Agent
date: 2026-04-25
dimensions: [01, 02, 03, 04, 05, 06, 07, 08, 09]
---

# Hermes Agent

> Hermes Agent is treated in this wiki as a **case-study product** rather than as a fourth first-class column in the core three-way comparison. The current schema still governs the main comparison around [Claude Code](references/entities/claude-code.md), [Codex](references/entities/codex.md), and [OpenClaw](references/entities/openclaw.md).

## Overview

Hermes Agent is an API-accessed, tool-using assistant oriented around real actions: reading and editing files, running terminal commands, browsing websites, delegating subtasks, managing memory/skills, scheduling jobs, and communicating through connected chat surfaces. This initial description is grounded in [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md).

The most useful design framing is: **Hermes is a self-evolving agent operating layer**. It is not only an agentic coding tool; it is a cross-platform executor that can apply agentic coding patterns to repositories, documents, browsers, schedules, and messaging workflows, while the new external article adds evidence for dynamic skill generation and trajectory/RL workflows. [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md) [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

## Product Role in This Wiki

Hermes should be used as a practical design case study for the product patterns already extracted from [Claude Code](references/entities/claude-code.md), [Codex](references/entities/codex.md), and [OpenClaw](references/entities/openclaw.md):

- tool-rich action loop: [01 - Agent Loop](references/dimensions/01-agent-loop.md), [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- layered context and memory: [04 - Context Management](references/dimensions/04-context-management.md)
- subagent orchestration: [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- chat/workspace distribution: [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- “self-evolving agent operating layer” philosophy: [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md), [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md)

## Design Positioning

| Question | Hermes Agent Position |
|---|---|
| Is it primarily a coding agent? | Partly. It can manage code/wiki repositories, but its tool surface is broader than coding. |
| Is it CLI-first? | Not necessarily. In this session it operates through Feishu/Lark while controlling local tools. |
| Is it tool-rich or minimalist? | Tool-rich, with specialized tools for files, terminal, browser, delegation, memory, skills, cron, messaging, media, and vision. |
| Is it multi-agent? | Yes, via `delegate_task` with isolated subagents and optional orchestrator roles. |
| Is memory built in? | Yes, through durable memory, session search, and skills as procedural memory. |
| Is it self-evolving? | According to the external article, Hermes adds dynamic skill generation, background review, trajectory capture, and RL workflows; the wiki treats this as source-backed article analysis pending direct repo verification. |
| Is it a fourth main comparison product? | Not yet. It is currently a case study under `wiki/hermes/` until enough source evidence accumulates. |

## Key Capabilities

- **Action-first loop** — executes tool calls instead of only describing intended actions. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Typed tool surface** — file, terminal, browser, process, scheduling, messaging, memory, skills, and delegation tools. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Persistent context mechanisms** — durable memory, session search, skills, and project context files. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Subagent delegation** — isolated leaf/orchestrator agents for parallel or specialized work. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Messaging integration** — current session is delivered through Feishu/Lark, with scheduled tasks able to deliver back to the origin chat. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Self-evolution loop** — dynamic skill generation, background review agents, trajectory capture/compression, and RL workflows according to the external article source. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

## Open Questions

- Should Hermes eventually become a fourth first-class product in the 9 dimension pages?
- Which Hermes materials should become immutable raw sources if a public/product documentation set exists?
- What is the canonical Hermes safety model: policy/tool discipline, sandboxing, platform permissions, or some hybrid?
- How should Hermes-specific concepts such as skills, session search, cron jobs, and delegation be mapped into existing wiki dimensions?

## Related Pages

- **Hermes Agent Case Study**
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md)
- [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md)
- **Agentic Coding Product Design Questions**
- **Agentic Coding Product Design Patterns**

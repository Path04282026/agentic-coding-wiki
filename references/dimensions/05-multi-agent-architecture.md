---
tags: [dimension]
dimension: 05
title: Multi-Agent Architecture
products: [Claude Code, Codex, OpenClaw]
source_count: 14
---

# Multi-Agent Architecture

## Overview

Multi-agent architecture is how these tools scale beyond a single model conversation to coordinate multiple independent agents working in parallel. All three products are converging on the same vision — swarms of agents with independent context windows — but approach it from different directions.

Claude Code has the most structured implementation (Coordinator Mode, typed sub-agents). Codex builds multi-agent support into its app-server and open-source harness. OpenClaw's multi-agent approach is emergent — Peter manually orchestrates 5–10 agents across terminals, treating it as "playing multiple chess boards at once."

## Claude Code

**AgentTool — Typed sub-agents:**
- Types: `general-purpose`, `Explore`, `Plan`, `claude-code-guide`, `statusline-setup`
- Each gets a fresh context window
- Can run in foreground (blocking) or background
- Optional git worktree isolation (`isolation: "worktree"`)
- Parent can send follow-up messages to running agents
- Automatic cleanup of worktrees

**Coordinator Mode (internal):**
- A coordinator agent spawns worker agents
- Parallel phases: Research → Synthesis → Implementation → Verification
- Shared scratchpad directory for inter-agent knowledge sharing
- Worker tools filtered to async-safe subset

**Uncorrelated context windows — the key insight:**
> "Throwing more tokens at it when the windows are uncorrelated gives you better results. It's actually a form of test-time compute." — [Boris Cherny](references/entities/boris-cherny.md)

Multiple agents with fresh, independent context windows perform better than a single agent with a very long context. The "magic of teams comes out of this idea of uncorrelated context windows. It's less about the specific configuration of the agents."

**Plugins built by swarms**: Daisy let a swarm run over a weekend — it spawned ~200 agents, created 100 tasks on an Asana board, implemented them independently. "That's pretty much the version of plugins that we shipped."

**Boris's daily workflow**: 5 terminal tabs, each with a separate repo checkout or git worktree, round-robining across them.

**Many remote Claude instances**: Cat Wu describes the scaling path from one reliable task to six parallel tasks, then eventually 50 or hundreds of Claude instances running remotely. The product challenge becomes task management, agent verification, and self-improving feedback loops.

**Code review agents**: Newer models unlocked code review by letting multiple review agents traverse the codebase simultaneously and synthesize real issues that engineers should fix before merge.

- Source citations: [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md), [blog--claude-code--subagents-in-claude-code](sources/blog-claude-code-subagents-in-claude-code.md), [blog--claude-code--building-multi-agent-systems-when-and-how-to-use-them](sources/blog-claude-code-building-multi-agent-systems-when-and-how-to-use-them.md), [blog--claude-code--claude-managed-agents](sources/blog-claude-code-claude-managed-agents.md)

## Codex

**Cloud agent fleets:**
> "We have strong conviction that the way this is headed is large populations of agents somewhere in the cloud… millions of agents working in company data centers to do useful work." — [Tibo Such](references/entities/tibo-such.md)

**Ghost snapshots**: Fork a turn for exploration without commitment — a lightweight multi-agent primitive where the model can explore alternatives without polluting the main conversation.

**App-server multi-client**: The app-server supports multiple client connections (IDE extensions, web clients, the Codex app) connecting to the same session, enabling distributed workflows.

**Sub-agents (emerging feature):**
> "There's an implementation of sub-agents that's out in the wild right now. People are using it and experimenting and we're learning a ton from them even though we don't actually trigger that proactively in the product." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

**Codex App as multi-agent surface**: Built specifically because power users were already t-muxing 18+ terminals across monitors. The app makes multi-agent delegation accessible to everyone.

**The delegation vision**: Alex's explicit long-term vision — "We're going to have models and we're not going to want to lend them our computer to do work because then we can only do one thing at a time. We're going to want infinitely many models doing work independently."

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md), [blog--codex--openai-developers-blog__2026-02-23__run-long-horizon-tasks-with-codex](sources/blog-codex-openai-developers-blog-2026-02-23-run-long-horizon-tasks-with-codex.md)

## OpenClaw

**Manual multi-agent orchestration:**
- Peter runs 5–10 agents in parallel across multiple terminals
- Each terminal handles an independent task
- He context-switches between them like "a chess grandmaster playing multiple boards"
- Workflow: design the system in conversation → hand off to agent → move to next terminal → check back when done

**The Starcraft analogy:**
> "It's like Starcraft — you have your main base and your side bases. They give you resources." — [Peter Steinberger](references/entities/peter-steinberger.md)

**Model-specific parallelization**: Peter uses different models for different tasks — Codex for complex coding (because "it reads three files and then reads for 10 minutes"), Claude for general purpose work. The parallelization strategy includes model selection.

**Agent-as-employee mental model:**
> "It felt a lot like being the boss again — imperfect, sometimes silly, but sometimes very brilliant engineers that I have to steer." — [Peter Steinberger](references/entities/peter-steinberger.md)

**6,600 commits in January**: Peter's multi-agent workflow produced 6,600 commits in a single month. "I sometimes posted the meme 'I'm limited by the technology of my time.' I could do more if agents would be faster."

**Swarm intelligence for framework improvement**: Luo Fuli describes OpenClaw's community and sub-agent workflows as a form of swarm intelligence. Research ideas can be handed to different sub-agents in parallel, cross-validated, and folded back into framework evolution.

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Sub-agent spawning** | Typed sub-agents via AgentTool | Ghost snapshots + emerging sub-agents | Manual terminal management |
| **Orchestration** | Coordinator Mode (internal) | App-server multi-client | Human orchestrator |
| **Agent isolation** | Git worktree per agent | Sandbox per process | Terminal per agent |
| **Shared state** | Scratchpad directory | App-server session state | Filesystem |
| **Key insight** | "Uncorrelated context windows" | "Millions of agents in the cloud" | "Multiple chess boards" |
| **Parallelism** | Background + foreground agents | Cloud delegation | 5–10 terminal instances |
| **Agent scaling** | Plugins built by ~200 agents over weekend | Cloud agent fleets (vision) | 6,600 commits/month |
| **User role** | Manager/orchestrator | Delegator | Grandmaster/architect |

## Key Quotes

> "Throwing more tokens at it when the windows are uncorrelated gives you better results. It's actually a form of test-time compute." — [Boris Cherny](references/entities/boris-cherny.md)

> "We have strong conviction that the way this is headed is large populations of agents somewhere in the cloud." — [Tibo Such](references/entities/tibo-such.md)

> "It's like Starcraft — you have your main base and your side bases." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "Developers act as orchestrators directing multiple independent models." — [Greg Brockman](references/entities/greg-brockman.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)
- [blog--claude-code--subagents-in-claude-code](sources/blog-claude-code-subagents-in-claude-code.md)
- [blog--claude-code--building-multi-agent-systems-when-and-how-to-use-them](sources/blog-claude-code-building-multi-agent-systems-when-and-how-to-use-them.md)
- [blog--claude-code--claude-managed-agents](sources/blog-claude-code-claude-managed-agents.md)
- [blog--codex--openai-developers-blog__2026-02-23__run-long-horizon-tasks-with-codex](sources/blog-codex-openai-developers-blog-2026-02-23-run-long-horizon-tasks-with-codex.md)
- [blog--claude-code--scaling-agentic-coding](sources/blog-claude-code-scaling-agentic-coding.md)

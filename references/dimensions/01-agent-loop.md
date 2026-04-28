---
tags: [dimension]
dimension: 01
title: The Agent Loop
products: [Claude Code, Codex, OpenClaw]
source_count: 21
---

# The Agent Loop

## Overview

The agent loop is the core architectural pattern that drives every agentic coding tool: **User input → Build prompt → Call LLM → Parse response → Execute tools → Loop**. This deceptively simple cycle is where the fundamental design decisions are made — how the model interacts with tools, how context is managed across turns, and how much autonomy the model is granted within each iteration.

All three products implement this pattern, but with radically different emphases. Claude Code treats the model as the primary driver and wraps it in a thin event-driven loop. Codex co-optimizes the model with a hardened Rust harness. OpenClaw pushes the boundary further — the agent is aware of its own source code and can modify itself mid-loop.

The agent loop isn't just an implementation detail — it's where each product's philosophy becomes concrete.

## Claude Code

Claude Code's agent loop lives in `query.ts` and `QueryEngine.ts` — a pure event-driven streaming architecture in TypeScript.

**Core characteristics:**
- **Streaming tool execution**: Tool calls are executed as they stream in from the API, not waiting for the full response. This makes the loop feel responsive even during complex multi-step operations.
- **Immutable message history**: The conversation history is treated as an immutable log, with compaction operating on a summarized view rather than mutating the original.
- **Concurrent tool partitioning**: Tools are annotated with `isConcurrencySafe`. Read-only tools (file reads, grep, glob) run in parallel automatically; mutating tools get exclusive access. This is a sophisticated optimization that most competitors lack.
- **Reactive compaction**: Token budgets are tracked per turn. When approaching limits, compaction triggers mid-turn — summarizing old context while the loop continues.

**Design philosophy — "Don't put the model in a box":**

> "Everyone had this mental model of: you take the model and you put it in a box and you figure out what is the interface. This is just not the way to think about the model. The way to think about it is the model is its own thing. You give it tools. You give it programs that it can run." — [Boris Cherny](references/entities/boris-cherny.md)

The architecture is intentionally simple — Boris describes it as "a core query loop, a few tools, and a security layer." They continuously add and remove tools, treating the loop as a stable foundation and the tool set as experimental.

**Sub-agents / "Mama Claude"**: Claude Code's loop is recursive. Sub-agents are "just a recursive Claude Code" with fresh context windows. The parent agent ("Mama Claude") spawns children for parallel work. This recursion is fundamental to features like Agent Teams and the Plugins system.

**Assistant to full agent**: Cat Wu frames the product roadmap as moving Claude Code from assistant behavior into full agent behavior: successful local tasks, every PR green, more PRs landed, and agent completion that includes verification before the human accepts it.

**Agent SDK as the packaged loop**: Thariq Shihipar's Agent SDK workshop reveals that Anthropic packaged this production-tested agent loop as a public SDK (`claude-agent-sdk`). The `query()` function provides a streaming async interface that abstracts the entire loop — developers just iterate over messages. The SDK embodies the "Harness" concept: loop, tool execution, and state management as a reusable infrastructure layer. "Building agents is not just about LLM capabilities; it is about infrastructure design."

- Source citations: [interview--claude-code--boris-cherny-2025-12-15](sources/interview-claude-code-boris-cherny-2025-12-15.md), [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md), [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md), [blog--claude-code--subagents-in-claude-code](sources/blog-claude-code-subagents-in-claude-code.md), [workshop--claude-code--thariq-shihipar-2026-01-05](sources/workshop-claude-code-thariq-shihipar-agent-sdk-2026-01-05.md)

## Codex

Codex's agent loop is implemented in Rust (`codex-rs/core/src/tasks/regular.rs`) with async Tokio runtime and Server-Sent Events (SSE) streaming from the OpenAI Responses API.

**Core characteristics:**
- **Async Rust with Tokio**: The loop is genuinely concurrent at the system level — not JavaScript's cooperative concurrency, but OS-thread-backed async tasks.
- **Central tool router** (`router.rs`): All tool calls pass through a single routing layer that enforces sandbox policies before execution.
- **Sandbox at the process level**: Unlike Claude Code's application-layer checks, Codex's tool execution happens inside a kernel-enforced sandbox. The loop and the sandbox are inseparable.
- **CancellationToken**: Graceful interruption is built into the loop via Tokio's cancellation tokens, allowing users to abort mid-operation cleanly.
- **SSE streaming**: The loop consumes events from the Responses API as a stream, processing tool calls as they arrive.

**The "harness" metaphor:**

> "The harness is your body and the model is your brain." — [Greg Brockman](references/entities/greg-brockman.md)

For Codex, the agent loop is part of the "harness" — the body that gives the model's intelligence physical capability. This framing leads to co-optimization: the model (GPT-5 Codex) is specifically trained to work with the harness, and the harness is tuned for the model. They are not interchangeable.

**Ghost snapshots**: Codex has a unique loop variant — ghost tasks that fork a turn for exploration without commitment. This lets the model explore approaches without polluting the main conversation state.

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md), [blog--codex--openai-news__2026-01-23__unrolling-the-codex-agent-loop](sources/blog-codex-openai-news-2026-01-23-unrolling-the-codex-agent-loop.md)

## OpenClaw

OpenClaw's agent loop is the most unconventional of the three — a self-modifying TypeScript system where the agent is aware of and can modify its own harness.

**Core characteristics:**
- **Self-aware architecture**: The agent "knows what its source code is. It understands how it sits and runs in its own harness. It knows where documentation is. It knows which model it runs." This self-awareness enables self-modification.
- **Closing-the-loop principle**: Peter's key design insight is that the agent must be able to debug and test itself. "The big secret is — it needs to be able to debug and test itself. That's the closing the loop principle."
- **Voice-driven prompting**: Peter uses extensive voice input, sending "bespoke prompts" via speech rather than typing. The loop accepts multi-modal inputs through messaging gateways (WhatsApp, Telegram, Discord).
- **Massive parallelization**: Peter runs 5–10 agents simultaneously across multiple terminals, each on independent tasks. The workflow resembles "a chess grandmaster playing multiple boards at once."
- **Self-modification in production**: The agent can modify its own source code via the agentic loop. "People talk about self-modifying software. I just built it." This emerged organically, not as a planned feature.

**The "agentic engineering" framework:**

> "I actually think vibe coding is a slur. I do agentic engineering — and then maybe after 3:00 AM I switch to vibe coding and then I have regrets the next day." — [Peter Steinberger](references/entities/peter-steinberger.md)

Peter frames himself as "the architect" — he has system-level understanding but doesn't read every line. The agent loop is his tool for expressing architectural intent, with the model handling implementation details.

**Gateway architecture**: Unlike Claude Code and Codex (which start from CLI), OpenClaw's loop originates from a messaging gateway. The original prototype was "hooking up WhatsApp to Claude Code — one-shot the CLI, message comes in, I call the CLI with minus P, it does its magic, I get the string back, I send it back to WhatsApp." This chat-first origin profoundly shapes the loop's design.

**Framework as middle layer**: Luo Fuli describes OpenClaw's agent loop as a thick middle layer between humans and models. The loop is not just chat plus tools; it includes scheduling, model routing, memory, skills, and task-completion choreography that compensate for model weaknesses.

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Primary language** | TypeScript (Bun) | Rust (Tokio async) | TypeScript (Node.js) |
| **Core pattern** | Streaming event-driven loop | Async task with SSE consumption | Self-modifying gateway loop |
| **Key metaphor** | "The model just wants to use tools" | "The harness is your body, the model is your brain" | "Closing the loop — it debugs itself" |
| **Tool execution** | Parallel for read-only, exclusive for writes | Through sandboxed exec-server | Direct execution, self-introspection |
| **Concurrency model** | JS event loop + concurrent-safe annotations | OS-level async (Tokio) | Multiple terminal instances |
| **Streaming** | Tools execute as they stream in | SSE events processed incrementally | Depends on underlying model CLI |
| **Context handling** | Immutable history + reactive compaction | CompactTask for summarization | Gateway message queuing |
| **Interruption** | User-initiated abort | CancellationToken (graceful) | Manual terminal management |
| **Self-modification** | No — agents spawn sub-agents | Ghost snapshots for exploration | Yes — agent modifies its own source |
| **Origin point** | Terminal chat app | Cloud agent → CLI pivot | WhatsApp message relay |
| **Model coupling** | Model-agnostic (supports Anthropic models) | Co-optimized (GPT-5 Codex specific) | Model-agnostic (uses various providers) |

## Key Quotes

> "The model — it just wants to use tools. That's all it wants." — [Boris Cherny](references/entities/boris-cherny.md)

> "The harness is your body and the model is your brain." — [Greg Brockman](references/entities/greg-brockman.md)

> "The big secret is it needs to be able to debug and test itself. That's the closing the loop principle." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "It's very simple… there's a core query loop. There's a few tools that it uses. We delete these tools all the time. We add new tools all the time." — [Boris Cherny](references/entities/boris-cherny.md)

> "This entity, this collaborator that's working with you — bringing that to you in the tools that you're already using." — [Tibo Such](references/entities/tibo-such.md)

> "I have a conversation with the model. It's like, 'Let's discuss, give me options' — they will not build things until I say build." — [Peter Steinberger](references/entities/peter-steinberger.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-02-19](sources/interview-claude-code-boris-cherny-2026-02-19.md)
- [interview--claude-code--boris-cherny-2026-02-24](sources/interview-claude-code-boris-cherny-2026-02-24.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md)
- [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-01](sources/interview-openclaw-peter-steinberger-2026-02-01.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--peter-steinberger-2026-02-25](sources/interview-openclaw-peter-steinberger-2026-02-25.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)
- [blog--codex--openai-news__2026-01-23__unrolling-the-codex-agent-loop](sources/blog-codex-openai-news-2026-01-23-unrolling-the-codex-agent-loop.md)
- [blog--codex--openai-developers-blog__2026-02-11__shell-skills-compaction-tips-for-long-running-agents-that-do-real-work](sources/blog-codex-openai-developers-blog-2026-02-11-shell-skills-compaction-tips-for-long-running-agents-that-do-real-work.md)
- [blog--claude-code--subagents-in-claude-code](sources/blog-claude-code-subagents-in-claude-code.md)
- [blog--claude-code--building-multi-agent-systems-when-and-how-to-use-them](sources/blog-claude-code-building-multi-agent-systems-when-and-how-to-use-them.md)
- [blog--claude-code--introduction-to-agentic-coding](sources/blog-claude-code-introduction-to-agentic-coding.md)
- [blog--claude-code--building-agents-with-the-claude-agent-sdk](sources/blog-claude-code-building-agents-with-the-claude-agent-sdk.md)
- [workshop--claude-code--thariq-shihipar-agent-sdk-2026-01-05](sources/workshop-claude-code-thariq-shihipar-agent-sdk-2026-01-05.md)

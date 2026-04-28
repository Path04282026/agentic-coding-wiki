---
tags: [navigation]
title: Start Here
---

# LLM Wiki — Agentic Coding Tools

> A persistent, self-maintaining knowledge base comparing the architecture, design philosophy, and implementation of three agentic coding products.

## The Three Products

| | [Claude Code](references/entities/claude-code.md) | [Codex](references/entities/codex.md) | [OpenClaw](references/entities/openclaw.md) |
|---|---|---|---|
| **Creator** | [Boris Cherny](references/entities/boris-cherny.md) @ [Anthropic](references/entities/anthropic.md) | [Greg Brockman](references/entities/greg-brockman.md), [Tibo Such](references/entities/tibo-such.md), [Michael Bolin](references/entities/michael-bolin.md) @ [OpenAI](references/entities/openai.md) | [Peter Steinberger](references/entities/peter-steinberger.md) |
| **Language** | TypeScript (Bun) | Rust (93 crates) | TypeScript (Node.js) |
| **Open Source** | No | Yes (Apache-2.0) | Yes (MIT, 175K+ stars) |
| **First Version** | Sep 2024 (internal) | Early 2025 ("10x" internal) | Nov 2025 (WhatsApp relay) |
| **Philosophy** | "Trust the model" | "Trust but verify" | "Agentic engineering" |
| **Key Metaphor** | The Printing Press | Brain & Body | Chess Grandmaster |

## Quick Navigation

### 🧭 Product Design Entry Point

Use this wiki first as an **agentic coding product design knowledge base**, not as a chronological source archive:

- **Agentic Coding Product Design Knowledge Base** — start here for design-oriented navigation
- **Agentic Coding Product Design Questions** — turn a product problem into 9 design questions
- **Agentic Coding Product Design Patterns** — reusable patterns distilled from Claude Code, Codex, and OpenClaw
- **Prompt Context Harness Design Lens** — 用 Prompt / Context / Harness 三层框架串联 Claude Code、OpenClaw、Hermes 三篇外部分析文章
- **Hermes Agent Case Study** — Hermes as an agent operating layer mapped into the same framework
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md) — first-pass Hermes 9-dimension analysis
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md) — Hermes 的动态 Skill、后台 review、trajectory 与 RL 自进化闭环
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md) — Hermes 的工具系统与 control-plane 设计深化
- [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md) — Hermes 的 memory / skills / session recall 分层模型
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md) — OpenClaw 的 messaging-native 个人助理路线 vs Hermes 的 self-evolving agent operating layer

### 🎯 The 9 Comparative Dimensions

These are the analytical backbone of the wiki — synthesized knowledge organized by theme:

1. [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Core architecture, streaming, tool-driven loops
2. [02 - Tool Systems](references/dimensions/02-tool-systems.md) — Tool design, editing approach, MCP, skills
3. [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Permission models, kernel vs app-layer safety
4. [04 - Context Management](references/dimensions/04-context-management.md) — System prompts, CLAUDE.md, compaction, memory
5. [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — Sub-agents, orchestration, swarm patterns
6. [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md) — CLI, desktop, web, mobile, messaging
7. [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) — Feature flags, open source vs proprietary
8. [08 - Shared Principles](references/dimensions/08-shared-principles.md) — Patterns common to all three products
9. [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) — The fundamental design bets

### 📊 Comparisons

- [Three-Way Comparison](references/comparisons/three-way-comparison.md) — Master comparison table across all 9 dimensions
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md) — Focused comparison of OpenClaw and Hermes Agent

### 👤 People

- [Boris Cherny](references/entities/boris-cherny.md) — Creator of Claude Code
- [Greg Brockman](references/entities/greg-brockman.md) — OpenAI co-founder, "harness is your body" philosophy
- [Tibo Such](references/entities/tibo-such.md) — Codex engineering lead
- [Michael Bolin](references/entities/michael-bolin.md) — Codex open-source tech lead
- [Alex (Codex PM)](references/entities/alex-(codex-pm).md) — Codex PM lead
- [Peter Steinberger](references/entities/peter-steinberger.md) — Creator of OpenClaw

### 🏢 Organizations

- [Anthropic](references/entities/anthropic.md) — Maker of Claude Code
- [OpenAI](references/entities/openai.md) — Maker of Codex

### 📅 Timeline

- [timeline](references/timeline.md) — Chronological milestones across all three products

## How This Wiki Works

This wiki is maintained as a **product design knowledge base** backed by sources:

1. **Design entry — `wiki/design/`**: question-driven guides and reusable patterns for designing agentic coding products.
2. **Layer 1 — `raw/`**: Immutable source material (100+ blog posts, interview transcripts, structured summaries). Never modified.
3. **Layer 2 — `wiki/`**: Obsidian vault with synthesis pages, dimensions, entities, comparisons, design guides, source summaries, and code-analysis notes.
4. **Layer 3 — `CLAUDE.md`**: The schema that governs how the LLM maintains this wiki.

See the [index](references/hermes/index.md) for a complete catalog of all pages.

*Wiki created 2026-04-11. Source material spans September 2025 – April 2026.*

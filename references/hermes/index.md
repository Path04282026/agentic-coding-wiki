---
tags: [hermes, index, case-study]
product: Hermes Agent
date: 2026-04-25
dimensions: [01, 02, 03, 04, 05, 06, 07, 08, 09]
---

# Hermes Agent Case Study

> This folder holds the Hermes Agent case study. It uses the wiki’s 9-dimension product-design framework without yet changing the main three-product schema for [Claude Code](references/entities/claude-code.md), [Codex](references/entities/codex.md), and [OpenClaw](references/entities/openclaw.md).

## Start Here

- [Hermes Agent](references/entities/hermes-agent.md) — product/entity overview
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md) — first-pass mapping of Hermes into the 9-dimension framework
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md) — deep dive on dynamic skills, background review, trajectories, and RL workflows
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md) — deep dive on Hermes as a tool-rich agent operating layer
- [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md) — deep dive on memory, skills, session recall, and context routing
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md) — focused comparison of OpenClaw's personal-agent route and Hermes's self-evolving operating-layer route
- [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md) — source note summarizing the current Hermes runtime/tool context
- [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md) — external article source on Hermes self-evolution, prompt, context, and harness design

## Why Hermes Is a Case Study, Not a Fourth Column Yet

The repository schema currently says the main wiki answers comparative questions about **three** products: [Claude Code](references/entities/claude-code.md), [Codex](references/entities/codex.md), and [OpenClaw](references/entities/openclaw.md). Adding Hermes as a fourth first-class product would require changing `CLAUDE.md`, all 9 dimension pages, comparison tables, overview, index, and source-count conventions.

This case-study folder is the safer first step:

1. preserve the three-product schema
2. map Hermes against the same 9 dimensions
3. identify where Hermes aligns with or diverges from existing patterns
4. accumulate enough source evidence before deciding whether to upgrade the whole wiki to a four-product comparison

## Reading Path

### If you want the short answer

Read **Hermes Agent Through the 9 Dimensions#Executive Summary**.

### If you want to compare Hermes to existing products

Read these pages in order:

1. **Agentic Coding Product Design Patterns**
2. [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
3. [Three-Way Comparison](references/comparisons/three-way-comparison.md)
4. [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

### If you want to design Hermes features

Use **Agentic Coding Product Design Questions** as the checklist, then read the Hermes deep dives for implementation-oriented design detail:

1. [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md)
2. [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md)
3. [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md)

Then map each proposed Hermes capability into:

- [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- [04 - Context Management](references/dimensions/04-context-management.md)
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)
- [08 - Shared Principles](references/dimensions/08-shared-principles.md)
- [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

## Current Hermes Thesis

Hermes is best framed as a **self-evolving agent operating layer**:

- broader than a coding-only agent
- action-first rather than advice-first
- tool-rich and workflow-oriented
- multi-agent capable through delegation
- chat/workspace distributed
- memory/skill/session-search aware
- capable of turning trajectories into dynamic skills and possible RL training data

This thesis is developed in [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md) and [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md), sourced to [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md) and [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md).

## Future Work

- Add public/raw Hermes documentation sources if available.
- Add a dedicated “Hermes Safety Model” deep dive.
- Compare Hermes directly against Claude Code / Codex / OpenClaw once enough source evidence exists.

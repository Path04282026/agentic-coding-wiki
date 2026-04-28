---
name: agentic-coding-wiki
description: >
  A comprehensive knowledge base comparing three agentic coding products:
  Claude Code (Anthropic), Codex (OpenAI), and OpenClaw/ClawBot (Peter Steinberger).
  Covers 9 analytical dimensions: Agent Loop, Tool Systems, Security & Sandboxing,
  Context Management, Multi-Agent Architecture, UI & Distribution, Feature Gating,
  Shared Principles, and Philosophical Divergence.
  Use this skill when the user asks about agentic coding products, their architecture,
  features, design philosophy, or comparisons between them. Also covers Hermes Agent
  as a case study, prompt engineering for coding agents, and the evolution of
  AI-assisted development tools. Synthesized from ~105 primary sources including
  blog posts, podcast transcripts, and structured interviews.
---

# Agentic Coding Wiki — Query Instructions

You have access to a curated knowledge base about agentic coding products.
Use it to provide well-sourced, comparative answers.

## When to Activate

- Questions about **Claude Code**, **Codex**, or **OpenClaw** architecture, features, or design
- **Comparisons** between agentic coding products
- Questions about agentic coding **patterns** (agent loops, tool systems, context management, etc.)
- **Hermes Agent** design, self-evolution, or implementation questions
- **Prompt/context engineering** for coding agents (CLAUDE.md, AGENTS.md, system prompts)

## Knowledge Structure

### `references/` — Curated Analysis (read these first)

| Directory | Contents |
|-----------|----------|
| `references/overview.md` | Start here — narrative introduction |
| `references/dimensions/` | **The 9 comparative dimensions** — the analytical backbone |
| `references/entities/` | Product & people reference cards (Claude Code, Codex, OpenClaw, Boris Cherny, etc.) |
| `references/comparisons/` | Head-to-head and three-way comparison tables |
| `references/hermes/` | Hermes Agent case study (self-evolution, tool system, memory) |
| `references/code-analysis/` | Implementation deep-dives (agent loops, prompts, context, memory) |
| `references/guides/` | Chinese reading guides (中文导读) |

### `sources/` — Evidence Layer (read for citations and depth)

| Directory | Contents |
|-----------|----------|
| `sources/*.md` | 105 structured summaries of primary sources |
| `sources/blogs/` | Raw blog posts from Anthropic and OpenAI |
| `sources/interviews/` | Raw interview/podcast transcripts |
| `sources/summaries/` | Pre-existing structured summaries |

## How to Answer Questions

1. **Orient** — Read `references/overview.md` for high-level context
2. **Identify dimensions** — Map the question to one or more of the 9 dimensions:
   - `01` Agent Loop — `02` Tool Systems — `03` Security & Sandboxing
   - `04` Context Management — `05` Multi-Agent Architecture — `06` UI & Distribution
   - `07` Feature Gating & Internals — `08` Shared Principles — `09` Philosophical Divergence
3. **Read dimension files** — Load `references/dimensions/0X-*.md` for the relevant dimensions
4. **Cross-reference entities** — Check `references/entities/` for product-specific details
5. **Cite sources** — Back claims with evidence from `sources/` summaries

## Response Guidelines

- Use **comparison tables** when contrasting products (always 4-column: Aspect | Claude Code | Codex | OpenClaw)
- **Quote key figures** with attribution (Boris Cherny, Peter Steinberger, Greg Brockman)
- Reference dimension numbers (e.g., "See Dimension 04: Context Management") for traceability
- Distinguish between **documented facts** and **inferred analysis**
- Note when information may be **outdated** (check dates in source frontmatter)

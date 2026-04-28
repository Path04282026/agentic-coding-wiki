---
tags: [dimension]
dimension: 04
title: Context Management
products: [Claude Code, Codex, OpenClaw]
source_count: 16
---

# Context Management

## Overview

Context management is how agents maintain coherence across long conversations, navigate large codebases, and persist knowledge between sessions. This dimension encompasses system prompts, project configuration files (CLAUDE.md / AGENTS.md), compaction strategies for long conversations, and memory systems for cross-session persistence.

Claude Code leads in sophistication — with multi-strategy compaction, the Dream system for background memory consolidation, and hierarchical CLAUDE.md files. Codex takes a simpler approach with AGENTS.md and CompactTask. OpenClaw makes the agent self-aware of its entire runtime, enabling a unique form of contextual understanding.

## Claude Code

**System prompt assembly** — Dynamic, multi-source:
1. `fetchSystemPromptParts()` — modular prompt sections
2. `getSystemContext()` — git status, OS, shell, current directory
3. `getUserContext()` — CLAUDE.md files, memory files, current date
4. Context injected fresh before every API call
5. Prompt sections are feature-gated (internal vs external builds)

**CLAUDE.md — hierarchical project instructions:**
- **Hierarchy**: Managed → User → Project → Local
- Users organically started writing markdown instruction files — "This is where CLAUDE.md came from" (entirely user-driven emergence)
- Boris's personal CLAUDE.md is only 2 lines: auto-merge PRs + post PRs to team Slack
- With each model improvement, less instruction is needed: "What you're going to find is with every model, you have to add less and less."

**Compaction — multi-strategy system:**
- Token budget tracked per turn
- **Snip compaction** — cut old history
- **Micro compaction** — local compression of tool results
- **Reactive compaction** — streaming-aware, triggers mid-turn
- Pre/post compact hooks for user extensibility
- Compaction runs as a separate LLM call to summarize old content
- Result: "essentially infinite context" through auto-compacting

**Memory — The Dream System:**
- File-based memory in `~/.claude/projects/<project>/memory/`
- `MEMORY.md` index (max 200 lines) loaded every session
- Memory types: `user`, `feedback`, `project`, `reference`
- **autoDream**: Background consolidation engine
  - Three-gate trigger: 24h since last + 5 sessions + lock
  - Four phases: Orient → Gather → Consolidate → Prune
  - Converts relative dates to absolute, deletes contradicted facts, merges duplicates
  - Runs as a forked sub-agent

**RAG was tried and abandoned:**
> "Agentic search just outperformed everything… glob and grep. That's all it is." — [Boris Cherny](references/entities/boris-cherny.md)

**Connected work context for Co-work**: Cat Wu emphasizes that non-code agent work depends on connecting the right context sources: Calendar, Slack, Gmail, Google Drive, design templates, and team source-of-truth docs. She also uses model introspection to debug harness failures, asking why the model skipped verification or misunderstood a task.

- Source citations: [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md), [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md), [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md), [blog--claude-code--1m-context-ga](sources/blog-claude-code-1m-context-ga.md)

## Codex

**AGENTS.md — flat project instructions:**
- Single file at project root
- Content: coding conventions, build instructions, test patterns
- Serves as both **compression** (efficient for the agent to read instead of exploring the entire codebase) and **preferences** (things not visible in code)

> "More efficient for the agent to just read agents.md instead of exploring the entire codebase." — [Greg Brockman](references/entities/greg-brockman.md)

**System prompt — Template-based:**
1. Templates in `codex-rs/core/templates/`
2. AGENTS.md loaded as project instructions
3. Configuration layered: system → user → CLI → env vars
4. Simpler assembly, less dynamic than Claude Code

**CompactTask:**
- Dedicated task type for compaction
- Simpler approach: summarize when history exceeds threshold
- Less granular than Claude Code's multi-strategy system

**Shell skills compaction:**
- Specific focus on compacting repetitive shell outputs
- Blog post details strategies for keeping long-running agents coherent

**Memory:**
- SQLite-backed state database for conversation history persistence
- Rollout state for feature flags
- No equivalent to the Dream system
- Agent memory flagged as an active research problem

> "Agents right now don't have great memory… We have real research to do to think about how do you have memory." — [Greg Brockman](references/entities/greg-brockman.md)

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md), [blog--codex--openai-developers-blog__2026-02-11__shell-skills-compaction-tips-for-long-running-agents-that-do-real-work](sources/blog-codex-openai-developers-blog-2026-02-11-shell-skills-compaction-tips-for-long-running-agents-that-do-real-work.md)

## OpenClaw

**System-aware context:**
- The agent "knows what its source code is. It understands how it sits and runs in its own harness."
- It knows where documentation is, which model it's running, whether verbose or reasoning mode is enabled
- This self-awareness provides a unique form of context — the agent's knowledge includes its own implementation

**Memory approach:**
- Markdown-based memory files (similar to Claude Code's approach)
- Vector database integration for longer-term memory retrieval
- Peter describes being "level two or three with markdown files and the vector database"
- Memory as a game-like progression: "Maybe the ultimate boss is continuous reinforcement learning"

**Soul.md as personality context:**
- Inspired by Anthropic's Constitutional AI work
- Gives the agent a personality and behavioral guidelines
- The agent helped write its own Soul.md — "give yourself a name"

**Persistent memory as framework capability**: Luo Fuli calls out OpenClaw's durable memory, cross-session sharing, session compression, user portraits, and group-chat context as part of the agent framework itself. She treats static memory/skills and dynamic run-time architecture as co-evolving context layers.

**Codebase as context:**
- Peter uses a browser extension to convert GitHub repositories into single large markdown files
- These are fed into models (like Gemini) to generate specs, which are then passed to coding agents
- This manual context construction is a pragmatic approach to the context window problem

> "It knows what its source code is. It understands how it sits and runs in its own harness. It knows where documentation is. It knows which model it runs." — [Peter Steinberger](references/entities/peter-steinberger.md)

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Project config file** | CLAUDE.md (hierarchical) | AGENTS.md (flat) | Soul.md + custom docs |
| **Config hierarchy** | Managed → User → Project → Local | Single file at root | Per-project configuration |
| **Compaction** | Multi-strategy (snip, micro, reactive) | Single CompactTask | Depends on underlying agent |
| **Cross-session memory** | Dream system (background consolidation) | SQLite state DB | Markdown files + vector DB |
| **Memory consolidation** | autoDream (4-phase background process) | None | Manual + vector indexing |
| **System prompt** | Dynamic, feature-gated, multi-source | Template-based, layered config | Self-aware (knows own source) |
| **Codebase navigation** | Agentic search (glob + grep) | Shell-based exploration | Manual spec generation |
| **RAG** | Tried and abandoned | Not implemented | Vector DB experiments |
| **Infinite context** | Auto-compacting context management | CompactTask when threshold hit | Context window dependent |

## Key Quotes

> "This is where CLAUDE.md came from — entirely user-driven." — [Boris Cherny](references/entities/boris-cherny.md)

> "Delete your CLAUDE.md and just start fresh." — [Boris Cherny](references/entities/boris-cherny.md)

> "With every model, you have to add less and less [instruction]." — [Boris Cherny](references/entities/boris-cherny.md)

> "More efficient for the agent to just read agents.md instead of exploring the entire codebase." — [Greg Brockman](references/entities/greg-brockman.md)

> "Agents right now don't have great memory… We have real research to do." — [Greg Brockman](references/entities/greg-brockman.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)
- [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- [blog--claude-code--1m-context-ga](sources/blog-claude-code-1m-context-ga.md)
- [blog--codex--openai-developers-blog__2026-02-11__shell-skills-compaction-tips-for-long-running-agents-that-do-real-work](sources/blog-codex-openai-developers-blog-2026-02-11-shell-skills-compaction-tips-for-long-running-agents-that-do-real-work.md)

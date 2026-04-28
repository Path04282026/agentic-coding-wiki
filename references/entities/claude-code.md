---
tags: [entity, product]
product: Claude Code
dimensions: ["01 - Agent Loop", "02 - Tool Systems", "03 - Security and Sandboxing", "04 - Context Management", "05 - Multi-Agent Architecture", "06 - UI and Distribution", "07 - Feature Gating and Internals", "08 - Shared Principles", "09 - Philosophical Divergence"]
---

# Claude Code

**Developer:** [Anthropic](references/entities/anthropic.md)
**Creator:** [Boris Cherny](references/entities/boris-cherny.md)
**Product lead:** [Cat Wu](references/entities/cat-wu.md)
**First version:** September 2024 (internal), originally named "QuadCLI"
**Language:** TypeScript (bundled with Bun)
**Open source:** No (leaked via npm sourcemap March 31, 2026)

## Origins

Boris Cherny built Claude Code as a personal side project to learn the Anthropic public API. He "didn't want to build a UI" so he made a simple terminal chatbot. When tool use shipped, he added a single Bash tool and asked it "what music am I listening to?" — the model one-shotted an AppleScript program. Two days later, the team was already using it.

The terminal form factor was supposed to be temporary. Boris: "It's unbelievable that we're still using a terminal. That was supposed to be the starting point." The decision to keep it was driven by the rate of model improvement making any GUI investment feel wasteful.

## Architecture

- **Agent loop**: Pure event-driven streaming in TypeScript (`query.ts` + `QueryEngine.ts`)
- **Tools**: 40+ specialized tools with `isConcurrencySafe` annotations for automatic parallelization
- **Code editing**: Find-and-replace (`Edit` tool with `old_string` → `new_string`)
- **Security**: Swiss cheese model — 8 application-layer defenses
- **Memory**: Dream system (autoDream — 4-phase background consolidation)
- **Context**: Hierarchical CLAUDE.md + multi-strategy compaction (snip, micro, reactive)
- **Sub-agents**: Typed (Explore, Plan, general-purpose) with git worktree isolation
- **UI**: Custom Ink (React for terminals), 50+ components
- **Build**: Single JS bundle (~785KB main.tsx)

## Key Design Principles

1. **"Don't put the model in a box"** — Give it tools and let it decide what to do
2. **The Bitter Lesson** — General methods always beat specialized ones; never bet against the model
3. **Build for the model 6 months from now** — Treat all scaffolding as tech debt
4. **Swiss cheese safety** — Many imperfect layers > one perfect layer

## Internal-Only Features

- KAIROS (proactive assistant)
- Coordinator Mode (multi-agent orchestration)
- Bridge Mode (remote control via claude.ai)
- ULTRAPLAN (30-minute planning with Opus 4.6)
- Voice Mode, Buddy (Tamagotchi companion)

## Distribution

CLI → VS Code → JetBrains → iOS/Android → Desktop → Web → Slack → GitHub → Co-work

Cat Wu maps the product surfaces by job: CLI for the newest and most powerful coding workflows, desktop for visual/front-end work and task control, web/mobile for remote task kickoff, and Co-work for non-code outputs such as decks, launch plans, docs, inbox workflows, and customer materials.

## Key Metrics (as of March 2026)

- 150% productivity growth per engineer since launch
- 70–90% of code at Anthropic written by Claude
- Boris: 100% of code written by Claude since Opus 4.5, ~20 PRs/day
- 4% of all public commits globally made by Claude Code (per Semi Analysis)

## Notable Quotes About Claude Code

> "All of Claude Code has just been written and rewritten and rewritten over and over. There is no part of Claude Code that was around 6 months ago." — [Boris Cherny](references/entities/boris-cherny.md)

## Article Analysis Notes

- [article--claude-code--prompt-context-harness-2026-04-20](sources/article-claude-code-prompt-context-harness-2026-04-20.md) — External analysis of Claude Code through Prompt / Context / Harness engineering.

## Interview Notes

- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md) — Cat Wu on AI-native product management, Co-work, action-based agents, multi-agent scaling, and designing around current model capability.

## Linked Dimensions

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Streaming event-driven loop, sub-agents
- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — 40+ specialized tools, skills system
- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Swiss cheese model, permission prompts
- [04 - Context Management](references/dimensions/04-context-management.md) — CLAUDE.md, Dream system, compaction
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — Coordinator Mode, Agent Teams
- [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md) — Terminal → multi-surface expansion
- [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) — KAIROS, dual build system
- [08 - Shared Principles](references/dimensions/08-shared-principles.md) — Minimal scaffolding, CLI-first
- [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) — "Trust the model"

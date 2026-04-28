---
tags: [entity, product]
product: OpenClaw
dimensions: ["01 - Agent Loop", "02 - Tool Systems", "03 - Security and Sandboxing", "04 - Context Management", "05 - Multi-Agent Architecture", "06 - UI and Distribution", "07 - Feature Gating and Internals", "08 - Shared Principles", "09 - Philosophical Divergence"]
---

# OpenClaw

**Creator:** [Peter Steinberger](references/entities/peter-steinberger.md)
**Former names:** W-relay → Clauders → Claudes → Clawbot → OpenClaw
**First version:** November 2025 (1-hour WhatsApp relay prototype)
**Language:** TypeScript (Node.js)
**Open source:** Yes (MIT, 175,000+ GitHub stars — fastest growing repo in GitHub history)

## Origins

Peter Steinberger, creator of PSPDFKit (used on 1 billion+ devices), burned out after 13 years, sold his shares, and disappeared from tech for 3 years. In April 2025, he returned, discovered Claude Code, and fell into "one more prompt" addiction. By November, frustrated that a personal AI assistant didn't exist, he "prompted it into existence" — hooking up WhatsApp to Claude Code in a 1-hour prototype.

The name saga: W-relay → Clauders → Claudes (Claude with a W — "Clawd") → Clawbot → OpenClaw (after Anthropic's friendly request to stop using "Claude" in the name).

> "I was annoyed that it didn't exist. So I just prompted it into existence." — [Peter Steinberger](references/entities/peter-steinberger.md)

## Architecture

- **Agent loop**: Self-modifying TypeScript — the agent knows and can modify its own source code
- **Gateway**: Messaging-first architecture (WhatsApp, Telegram, Discord, Signal, iMessage)
- **Tools**: System-level access + self-modifying tool creation
- **Security**: Docker containers, evolved from initial prompt-level constraints
- **Memory**: Markdown files + vector database experiments
- **Context**: Soul.md (personality/constitutional AI), self-aware of own internals
- **Sub-agents**: Manual orchestration of 5–10 parallel terminal instances
- **UI**: Messaging apps as primary interface, web UI for community

## Key Design Principles

1. **"Closing the loop"** — The agent must be able to debug and test itself
2. **"Agentic engineering, not vibe coding"** — Structured prompting with system-level understanding
3. **Self-modifying software** — The agent improves itself through use
4. **Solo-builder philosophy** — One person + multiple agents can build what used to require teams
5. **Community-first** — Open source, transparent development, "prompt requests"
6. **Agent framework as middle layer** — [Luo Fuli](references/entities/luo-fuli.md) describes OpenClaw's important layer as the framework between humans and models: memory, skills, scheduling, model routing, and multi-agent orchestration.

## Milestone Numbers

- 6,600 commits in January 2026
- 175,000+ GitHub stars
- 5–10 agents running in parallel daily
- Built primarily using Codex (for complex coding) and Claude Code (for general purpose)

## Distribution

WhatsApp → Discord → Telegram → Signal → iMessage → Terminal → Web

## Notable Quotes About OpenClaw

> "People talk about self-modifying software. I just built it." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "Building this feels better than Factorio." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "It's hard to compete against someone who's just there to have fun." — [Peter Steinberger](references/entities/peter-steinberger.md)

## Article Analysis Notes

- [article--openclaw--prompt-context-harness-2026-04-13](sources/article-openclaw-prompt-context-harness-2026-04-13.md) — External analysis of OpenClaw through Prompt / Context / Harness engineering.
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md) — Focused comparison with Hermes Agent as a self-evolving agent operating layer.

## Interview Notes

- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md) — Luo Fuli on OpenClaw as an agent framework, model/framework co-evolution, swarm intelligence, and multi-agent productivity.

## Linked Dimensions

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Self-modifying gateway loop
- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — System-as-tool, voice-driven prompting
- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Docker, community trust
- [04 - Context Management](references/dimensions/04-context-management.md) — Self-aware agent, Soul.md
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — Manual multi-terminal orchestration
- [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md) — Messaging-first
- [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) — Everything public
- [08 - Shared Principles](references/dimensions/08-shared-principles.md) — CLI origins, minimal scaffolding
- [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) — "Agentic engineering"

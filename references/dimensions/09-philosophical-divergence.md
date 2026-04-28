---
tags: [dimension]
dimension: 09
title: Philosophical Divergence
products: [Claude Code, Codex, OpenClaw]
source_count: 17
---

# Philosophical Divergence

## Overview

Despite the shared principles documented in [08 - Shared Principles](references/dimensions/08-shared-principles.md), the three products represent fundamentally different bets about the future of AI-assisted development. These divergences aren't contradictions — they're different emphasis points that lead to different architectural choices, different safety tradeoffs, and different visions of what software development becomes.

## Claude Code: "Trust the Model"

**Core belief:** The model will soon be good enough that scaffolding is tech debt. Build the thinnest possible interface and let the model do the work.

**Key design decisions driven by this philosophy:**
- Safety is layered but soft — application-level, can be overridden by user choice
- More tools = more model autonomy. 40+ specialized tools give the model maximum flexibility.
- Internal features (KAIROS, Coordinator, Dream) push toward full autonomy — the agent manages its own memory, orchestrates its own workers, works proactively
- The Dream system lets the model manage its own long-term memory
- RAG was abandoned because the model's own search capability surpassed it

**The Bitter Lesson:** The team has a framed copy of Rich Sutton's "The Bitter Lesson" on the wall. The core argument: general-purpose learning always beats hand-crafted solutions. Applied to agents: never bet against the model's improving capability.

**Current-model product taste:** Cat Wu sharpens the Claude Code philosophy from a product-management angle: it is easy to design for a future super-AGI model, but hard to elicit maximum capability from today's model. The product should guide users toward model strengths, patch weaknesses, and delete scaffolding when newer models no longer need it.

**Action-based products:** Cat distinguishes Claude Code-era products from 2024 chatbots: the agent does the work rather than only telling the user what to do.

> "Never bet against the model." — [Boris Cherny](references/entities/boris-cherny.md)

> "The model — it just wants to use tools. That's all it wants." — [Boris Cherny](references/entities/boris-cherny.md)

> "The model is improving so quickly that the ideas that worked with the old model might not work with the new model." — [Boris Cherny](references/entities/boris-cherny.md)

**Implications for builders:** If you believe models will be trustworthy soon, lean toward Claude Code's approach — more model autonomy, richer tools, soft safety layers that can be relaxed as trust increases.

## Codex: "Trust but Verify"

**Core belief:** The model ("brain") is only as good as its harness ("body"). Raw intelligence without the right UX, safety, and infrastructure is insufficient. Co-evolve the interface with the model's capabilities.

**Key design decisions driven by this philosophy:**
- Safety is hard — kernel-enforced, cannot be overridden by the model or prompt injection
- Fewer tools, broader primitives — the model composes them rather than being given specialized tools
- Open source so users can verify the sandbox code themselves
- The harness is co-optimized with the model — GPT-5 Codex is trained specifically for the harness
- Performance matters — Rust binary, instant startup, minimal overhead

**The "harness matters" conviction:**

> "The harness is your body and the model is your brain… The tooling, everything else matters — can make such a big difference." — [Greg Brockman](references/entities/greg-brockman.md)

> "Scalable oversight — humans remain in the driver's seat." — [Greg Brockman](references/entities/greg-brockman.md)

> "Strict sandboxing written manually in Rust to guarantee the model stays within system bounds." — [Michael Bolin](references/entities/michael-bolin.md)

**The intelligence × convenience matrix:** Greg proposes two axes — intelligence (how smart) and convenience (latency, cost, integrations). The design space is about navigating between pulling convenience left and pushing intelligence up.

**Implications for builders:** If you believe models will remain unpredictable and need hard constraints, lean toward Codex's approach — kernel sandboxing, open source for trust, fewer but harder safety boundaries.

## OpenClaw: "Agentic Engineering, Not Vibe Coding"

**Core belief:** The agent is a collaborator that should understand itself, modify itself, and serve as a general-purpose assistant — not just a coding tool. The builder is the architect; the agent is the implementation team.

**Key design decisions driven by this philosophy:**
- Self-modifying software — the agent can read and modify its own source code
- Messaging-first — the agent should be accessible anywhere, like a friend on WhatsApp
- Solo-builder philosophy — one person + multiple agents can build what used to require a team
- System-level awareness — the agent knows its own internals, configuration, and state
- Community-driven development — open source not just for trust but for collective building

**The "closing the loop" principle:**

> "The big secret is it needs to be able to debug and test itself. That's the closing the loop principle." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "I actually think vibe coding is a slur. I do agentic engineering." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "It felt a lot like being the boss again — imperfect, sometimes silly, but sometimes very brilliant engineers that I have to steer." — [Peter Steinberger](references/entities/peter-steinberger.md)

**Agent framework as product:** Luo Fuli interprets OpenClaw's product not as the visible UI, but as the thick agent-framework layer between people and models. The open-source framework can be inspected, modified, taught, and improved by a community, which makes self-evolution a product property rather than only a model property.

**Implications for builders:** If you believe in the solo builder / small team future where one person orchestrates many agents, OpenClaw's approach provides the most direct model — self-modifying, system-aware, messaging-first, community-extended.

## The Three Philosophies Compared

| Dimension | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Core bet** | Model capability will solve everything | Harness quality is equally important | Self-modifying agents are the future |
| **Safety stance** | Soft layers, trusting the model | Hard boundaries, verifiable | Community-driven, power-user focused |
| **Model relationship** | "The model is its own thing" | "Brain + body" | "Imperfect but brilliant employee" |
| **Open source** | No (proprietary) | Yes (Apache-2.0, trust mechanism) | Yes (MIT, community mechanism) |
| **Target builder** | Enterprise teams + individual devs | Enterprise teams + open community | Solo builders + small teams |
| **Design reference** | The Bitter Lesson | Intelligence × Convenience matrix | Closing the loop principle |
| **Key metaphor** | Printing press | Brain and body | Chess grandmaster |
| **Where scaffolding goes** | Gets deleted as model improves | Gets hardened as harness matures | Gets modified by the agent itself |
| **Safety endgame** | Model alignment solves safety | Formal verification + kernel enforcement | Agent self-awareness enables responsibility |

## What This Means for the Industry

These aren't contradictions — they're different emphasis points. The safest approach is probably **all three combined:**

1. **Soft layers for UX** (Claude Code's approach) — flexible, iterable, good user experience
2. **Hard layers for security** (Codex's approach) — kernel-enforced, auditable, trustworthy
3. **Self-modifying for adaptability** (OpenClaw's approach) — the agent evolves with use

This synthesis is where the industry is heading. The winner won't be the product that picks one philosophy — it'll be the one that balances all three.

## Key Quotes

> "There's always this trade-off between building scaffolding now versus waiting for the model." — [Boris Cherny](references/entities/boris-cherny.md)

> "We haven't really nailed [the right interface] yet. That's going to continue to evolve." — [Greg Brockman](references/entities/greg-brockman.md)

> "I care more about the outcome, the product. How the plumbing works underneath — I care structurally, but not to the biggest detail." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "If something kind of works in AI right now, one year from now, it'll be incredibly reliable, incredibly mission critical." — [Greg Brockman](references/entities/greg-brockman.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
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

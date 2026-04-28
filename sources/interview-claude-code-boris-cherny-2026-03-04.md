---
tags: [source, interview]
product: Claude Code
date: 2026-03-04
dimensions: [01, 02, 03, 04, 05, 06, 07, 09]
speaker: Boris Cherny

---

# Interview — Claude Code — Boris Cherny (2026-03-04)

**Format:** Podcast interview (Pragmatic Engineer with Gergely Orosz)
**Speaker:** [Boris Cherny](references/entities/boris-cherny.md)
**Existing summary:** `raw/summaries/Claude_Code_20260304_Summary.md`
**Tier:** **Tier 1** — Creator and engineering lead discussing all aspects

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Core philosophy: "Don't put the model in a box"
- Three main parts: agent loop, UX layer, security infrastructure
- Sub-agents are "just a recursive Claude Code"

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- First tool was bash, second was file edit
- ~80% of tool experiments thrown away
- RAG abandoned for agentic search (glob + grep)
- Plugins system built by a swarm (~200 agents over a weekend)

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Swiss cheese model detailed
- Permission prompts invented with Ben Mann
- Conservative defaults — even `find` blocked due to code execution flags
- Co-work: VM + classifiers + OS-level deletion protection

### [04 - Context Management](references/dimensions/04-context-management.md)
- CLAUDE.md hierarchy emerged from user behavior
- Dream system / autoDream 4-phase consolidation
- "Delete your CLAUDE.md and just start fresh"

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- Agent Teams — "uncorrelated context windows = test-time compute"
- ~200 agent swarm built plugins over a weekend
- Boris workflow: 5 tabs, round-robin, plan mode

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Terminal → VS Code → JetBrains → iOS → Desktop → Web → Slack → Co-work
- Boris codes 1/3 to 1/2 on iPhone
- Co-work built by Felix in ~10 days, 100% by Claude Code

### [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)
- Internal vs external build system
- Feature velocity: weekly additions/removals

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- The Bitter Lesson as guiding principle
- Printing press analogy (most extended version)
- "Year of the generalist" / "Year of ADHD"
- Safety went from "kind of important" to "the most important thing"

## Notable Quotes

> "Everyone had this mental model of: you take the model and you put it in a box. This is just not the way to think about the model."

> "Throwing more tokens at it when the windows are uncorrelated gives you better results. It's actually a form of test-time compute."

> "Half my ideas are bad. At least half. And I don't know which half until I try it."

> "There's just no way we could have shipped this if we started with static mocks in Figma or if we started with a PRD."

> "If you told me six months ago I'd be writing a third/half of my code on a phone, that's crazy."

> "This is the first time ever where it's actually not crazy to just try the same idea every few months."

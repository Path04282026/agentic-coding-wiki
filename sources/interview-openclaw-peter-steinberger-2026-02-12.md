---
tags: [source, interview]
product: OpenClaw
date: 2026-02-12
dimensions: [01, 02, 03, 04, 05, 06, 07, 09]
speaker: Peter Steinberger

---

# Interview — OpenClaw — Peter Steinberger (2026-02-12)

**Format:** Lex Freedman Podcast — "The Age of the Lobster"
**Speaker:** [Peter Steinberger](references/entities/peter-steinberger.md)
**Lines:** ~4,730 (longest interview in the corpus)
**Tier:** **Tier 1** — Most comprehensive OpenClaw interview

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Self-modifying software: "People talk about self-modifying software. I just built it."
- The agent modifies its own source code through the agentic loop
- "The agent would just modify its own software"
- Self-introspection: "Hey, what tools do you see? Can you call the tool yourself?"

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- 15,000-line change to plugin architecture — didn't read all code
- "Most apps are just massaging data in different forms throughout your app"
- Voice prompting as primary input method

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Docker sandboxing details — Arch Linux container
- Lobster Curl story: agent built its own curl when Docker had no tools installed
- "I watch my agent happily click the 'I'm not a robot' button"
- Crypto swarming attacks on Discord

### [04 - Context Management](references/dimensions/04-context-management.md)
- Soul.md — agent helped write own personality
- Agent knows it's running in verbose mode, knows which model, knows documentation location
- "Give yourself a name" → lobster persona emerged

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- 4–10 agents in parallel depending on task difficulty and sleep
- "Between four and ten agents depending on how much I slept"
- 6,600 commits in January

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Name saga: W-relay → Clauders → Claudes → Clawbot → OpenClaw
- Anthropic's friendly name change request
- 175,000+ GitHub stars — fastest growing repo ever
- ClawCon: community-organized event, 1,000 attendees
- "Prompt requests" — non-developers submitting PRs using agents

### [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)
- Everything public, community-driven
- Peter builds in the open using agents

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- "I actually think vibe coding is a slur"
- Builder > architect: "I care about the outcome, the product"
- "It's hard to compete against someone who's just there to have fun"
- Factorio analogy: "Building this feels better than Factorio"

## Background: Full PSPDFKit Story

- Built PSPDF for iPad PDF display
- Grew to billion-device SDK
- Forced monthly blog posts: "a lot of developers don't like to write"
- CEO burnout: "You're basically the waste bin"
- 3-year digital exile: "months where I didn't even turn on my computer"
- Return via Claude Code in April 2025

## Notable Quotes

> "I made the agent very aware — it knows what its source code is. It understands how it sits and runs in its own harness."

> "I used to write really long prompts. And by writing, I mean I don't write. I talk. These hands are too precious for writing now."

> "It's hard to compete against someone who's just there to have fun."

> "Every time someone made the first pull request is a win for our society."

> "I had this idea — I was annoyed that it didn't exist. So I just prompted it into existence."

---
tags: [source, interview]
product: Codex
date: 2025-09-16
dimensions: [01, 02, 03, 04, 05, 06, 09]
speaker: [Greg Brockman, Tibo Such]

---

# Interview — Codex — Greg Brockman & Tibo Such (2025-09-16)

**Format:** OpenAI Podcast
**Speakers:** [Greg Brockman](references/entities/greg-brockman.md) (Co-founder & President), [Tibo Such](references/entities/tibo-such.md) (Engineering Lead)
**Host:** Andrew Maine
**Existing summary:** `raw/summaries/CodeX_20250916_Summary.md`
**Tier:** **Tier 1** — Founders and leads on design philosophy

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- "The harness is your body and the model is your brain"
- Intelligence × Convenience matrix — two axes of agent value
- "Maybe instead of the user driving this thing, let the model drive the interaction"
- GPT-5 Codex = co-optimized model + harness ("one agent")

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- Rust harness chosen — team learned Rust partly by using Codex
- GPT-5 Codex differentiator: "sustained effort" — works up to 7 hours on complex refactoring

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Sandbox-first by default
- Graduated permissions: sandbox → escalated permissions
- "Scalable oversight" as a research problem since 2017
- "Nobody reads the code" problem acknowledged

### [04 - Context Management](references/dimensions/04-context-management.md)
- AGENTS.md serves as compression + preferences
- Agent memory flagged as unsolved research problem

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- Vision: "millions of agents working in company data centers"
- Internal "10x" prototype was the first agent system
- Cloud + local should not be separate entities

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Internal "10x" prototype → Codex Cloud → Codex CLI
- CLI = "amazing vibe coding tool" / "zero-setup entry point"
- IDE for polish, terminal for power workflows

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- "Trust but verify" — kernel sandbox + scalable oversight
- "Always bet on intelligence" over convenience
- "If you can't make it useful for yourself, how are you going to make it useful for everyone?"
- 2030: "material abundance, compute scarcity" — need ~10 billion GPUs

## Notable Quotes

> "The harness is your body and the model is your brain."

> "There's two dimensions: intelligence and convenience. There's some acceptance region."

> "We will be in a world of material abundance… but absolute compute scarcity."

> "We have strong conviction that the way this is headed is large populations of agents somewhere in the cloud."

> "We actually had a prototype fully working in a terminal. It was called 10x."

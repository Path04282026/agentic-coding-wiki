---
tags: [source, interview]
product: Claude Code
date: 2025-12-15
dimensions: [01, 02, 04, 06, 09]
speaker: Boris Cherny

---

# Interview — Claude Code — Boris Cherny (2025-12-15)

**Format:** Podcast interview (Pragmatic Engineer)
**Speaker:** [Boris Cherny](references/entities/boris-cherny.md) — Creator & Engineering Lead, [Claude Code](references/entities/claude-code.md)
**Lines:** ~3,000+ (full transcript)

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Built as a personal side project to learn the Anthropic API
- First tool was Bash — asked "what music am I listening to?" and the model one-shotted an AppleScript
- Second tool was file edit. That was the entire initial tool set.
- Architecture is "very simple — a core query loop, a few tools"

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- ~80% of tool experiments are thrown away
- RAG was tried (local vector DB in TypeScript) and abandoned in favor of agentic search (glob + grep)
- Terminal spinner went through "50–100 iterations, ~80% never shipped"

### [04 - Context Management](references/dimensions/04-context-management.md)
- CLAUDE.md emerged from users organically writing instruction files
- Boris's personal CLAUDE.md is only 2 lines
- "With every model, you have to add less and less"
- The printing press analogy — scribes → writers/authors as software literacy expands

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Built in the terminal because Boris "didn't want to build a UI"
- "It's unbelievable that we're still using a terminal. That was supposed to be the starting point."
- Internal adoption went "vertical — 100%"

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- "Don't put the model in a box" — the Bitter Lesson applied to agents
- "The model is improving so quickly that the ideas that worked with the old model might not work with the new model"
- "Never bet against the model"

## Notable Quotes

> "It was essentially like a chat-based application but just in the terminal because that's what AI used to be."

> "The model — it just wants to use tools. That's all it wants."

> "Quad Code writes something like 80% of the code at Anthropic on average."

> "If you think about what happened to the scribes, they ceased to become scribes, but now there's a category of writers and authors."

---
tags: [source, interview]
product: OpenClaw
date: 2026-01-29
dimensions: [01, 02, 03, 04, 05, 06, 09]
speaker: Peter Steinberger

---

# Interview — OpenClaw — Peter Steinberger (2026-01-29)

**Format:** Podcast interview (Pragmatic Engineer with Gergely Orosz)
**Speaker:** [Peter Steinberger](references/entities/peter-steinberger.md) — Creator, [OpenClaw](references/entities/openclaw.md)
**Lines:** ~3,138 (full transcript)
**Tier:** **Tier 1** — Creator discussing origin story, architecture, and philosophy

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Closing-the-loop principle: "it needs to be able to debug and test itself"
- Self-modifying architecture: the agent modifies its own source code
- Runs 5–10 agents in parallel — "like Starcraft with your main base and side bases"
- Conversation-driven: "I have a conversation with the model until I say build"

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- System-as-tool: agent knows its source code, configuration, documentation, model
- Voice-driven prompting: "these hands are too precious for writing now"
- Audio message incident: agent self-discovered Whisper transcription via curl
- Self-introspection for debugging: "What tools do you see? Can you call the tool yourself?"

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Initially no sandboxing: "I just prompted it to only listen to me"
- Discord exposure led to hacking attempts
- Evolved toward Docker-based containerization

### [04 - Context Management](references/dimensions/04-context-management.md)
- Agent knows its own source code, harness, model, documentation
- Soul.md: agent helped write its own personality ("give yourself a name")
- Manual context via browser extension converting repos to markdown

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- 5–10 agents in parallel across terminals
- Chess grandmaster analogy: "I go there, make a decision, and for some boards I stop longer"
- 6,600 commits in January
- Multi-model: Codex for complex coding, Claude for general purpose

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Built in 1 hour: WhatsApp → Claude Code CLI → WhatsApp response
- Messaging-first: "something magical about using a chat client to talk to an agent"
- Audio message support emerged organically
- Peter: "I prompted it into existence"

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- "Vibe coding is a slur. I do agentic engineering."
- "It felt like being the boss again — imperfect, sometimes silly, but brilliant engineers I have to steer"
- Builder identity: "I care more about the outcome, the product"
- PSPDFKit background: 13 years, sold, burned out, 3-year hiatus, rediscovered coding via AI

## Background: PSPDFKit Story

- Built PDF framework used on 1 billion+ devices
- Rural Austria → bootstrap → $20M ARR at peak
- Burned out: "as a CEO you're basically the waste bin"
- 3-year digital exile → rediscovery via Claude Code in April 2025

## Notable Quotes

> "I actually think vibe coding is a slur. I do agentic engineering — and then maybe after 3 AM I switch to vibe coding and then I have regrets."

> "People talk about self-modifying software. I just built it."

> "The big secret is it needs to be able to debug and test itself. That's the closing the loop principle."

> "It's the same economics as a casino. My little slot machines. You press the trigger and ding ding ding."

> "I was annoyed that it didn't exist. So I just prompted it into existence."

> "Everything is just the right question away."

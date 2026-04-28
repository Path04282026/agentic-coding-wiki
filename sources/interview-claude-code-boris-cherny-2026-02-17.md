---
tags: [source, interview]
product: Claude Code
date: 2026-02-17
dimensions: [01, 02, 03, 04, 05, 07, 09]
speaker: Boris Cherny

---

# Interview — Claude Code — Boris Cherny (2026-02-17)

**Format:** Podcast interview (Martin Beeby / ThunderNerd)
**Speaker:** [Boris Cherny](references/entities/boris-cherny.md) — Creator & Engineering Lead, [Claude Code](references/entities/claude-code.md)
**Existing summary:** `raw/summaries/Claude_Code_20260217_Summary.md`

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- "Mama Claude" sub-agent architecture — sub-agents are "just a recursive Claude Code"
- Event-driven streaming — tools execute as they stream from the API
- Concurrent tool partitioning via `isConcurrencySafe` annotations

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- Edit tool uses find-and-replace (`old_string` → `new_string`)
- 40+ specialized tools with rich type annotations
- Skills system: organization, project, and dynamic levels

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Swiss cheese model — "no one perfect answer, always a Swiss cheese model"
- Permission prompts invented with Ben Mann when safety teams blocked bash
- Conservative defaults — `find` not pre-allowed due to code execution flags
- YOLO classifier for auto-approval

### [04 - Context Management](references/dimensions/04-context-management.md)
- Dream system — 4-phase background memory consolidation (autoDream)
- Multi-strategy compaction: snip, micro, reactive
- CLAUDE.md hierarchy: Managed → User → Project → Local
- RAG abandoned: "Agentic search just outperformed everything… glob and grep"

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- Coordinator Mode (internal) — multi-agent orchestration with parallel phases
- AgentTool types: general-purpose, Explore, Plan, claude-code-guide

### [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)
- KAIROS (proactive assistant), Bridge Mode, ULTRAPLAN, Voice Mode, Buddy
- Dual build: compile-time gates + runtime GrowthBook flags
- Feature velocity: tools shipped and unshipped weekly

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- "Don't put the model in a box" — corollary of the Bitter Lesson
- "Build for the model of 6 months from now"
- "The model just wants to use tools"

## Notable Quotes

> "For things like safety and security, there's no one perfect answer. It's always a Swiss cheese model."

> "Agentic search just outperformed everything… and when I say agentic search, this is a fancy word for glob and grep."

> "Don't build for the model of today, build for the model 6 months from now."

> "All of Claude Code has been written and rewritten and rewritten over and over."

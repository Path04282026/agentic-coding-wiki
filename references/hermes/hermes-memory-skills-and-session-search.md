---
tags: [hermes, deep-dive, context-management, memory, skills]
product: Hermes Agent
date: 2026-04-25
dimensions: [04, 05, 07, 08, 09]
source_count: 2
---

# Hermes Memory, Skills, and Session Search

> A deep dive on how [Hermes Agent](references/entities/hermes-agent.md) separates durable memory, procedural skills, searchable session history, and project context. This page is grounded in [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md) and extends the context-management analysis in [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md).

## Thesis

Hermes’s context architecture is strongest when treated as a **layered recall system**, not a single “memory” feature. The key design move is separation:

- **memory** stores compact durable facts and preferences
- **session search** recalls past conversations without permanently injecting them into future context
- **skills** store reusable procedures and workflows
- **project context files** define local rules for a repository or workspace
- **current conversation context** carries the active task state

This separation reduces context pollution and makes Hermes more suitable for long-running, cross-session work than a flat memory bucket would be. The external Hermes article adds a stronger self-evolution layer: completed trajectories can be reviewed asynchronously, converted into skills, and in advanced workflows prepared as RL training data. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md) [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

## 1. The Four Persistence Layers

| Layer | What it stores | Typical lifetime | Primary risk | Hermes design rule |
|---|---|---|---|---|
| Durable memory | Stable user preferences, environment facts, durable conventions | Long-term | Pollution by temporary task state | Save compact declarative facts only |
| Session search / conversation persistence | Past conversations, transcripts, and summaries | Historical, query-time | Over-reliance on stale prior context | Retrieve on demand; do not treat as global instruction |
| Skills | Procedures, workflows, tool-specific methods | Long-term, maintained | Stale or wrong procedure | Patch skills when issues are discovered |
| Dynamic skill review | Completed trajectories and reusable task patterns | Background / post-task | Extracting bad habits into future behavior | Review, validate, and keep skills editable |
| Project context | Repo-specific governance and schema such as `CLAUDE.md` | Project-scoped | Conflicts with current task or source facts | Read before modifying project files |
| Training trajectories | Cleaned/compressed execution traces | Dataset lifecycle | Privacy leakage or low-quality training examples | Curate, filter, and evaluate before training |

The important product-design insight is that Hermes does not need one universal context mechanism. It needs clear routing: **facts go to memory, procedures go to skills, past work stays searchable, project rules live with the project, and training examples require explicit curation**.

## 2. Durable Memory: Facts, Not Logs

Hermes’s durable memory is for information that should influence future sessions. In this wiki context, good memory candidates would be stable preferences or durable environment facts, not the fact that one page was just edited.

Good memory examples:

- “User prefers Chinese discussion for this wiki project.”
- “The Agentic Coding Tools wiki treats `raw/` as immutable and generated synthesis as `wiki/` workspace.”
- “Hermes Agent is currently represented as a case study, not a fourth first-class product column.”

Bad memory examples:

- “Today we created two pages.”
- “Current todo item d3 is in progress.”
- “The latest verification script succeeded.”

Those bad examples are task progress and should remain in logs, todos, or session history rather than durable memory. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)

## 3. Session Search: Recall Without Pollution

`session_search` gives Hermes a way to recover prior work without saving everything into durable memory. This matters because many user references are contextual but not permanently important:

- “上次我们怎么处理 Hermes 的？”
- “之前那个 OpenClaw 页面在哪？”
- “你还记得我们为什么没把 Hermes 加成第四列吗？”

In these cases Hermes can search past sessions, summarize the relevant context, and use it for the current task. The past session does **not** have to become a permanent instruction.

**Design implication:** session search should be framed as *retrieval on demand*, while memory should be framed as *future default context*. This distinction prevents temporary decisions from turning into global behavior.

## 4. Skills: Procedural Memory

Skills are Hermes’s procedural layer. They are not facts about the user; they are reusable methods for doing work.

In this wiki project, the key skill is **Agentic Coding Product Design Patterns** at the content layer and `llm-wiki-authoring` at the procedural layer. The loaded skill tells Hermes how to:

1. gather evidence before drafting
2. write pages with frontmatter and wikilinks
3. update `wiki/index.md`
4. append `wiki/log.md`
5. verify page count and broken links
6. avoid modifying `raw/`

This is different from memory. A memory might say that the project has immutable raw sources; a skill says how to perform a wiki authoring workflow safely.

## 5. Project Context: Local Law Beats Generic Habit

Project context files such as `CLAUDE.md` define local operating rules. For this wiki, `CLAUDE.md` establishes:

- the 3-layer architecture: `raw/`, `wiki/`, and repo governance
- the 9 comparative dimensions
- the original three-product schema: [Claude Code](references/entities/claude-code.md), [Codex](references/entities/codex.md), [OpenClaw](references/entities/openclaw.md)
- page frontmatter conventions
- `wikilink` usage
- mandatory `wiki/log.md` updates
- no modifications to `raw/`

This layer is not exactly memory and not exactly a skill. It is **workspace law**: the agent must read it and obey it before making local changes. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)

## 6. Hermes vs Existing Context-Management Patterns

| Aspect | Claude Code | Codex | OpenClaw | Hermes Agent |
|---|---|---|---|---|
| Project instructions | Hierarchical `CLAUDE.md` | Flat `AGENTS.md` | `Soul.md` + custom docs | Project context files injected into workspace task |
| Durable memory | Dream system / file memory | SQLite state and active research | Markdown + vector DB experiments | Compact persistent memory tool |
| Session recall | Conversation + compaction | History/state DB | Personal system history | `session_search` across past sessions |
| Procedural reuse | Skills / plugins | Skills crate / MCP workflows | Plugins / self-modification | Skills loaded and patched by agent |
| Main risk | Over-instruction and stale memory | Thin memory model | Self-aware sprawl | Mixing facts, procedures, and task progress |

Hermes’s distinctive move is the explicit separation of **durable facts**, **procedures**, and **past sessions**. This maps directly to the design pattern “Compaction and Memory Are Different Problems.” **Agentic Coding Product Design Patterns**

## 7. Context Routing Rules for Hermes

A practical Hermes design guide can be expressed as routing rules:

| If the information is... | Put it in... | Example |
|---|---|---|
| A stable user preference | `memory` | User prefers Chinese for this project |
| A durable project convention | `memory` or project context, depending on scope | Wiki `raw/` is immutable |
| A workflow with steps | `skill_manage` | How to author and verify a wiki page |
| A prior conversation outcome | `session_search` retrieval | Why Hermes is a case study first |
| A current task checklist | `todo` | Create two deep-dive pages |
| A source-backed claim | `wiki/sources/` or synthesis page | Hermes runtime capability note |
| A local repository rule | project context file | `CLAUDE.md` schema |
| A maintenance event | `wiki/log.md` | Pages created and index updated |

These routing rules are a strong candidate for becoming a Hermes product feature: the agent could explain where it is storing information and why.

## 8. Design Risks

### Risk 1 — Memory Pollution

If Hermes writes temporary status into durable memory, future sessions will inherit stale state.

**Mitigation:** memory entries should be compact, declarative, and future-relevant; task progress belongs in logs or session history.

### Risk 2 — Skill Staleness

Skills can become wrong as tools or project conventions change.

**Mitigation:** Hermes should patch skills immediately when a workflow fails or a missing pitfall is discovered. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)

### Risk 3 — Session Search Overreach

Past sessions may contain decisions that were valid only under old constraints.

**Mitigation:** treat session search as evidence to inspect, not as automatically authoritative memory.

### Risk 4 — Project Context Conflicts

A project file can conflict with user’s latest instruction or with source evidence.

**Mitigation:** use project context as governance, but surface conflicts explicitly when user intent would violate schema or safety rules.

## 9. Product Opportunities

Hermes could improve this layer with:

- a **context router UI** showing whether something was stored as memory, skill, log, or source
- a **memory review mode** to inspect and prune durable facts
- a **skill freshness check** that detects unused or failing procedures
- a **session-search citation mode** that turns recalled prior work into source-backed notes
- a **project-context conflict detector** for cases where user requests conflict with `CLAUDE.md`/`AGENTS.md`
- a **context budget dashboard** separating active context, loaded skills, retrieved sessions, and durable memory

## Summary

Hermes should not be understood as “an agent with memory.” It is better understood as an agent with **multiple persistence channels**, each optimized for a different kind of knowledge. This is central to Hermes’s identity as an agent operating layer: it can carry intent across sessions without turning every past detail into a permanent instruction. [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)

## Related Pages

- [Hermes Agent](references/entities/hermes-agent.md)
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md)
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md)
- [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)
- [04 - Context Management](references/dimensions/04-context-management.md)
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- **Agentic Coding Product Design Patterns**

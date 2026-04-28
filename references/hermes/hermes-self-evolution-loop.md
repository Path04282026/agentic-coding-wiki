---
tags: [hermes, deep-dive, self-evolution, skills, training]
product: Hermes Agent
date: 2026-04-25
dimensions: [01, 02, 04, 05, 07, 08, 09]
source_count: 2
---

# Hermes Self-Evolution Loop

> Deep dive on Hermes Agent’s self-evolution thesis: how execution traces can become skills, memory, training data, and eventually better models. Primary sources: [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md) and [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md).

## Thesis

The strongest synthesis is:

> [Hermes Agent](references/entities/hermes-agent.md) is a **self-evolving agent operating layer**: an action runtime that can execute work across tools and platforms, while also turning completed work into reusable procedural assets and, in more advanced workflows, training data for model improvement.

This upgrades the earlier wiki thesis from “agent operating layer” to a two-part formulation:

1. **Operating layer** — Hermes turns chat intent into verified actions across files, browsers, shells, schedules, messages, memory, skills, and subagents. [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
2. **Self-evolution loop** — Hermes can review trajectories, generate or update skills, preserve selected memory, compress trajectories, and support RL-style model improvement workflows. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

## 1. The Two Evolution Paths

The article frames Hermes’s self-evolution as “内外双路径” — an external/context-level path and an internal/weight-level path. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

| Path | What changes | Mechanism | Strength | Risk |
|---|---|---|---|---|
| External evolution | Context assets around the model | Dynamic skills, memory review, background review agents | Fast, explainable, editable | Can become stale or polluted |
| Internal evolution | Model behavior/weights | Trajectory capture, data synthesis, compression, RL/GRPO training | Can improve smaller/domain models | Expensive, quality-sensitive, privacy-sensitive |

This distinction matters because not all “learning” is the same. A generated skill changes Hermes’s future context. RL training changes the model that Hermes may later use.

## 2. External Evolution — Dynamic Skill Generation

The article’s most concrete Hermes differentiator is dynamic skill generation. Instead of treating skills as static packages written by humans, Hermes can review completed work and extract reusable procedures. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

A typical loop looks like:

1. Hermes completes a complex task.
2. The execution trajectory contains mistakes, corrections, effective steps, and user-validated patterns.
3. A review process identifies reusable structure.
4. Hermes creates or updates a skill package.
5. Future similar tasks can load that skill instead of rediscovering the path.

This turns the design pattern “skills as procedural memory” into a stronger pattern: **skills as accumulated operational experience**. [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md)

## 3. Skill Nudge and Background Review

The article names two mechanisms that make skill evolution more automatic:

- `_iters_since_skill` — tracks how many turns have passed since the last skill-management action
- `_skill_nudge_interval = 10` — nudges the agent to consider whether recent experience should become or update a skill [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

It also describes `_spawn_background_review`, which asynchronously starts a lightweight review agent after a foreground response. That review agent can inspect the recent interaction from three angles:

| Review type | Question | Output |
|---|---|---|
| Memory review | What should be remembered? | durable facts or preferences |
| Skill review | Is there a reusable task pattern? | new/updated skill |
| Combined review | What could be improved? | process critique / future improvement |

This is important product design: the user gets a fast foreground answer, while improvement work can happen asynchronously in the background.

## 4. Internal Evolution — Trajectory to Training Data

The article’s second path is weight-level improvement through training workflows. It describes trajectories as complete records of agent execution: system prompts, user prompts, assistant reasoning/actions, tool calls, tool results, and final outputs. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

The reported pipeline is:

```text
agent trajectory
  → ShareGPT-style conversation data
  → reasoning / scratchpad normalization
  → quality filtering
  → trajectory compression
  → RL environment / config
  → small experiment
  → larger training
  → evaluation
  → accepted model or next iteration
```

The article mentions files and components such as `agent/trajectory.py`, `batch_runner.py`, `mini_swe_runner.py`, `trajectory_compressor.py`, and `rl_cli.py` as parts of this pipeline.

## 5. Why Trajectory Compression Is Different From Context Compression

The article distinguishes two compression problems:

| Compression type | When it runs | Goal | Typical strategy |
|---|---|---|---|
| Runtime context compression | During an active conversation | Keep the task going within context limits | relative threshold, preserve head/tail, summarize middle |
| Offline trajectory compression | After trajectories are collected | Prepare efficient training data | tokenizer-counted target length, preserve task start/end, summarize middle |

This distinction should be added to the wiki’s context-management vocabulary. Runtime compression is about continuity; trajectory compression is about dataset quality and training cost. [04 - Context Management](references/dimensions/04-context-management.md)

## 6. RL Is Not “Train on All User Chats”

One of the article’s most useful clarifications is negative: Hermes’s RL value should not be understood as blindly training on raw user chat history. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

The article gives two reasons:

1. **Privacy** — user conversations may include sensitive data.
2. **Quality** — raw user conversations are noisy and can degrade a model if used without filtering.

Instead, the article frames the practical value of RL as:

- distilling stronger teacher-model behavior into smaller/local models
- lowering inference cost
- improving latency
- supporting local/offline/compliance-sensitive deployments
- specializing a model for a domain-specific reward function

This is a good evidence boundary for the wiki: “self-evolution” does not mean indiscriminate continuous online learning from every user interaction.

## 7. GRPO and Reward Design

The article says Hermes uses GRPO-style training workflows and highlights reward design as the core of RL usefulness. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

The described reward-design principles include:

- combine 3–5 reward functions
- weight correctness highest
- include format rewards where useful
- allow partial credit
- test reward functions independently before combining

Most importantly, rewards can use tools. A reward function can compile code, run tests, inspect files, query networks, or use browsers. This turns reward design into harness design: correctness can be measured through real-world verification rather than only final text comparison. [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md)

## 8. Context Injection as a Speed Layer

The article’s `@`-style context injection is a separate design pattern: instead of waiting for the agent to discover and call tools, the user can mount context directly into the prompt. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

Examples include:

- `@file:main.py`
- `@file:src/utils.py:10-20`
- `@folder:src/`
- `@diff`
- `@staged`
- `@git:3`
- `@url:https://...`

This converts tool calls into **context preloading**. It reduces latency and ambiguity, but creates a new product-design question: how should Hermes make injected context provenance visible so the model does not confuse recalled or mounted context with new user intent?

## 9. Prompt / Context / Harness Reframing

The article’s analysis uses a different three-part frame from the wiki’s 9 dimensions:

| Article frame | Wiki dimensions it maps to | Hermes-specific insight |
|---|---|---|
| Prompt Engineering | 01 Agent Loop, 02 Tool Systems, 04 Context Management | model-specific tool-use enforcement and ecosystem compatibility |
| Context Engineering | 04 Context Management, 08 Shared Principles | relative-threshold compression, memory wrapping, external memory, context injection |
| Harness Engineering | 03 Security, 05 Multi-Agent, 07 Internals | hooks, error classification, recovery strategies, subagent restrictions, skill scans |
| Self-Evolving | 01, 04, 05, 07, 09 | background review, skill generation, trajectory-to-RL loop |

This mapping lets the article become a source in the wiki without replacing the 9-dimension structure.

## 10. Hermes vs Existing Products

| Aspect | Claude Code | Codex | OpenClaw | Hermes Agent |
|---|---|---|---|---|
| Core identity in current wiki | Tool-rich coding agent | Verifiable coding harness | Self-modifying personal coding assistant | Self-evolving agent operating layer |
| Skill model | Skills as reusable procedures | Skills crate / workflows | Plugins and self-modification | Dynamic skill generation and refinement |
| Training loop | Not central in filed wiki evidence | Model/product evals and harness discipline | Article cites OpenClaw-RL as related work | Trajectory capture + RL workflow emphasized |
| Context injection | `CLAUDE.md` / agentic search | `AGENTS.md` / shell exploration | docs and runtime awareness | `@file`, `@folder`, `@diff`, `@url` style preloading |
| Evolution mechanism | Model improves externally; skills/configs evolve | Harness and model releases evolve | Self-modifying code/plugins | Skills + background review + trajectory/RL pipeline |

## 11. Evidence Boundaries and Cautions

The article is valuable but should not be over-interpreted:

- It is an external analysis article, not an official specification.
- Source-file claims should be verified against the Hermes repository if used for high-stakes comparison.
- “Self-evolving” includes both context-level evolution and weight-level training; only the latter changes model weights.
- Dynamic skills can preserve mistakes if review quality is poor.
- RL training on raw user data is explicitly risky because of privacy and quality concerns.
- Harness guardrails are not the same as kernel-level sandboxing.

## Product Design Takeaways

1. **Self-evolution needs routing.** Memory, skills, session history, and training data should not collapse into one bucket.
2. **Skills are the fastest evolution path.** They are cheap, editable, and immediately reusable.
3. **RL is the expensive evolution path.** It is useful for distilled/local/domain models, but needs high-quality data and evaluation.
4. **Background review is the bridge.** It turns normal interactions into improvement opportunities without blocking the user.
5. **Context injection is UX leverage.** Users can help the agent skip discovery when they already know which artifacts matter.
6. **Guardrails must cover generated assets.** A self-evolving system needs safety checks not only for tools, but also for newly generated skills and training data.

## Summary

The article changes the Hermes thesis from simply “agent operating layer” to **self-evolving agent operating layer**. The operating layer explains Hermes’s surface area; the self-evolution loop explains why the system may improve over time through dynamic skills, background review, trajectory capture, and RL workflows. [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)

## Related Pages

- [Hermes Agent](references/entities/hermes-agent.md)
- **Hermes Agent Case Study**
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md)
- [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md)
- [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md)
- [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)
- **Agentic Coding Product Design Patterns**

---
tags: [source, article, analysis]
product: Hermes Agent
date: 2026-04-24
dimensions: [01, 02, 03, 04, 05, 06, 07, 08, 09]
author: 飞樰
publication: 阿里云开发者
raw: https://mp.weixin.qq.com/s/2xFei8dMx99lc-iyrZZrww
---

# 深度解析 Hermes Agent 如何实现“自进化”及其 Prompt / Context / Harness 的设计实践

> Source summary for a WeChat/阿里云开发者 article by 飞樰. The article analyzes [Hermes Agent](references/entities/hermes-agent.md) as a persistent, self-evolving agent system and compares parts of its Prompt Engineering, Context Engineering, and Harness Engineering with [OpenClaw](references/entities/openclaw.md) and [Claude Code](references/entities/claude-code.md).

## Scope and Evidence Boundary

This page summarizes the article’s claims and technical interpretation. It should be treated as an **external technical analysis article**, not as an official Hermes specification. Some claims are based on the author’s reading of Hermes source files, README, and project behavior; stronger claims such as “self-evolving” and “AGI” framing should be cited as article perspective unless independently verified.

The source is external and was not copied into `raw/`; `raw/` remains immutable under the repository schema.

## Core Thesis

The article’s central claim is that Hermes Agent’s differentiator is **self-evolution**: Hermes is not only a persistent action agent with tools, memory, cron, and messaging surfaces, but a system that can convert execution history into reusable skills and, in a deeper loop, use trajectories for reinforcement-learning-based model improvement.

The article frames Hermes’s evolution as two coupled paths:

1. **External / context-level evolution** — dynamic skill generation and continuous skill refinement.
2. **Internal / weight-level evolution** — trajectory capture, data synthesis, compression, RL training, evaluation, and model iteration.

## Key Takeaways by Dimension

### 01 — Agent Loop

The article frames Hermes’s loop as moving beyond “one-shot task execution” into a self-improving cycle: execute a task, record trajectory, review the outcome, extract reusable experience, update skills, and optionally feed cleaned trajectories into model-training workflows. [01 - Agent Loop](references/dimensions/01-agent-loop.md)

This extends the existing wiki’s “act-and-verify” framing with an additional **act-review-improve** layer.

### 02 — Tool Systems

The article says Hermes supports 40+ built-in tools, multiple models, cron scheduling, third-party messaging access, and model-specific tool-use guidance. It highlights dynamic prompt patches for different model families: GPT/Codex-like models may need stronger “use tools, do not just describe” instructions, while Gemini/Gemma-like models may need reminders about absolute paths, read-before-edit, and parallel tool use. [02 - Tool Systems](references/dimensions/02-tool-systems.md)

The article also identifies RL toolsets and tool-enabled reward functions as important: reward functions can execute commands, read files, access networks, or use browsers to verify outcomes rather than relying only on text matching.

### 03 — Security and Sandboxing

The article emphasizes harness-level guardrails rather than kernel-level sandboxing. It mentions prompt-injection defenses, skill security scanning before loading generated/external skill files, controlled subagent restrictions, and lifecycle hooks for custom checks. [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)

This supports a stronger application-layer safety story than the initial runtime source note, while still not proving a Codex-style OS/kernel sandbox.

### 04 — Context Management

The article gives a detailed context-management account:

- real-time context compression triggered by relative context-window thresholds rather than fixed token counts
- head/tail preservation with middle summarization
- separate offline trajectory compression for training-data preparation
- SQLite persistence for daily conversation history
- Markdown memory files such as `MEMORY.md` / `USER.md`
- external memory services such as Mem0, Honcho, Hindsight, and Supermemory
- explicit `<memory-context>` wrapping to distinguish recalled memory from new user input
- direct context injection via `@file`, `@folder`, `@diff`, `@staged`, `@git`, and `@url` [04 - Context Management](references/dimensions/04-context-management.md)

The article distinguishes runtime context compression from offline trajectory compression: runtime compression keeps the conversation going; trajectory compression prepares high-quality training data.

### 05 — Multi-Agent Architecture

The article identifies a background review agent spawned after the main agent finishes a response. This background process reviews the conversation for memory-worthy facts, skill-worthy patterns, and general improvement opportunities. [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)

It also describes subagent controls: blocked tools such as `delegate_task`, `clarify`, `memory`, `send_message`, and `execute_code`; a maximum number of concurrent children; and a maximum delegation depth. These constraints prevent recursive delegation loops, memory manipulation, and message hijacking.

### 06 — UI and Distribution

The article highlights Hermes’s third-party messaging platform access and its similarity to OpenClaw’s messaging-oriented usage. It also argues that Hermes lowers migration friction by supporting OpenClaw-style configuration files and AI coding ecosystem files such as `CLAUDE.md`, `.cursorrules`, and `.cursor/rules/*.mdc`. [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)

### 07 — Feature Gating and Internals

The article surfaces several internal mechanisms:

- `_iters_since_skill` tracks how many turns have passed since skill management
- `_skill_nudge_interval = 10` nudges the agent to consider creating/updating skills
- `_spawn_background_review` starts asynchronous review agents
- `agent/trajectory.py` converts trajectories to ShareGPT-style data
- `batch_runner.py` creates batch trajectory data
- `mini_swe_runner.py` targets SWE benchmark-style tasks
- `trajectory_compressor.py` compresses trajectories for training
- `rl_cli.py` orchestrates RL training workflows
- `agent/error_classifier.py` classifies errors into 14 categories [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md)

### 08 — Shared Principles

The article reinforces several cross-product patterns already present in the wiki: thin agent loops with rich tools, context compression, skills, hooks, subagents, messaging surfaces, and layered safety. [08 - Shared Principles](references/dimensions/08-shared-principles.md)

Its strongest new contribution is the pattern of **background learning from trajectories**: completed interactions become a resource for skill generation and possible training-data generation.

### 09 — Philosophical Divergence

The article’s philosophical label for Hermes is **Self-Evolving Agent**. It positions Hermes as a step beyond autonomous agents such as Claude Code and OpenClaw: not only capable of planning and tool use, but also capable of learning from execution history through dynamic skills and RL pipelines. [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

This complements the wiki’s prior label, **agent operating layer**. A synthesis label would be: **self-evolving agent operating layer**.

## Specific Mechanisms Highlighted

### Dynamic Skill Generation

The article says Hermes can review completed tasks, extract key steps, pitfalls, corrections, and validated best practices, then turn those into structured skill packages. Skills are presented as dynamic assets rather than static pre-installed workflows.

### Background Review Agent

After a foreground answer, Hermes can asynchronously spawn a background review agent. The article names three review prompts:

- memory review: what should be remembered?
- skill review: should this task pattern become a skill?
- combined review: what could be improved?

### RL Training Loop

The article describes a pipeline in which Hermes captures agent trajectories, converts them to ShareGPT format, filters incomplete reasoning traces, compresses long trajectories, and uses RL workflows such as GRPO to train or improve smaller/local models.

The article cautions that the main value of RL is not automatically training on raw user conversations. Instead, it emphasizes teacher-model distillation, benchmark or curated data, privacy protection, quality filtering, cost reduction, faster local inference, and domain-specific optimization.

### Context Injection

The article highlights `@`-style context injection as an efficiency mechanism: users can directly mount files, folders, diffs, staged changes, git history, or URLs into the prompt context, reducing the need for multi-turn tool discovery.

### Harness Engineering

The article frames Hermes’s harness as a production-oriented control layer with hooks, error classification, recovery strategies, subagent restrictions, plugins, prompt-injection defense, and skill safety scanning.

## Notable Quotes / Claims

> “Hermes 主打的核心亮点非常明确：‘持久运行’（Persistent）和‘自进化’（Self-Evolving）。” — 飞樰

> “Skill从‘静态调用’变成了‘动态生成’。” — 飞樰

> “如果说 Skill 生成是‘记笔记’，那么 RL 训练就是‘练内功’。” — 飞樰

> “这种机制的本质，是将‘工具调用’转化为‘上下文预加载’。” — 飞樰

## How This Changes the Hermes Wiki

This article strengthens the Hermes case study in three ways:

1. It adds source-backed evidence for **self-evolution** as a Hermes thesis.
2. It provides concrete mechanisms for context management, skill generation, trajectory capture, RL training, hooks, and guardrails.
3. It suggests that the Hermes thesis should evolve from **agent operating layer** to **self-evolving agent operating layer**, while preserving evidence boundaries around external article claims.

## Related Pages

- [Hermes Agent](references/entities/hermes-agent.md)
- **Hermes Agent Case Study**
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md)
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md)
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md)
- [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md)
- [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md)

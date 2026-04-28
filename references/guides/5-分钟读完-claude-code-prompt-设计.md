---
tags: [guide, chinese]
product: Claude Code
date: 2026-04-14
dimensions: [01, 04]
---

# 5 分钟读完 Claude Code Prompt 设计

!**Claude Code Prompt 设计总览.excalidraw**

> 一句话版：Claude Code 的 prompt 不是“一段写死的大提示词”，而是一套**分层装配、按需注入、持续压缩**的上下文系统。它既依赖 `CLAUDE.md` 这样的项目说明，也依赖运行时环境、工具说明、记忆和压缩机制共同工作。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md) [04 - Context Management](references/dimensions/04-context-management.md) [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

## 先看结论

如果你只想快速抓住 Claude Code 的 prompt 设计，可以先记住 4 点：

1. **Prompt 是装配出来的，不是手写一整段的。** Claude Code 会把默认系统提示、环境信息、用户记忆、`CLAUDE.md`、工具说明等内容组合起来，再发给模型。[04 - Context Management](references/dimensions/04-context-management.md) [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
2. **`CLAUDE.md` 是项目级 prompt 的核心接口。** 它会自动进入每次对话，让 Claude 反复获得你的架构、规范、命令和工作流。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
3. **Prompt 设计目标不是“把模型关进盒子”，而是让模型在足够上下文里自主用工具。** 这和 Boris Cherny 的设计哲学一致：少做僵硬脚手架，多给高质量上下文与工具。[interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md) [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
4. **长任务能跑下去，靠的不是超长静态 prompt，而是 compaction（压缩）与 memory（记忆）。** Claude Code 会在上下文变长时做摘要、裁剪和记忆整理。[04 - Context Management](references/dimensions/04-context-management.md) [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md)

## 1. Claude Code 的 prompt，到底在设计什么？

传统理解里的 prompt，往往只是“给模型一段说明”。但 Claude Code 更接近一个**运行时上下文编排器**：

- 一部分内容是稳定的角色与规则；[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- 一部分内容是当前会话的环境信息，比如目录、系统、shell、git 状态；[04 - Context Management](references/dimensions/04-context-management.md)
- 一部分内容来自用户和项目，例如 `CLAUDE.md`、记忆文件和语言偏好；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md) [04 - Context Management](references/dimensions/04-context-management.md)
- 还有一部分并不塞进主系统提示，而是附着在各个 tool 的描述里，告诉模型“什么时候该用这个工具、怎么安全地用”。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

所以，Claude Code 的 prompt 设计，本质上是在回答一个问题：**怎样让模型每一轮都拿到“刚好够用”的上下文，同时不过度浪费 token。** [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md)

## 2. `CLAUDE.md` 为什么这么关键？

Anthropic 对 `CLAUDE.md` 的定义非常直接：它是一个项目里的特殊配置文件，会自动进入 Claude 的系统提示，让每次会话都带着项目上下文启动。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

它适合放什么？

- 项目简介与架构图景；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- 关键目录；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- 主要依赖和非标准组织方式；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- 常用命令；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- 代码风格与测试要求；[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- 团队工作流，比如 explore → plan → code → commit。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

它为什么重要？因为它把“每次都得重新解释”的东西，变成了“默认就知道”的东西。对 Claude Code 来说，这比写一段花哨的万能 prompt 更有价值。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

Boris 也提到，`CLAUDE.md` 其实是从用户行为里自然长出来的；而且随着模型变强，你往往需要写的说明会越来越少。[interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md) [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)

## 3. Claude Code 的 prompt 不是越长越好，而是越“分层”越好

从代码分析页看，Claude Code 的系统提示采用一种**优先级级联（priority cascade）**：某些场景下会直接换用 coordinator prompt、agent prompt 或自定义 prompt；其余场景才回落到默认系统提示。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

更关键的是，它把提示内容切成了两类：

- **静态部分**：角色、行为规则、风格、任务原则、工具偏好；[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- **动态部分**：会话引导、记忆、环境信息、语言偏好、MCP 说明等。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

这样设计的好处有两个：

1. **可缓存。** 静态部分变化少，适合 prompt cache；[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
2. **可更新。** 动态部分每轮按需刷新，不必把整份 prompt 每次都重算。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

换句话说，Claude Code 在做的不是“写更大的 prompt”，而是“把 prompt 做成一个可维护的系统”。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

## 4. 工具说明也属于 prompt 设计的一部分

Claude Code 的一个重要判断是：模型天然会想用工具。Boris 的原话是，“The model just wants to use tools.” [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)

这直接影响了 prompt 设计：

- 主系统提示负责定义总体行为边界；[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- 每个工具再有自己的 `prompt.ts` 或说明文本，告诉模型该怎么用；[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- 于是“如何读文件”“如何改文件”“如何安全执行 bash”“何时开 subagent”这类细节，不必都塞进一段巨大的总 prompt。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)

这是一种很工程化的思路：**把 prompt 拆到最接近行为发生的位置。**

## 5. 真正拉开差距的，是“上下文管理”，不是一句神奇咒语

很多人讨论 prompt 设计时，会把重点放在“开场白怎么写”。但对 Claude Code 来说，更重要的是：**长任务如何不失焦。**

Claude Code 在这件事上主要靠 3 个机制：

### 5.1 用 `/clear` 和 subagent 保持上下文清爽

官方文档建议，在切换任务时使用 `/clear`，因为旧的调试信息、命令输出和无关对话会持续占满上下文。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

如果一个任务分成不同阶段，比如“先实现，再安全审查”，则适合用 subagent，把上下文隔离开。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

### 5.2 用 compaction 压缩长对话

在 [04 - Context Management](references/dimensions/04-context-management.md) 和 [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) 中，Claude Code 被描述为采用多层压缩：轻量清理旧工具结果、达到阈值后自动摘要、必要时保留最近文件附件与计划信息。这样做的目的，是在长时任务中维持“几乎无限上下文”的体验。[04 - Context Management](references/dimensions/04-context-management.md) [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md)

### 5.3 用 memory 保留真正该长期记住的事

Claude Code 还有 Dream / autoDream 这类记忆整理机制，会把跨会话仍然重要的信息整理进记忆系统，而不是让每次对话都重复携带全部原始历史。[interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md) [04 - Context Management](references/dimensions/04-context-management.md)

所以，Claude Code 的 prompt 设计不是“让一次提示词包打天下”，而是：

> **把稳定规则留在系统层，把项目知识留在 `CLAUDE.md`，把会话噪音交给压缩，把长期事实交给记忆。**

## 6. 这套设计背后的哲学是什么？

Boris 在多次访谈里都强调，不要把模型想成一个必须被死死限制的东西；更合理的做法是为更强的模型提前设计系统，并让它在工具与上下文中发挥能力。[interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md) [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)

这就解释了为什么 Claude Code 的 prompt 设计有几个明显特征：

- **少硬编码流程，多给可执行环境；** [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md) [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- **少依赖僵硬模板，多依赖项目文档与工具说明；** [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md) [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- **少追求一次写完，多强调持续演化。** 官方也明确建议把 `CLAUDE.md` 当成持续更新的实践，而不是一次性配置。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

## 7. 如果你想借鉴 Claude Code 的 prompt 设计，可以直接抄这 5 条

### 7.1 不要先写“万能总 prompt”
先把稳定内容拆开：角色规则、项目上下文、工具说明、工作流要求。[Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

### 7.2 给 agent 一份项目说明文件
哪怕不是 `CLAUDE.md` 这个名字，也应该有一份项目级说明：架构、目录、命令、规范、测试、禁区。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

### 7.3 工作流要写成可执行的默认路径
例如“先调研、再计划、再编码、最后验证”，比空泛地说“请认真思考”有效得多。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

### 7.4 长任务必须有压缩机制
没有 compaction，任何 prompt 设计最后都会输给上下文污染。[04 - Context Management](references/dimensions/04-context-management.md) [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md)

### 7.5 保持简洁，持续迭代
Anthropic 的建议很明确：从 prompt engineering / context engineering 的角度看，`CLAUDE.md` 要尽量简洁，只记录真正有用的东西。[blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)

## 最后一句话

**Claude Code 的 prompt 设计，最值得学的不是“写词技巧”，而是把 prompt 当作一个由系统提示、项目文档、工具描述、上下文压缩和长期记忆组成的工程系统。** [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md) [04 - Context Management](references/dimensions/04-context-management.md)

## 延伸阅读

- [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md)
- [04 - Context Management](references/dimensions/04-context-management.md)
- [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md)
- [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md)
- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)

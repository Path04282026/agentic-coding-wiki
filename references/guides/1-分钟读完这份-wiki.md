---
tags: [guide, chinese]
product: [Claude Code, Codex, OpenClaw]
date: 2026-04-14
dimensions: [08, 09]
---

# 1 分钟读完这份 wiki

> 一句话版：这不是一个“新闻合集”，而是一套把 [Claude Code](references/entities/claude-code.md)、[Codex](references/entities/codex.md)、[OpenClaw](references/entities/openclaw.md) 放进同一分析框架里比较的 Obsidian wiki。你真正该先看的不是 100 多篇 source 页面，而是 **9 个维度页 + 1 个总对比页**。[overview](references/overview.md) [index](references/hermes/index.md) [Three-Way Comparison](references/comparisons/three-way-comparison.md)

## 先记住 3 件事

1. **这份 wiki 的目标，不是收集资料，而是回答 9 个固定比较问题。** 它围绕 agent loop、工具系统、安全、上下文管理、多代理、分发渠道、内部机制、共同原则、哲学分歧这 9 个维度组织内容。[overview](references/overview.md) [01 - Agent Loop](references/dimensions/01-agent-loop.md) [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
2. **最值得优先读的是“维度页”，不是 source 页。** `wiki/sources/` 更像证据库；真正的结论被收敛在 9 个维度页和 [Three-Way Comparison](references/comparisons/three-way-comparison.md) 里。[index](references/hermes/index.md) [Three-Way Comparison](references/comparisons/three-way-comparison.md)
3. **这三款产品的差异，本质上是三种设计赌注。** [Claude Code](references/entities/claude-code.md) 更像“相信模型”，[Codex](references/entities/codex.md) 更像“相信模型，但要可验证”，[OpenClaw](references/entities/openclaw.md) 更像“把 agent 当成可自我改造的工程团队”。[09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

## 如果你只有 1 分钟，按这个顺序读

### 路线 A：只想抓全貌
1. 先看 [overview](references/overview.md)：知道这份 wiki 在比较谁、为什么这样分层。[overview](references/overview.md)
2. 再看 [Three-Way Comparison](references/comparisons/three-way-comparison.md)：一页看完三者在 9 个维度上的核心差别。[Three-Way Comparison](references/comparisons/three-way-comparison.md)
3. 最后按兴趣跳到一个维度页深入。最推荐从 [04 - Context Management](references/dimensions/04-context-management.md)、[05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)、[09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) 开始。[index](references/hermes/index.md)

### 路线 B：只想知道“结论是什么”
- 想看**共同趋势**：读 [08 - Shared Principles](references/dimensions/08-shared-principles.md)。你会看到三者虽然路线不同，但都在收敛到 CLI-first、MCP、分层安全、sub-agents 等共识。[08 - Shared Principles](references/dimensions/08-shared-principles.md)
- 想看**根本分歧**：读 [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)。这是理解三者为什么长成今天这个样子的最快入口。[09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

## 这份 wiki 的正确打开方式

### 1. 把 source 页当“证据”，不要当“结论”
`wiki/sources/` 里是一篇篇博客、访谈、摘要的整理页；它们很有用，但更适合在你想追证某个观点时再回去看。日常阅读先看维度页，效率最高。[index](references/hermes/index.md)

### 2. 把 9 个维度当“导航栏”
这份 wiki 的真正骨架不是时间线，而是 9 个分析维度。换句话说，它不是按“谁先发布了什么”来组织，而是按“这些 agent 到底在设计什么问题”来组织。[overview](references/overview.md) [01 - Agent Loop](references/dimensions/01-agent-loop.md) [02 - Tool Systems](references/dimensions/02-tool-systems.md)

### 3. 把三家产品看成三种路线，而不是三份功能表
[Three-Way Comparison](references/comparisons/three-way-comparison.md) 给你横向差异，[08 - Shared Principles](references/dimensions/08-shared-principles.md) 告诉你行业共识，[09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) 则解释这些差异背后的世界观。三页合起来，基本就能吃透这份 wiki 的核心。[Three-Way Comparison](references/comparisons/three-way-comparison.md) [08 - Shared Principles](references/dimensions/08-shared-principles.md) [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

## 你读完后大概会得到什么

- 对 agentic coding 工具的主流架构有一张脑图：agent loop、tooling、context、memory、sub-agents、distribution。[01 - Agent Loop](references/dimensions/01-agent-loop.md) [02 - Tool Systems](references/dimensions/02-tool-systems.md) [04 - Context Management](references/dimensions/04-context-management.md) [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- 对行业共识有把握：三者虽然风格不同，但都在走向更强的工具使用、更长任务、更深的上下文管理和更多代理协作。[08 - Shared Principles](references/dimensions/08-shared-principles.md) [Three-Way Comparison](references/comparisons/three-way-comparison.md)
- 对核心分歧有判断：到底应该“更信模型”、还是“更信 harness 与 sandbox”、还是“更信 agent 自我改造能力”。[09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)

## 最后一句话

**把这份 wiki 当成一张“agentic coding 设计地图”来读：先看 [overview](references/overview.md) 定位，再用 [Three-Way Comparison](references/comparisons/three-way-comparison.md) 抓总览，最后沿着 9 个维度挑你最关心的问题往下钻。** [overview](references/overview.md) [Three-Way Comparison](references/comparisons/three-way-comparison.md) [index](references/hermes/index.md)

## 延伸阅读

- [overview](references/overview.md)
- [index](references/hermes/index.md)
- [Three-Way Comparison](references/comparisons/three-way-comparison.md)
- [08 - Shared Principles](references/dimensions/08-shared-principles.md)
- [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- [04 - Context Management](references/dimensions/04-context-management.md)
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)

---
tags: [navigation]
title: Page Index
---

# Wiki Page Index

> Complete catalog of all pages in this wiki. Updated after each batch operation.

**Total pages: 156** | Last updated: 2026-04-27

## Navigation (4 pages)

- [overview](references/overview.md) — Start Here
- [timeline](references/timeline.md) — Chronological milestones
- [index](references/hermes/index.md) — This page
- [log](references/log.md) — Operations log

## Design Knowledge Base (4 pages)

- **Design Knowledge Base Index** — Product-design entry point under `wiki/design/`
- **Agentic Coding Product Design Questions** — Question-driven design checklist across the 9 dimensions
- **Agentic Coding Product Design Patterns** — Reusable product design patterns distilled from the three products
- **Prompt Context Harness Design Lens** — Synthesis of external Claude Code, OpenClaw, and Hermes articles through Prompt / Context / Harness

## Hermes Agent Case Study (5 pages)

- **Hermes Agent Case Study** — Case-study entry point for mapping Hermes into the 9-dimension framework
- [Hermes Agent Through the 9 Dimensions](references/hermes/hermes-agent-through-the-9-dimensions.md) — First-pass 9-dimension mapping of Hermes Agent
- [Hermes Tool System Deep Dive](references/hermes/hermes-tool-system-deep-dive.md) — Deep dive on Hermes as a tool-rich agent operating layer
- [Hermes Memory, Skills, and Session Search](references/hermes/hermes-memory-skills-and-session-search.md) — Deep dive on Hermes's layered persistence and context-routing model
- [Hermes Self-Evolution Loop](references/hermes/hermes-self-evolution-loop.md) — Deep dive on dynamic skills, background review, trajectories, and RL workflows

## Dimensions (9 pages)

| # | Page | Products | Source Count |
|---|---|---|---|
| 01 | [01 - Agent Loop](references/dimensions/01-agent-loop.md) | All three | 20 |
| 02 | [02 - Tool Systems](references/dimensions/02-tool-systems.md) | All three | 17 |
| 03 | [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) | All three | 14 |
| 04 | [04 - Context Management](references/dimensions/04-context-management.md) | All three | 16 |
| 05 | [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) | All three | 14 |
| 06 | [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md) | All three | 16 |
| 07 | [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) | All three | 10 |
| 08 | [08 - Shared Principles](references/dimensions/08-shared-principles.md) | All three | 16 |
| 09 | [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) | All three | 17 |

## Comparisons (2 pages)

- [Three-Way Comparison](references/comparisons/three-way-comparison.md) — Master comparison across all 9 dimensions
- [OpenClaw vs Hermes Agent](references/comparisons/openclaw-vs-hermes-agent.md) — Focused comparison of OpenClaw's messaging-native personal-agent design and Hermes's self-evolving agent operating layer

## Guides (2 pages)

- **5 分钟读完 Claude Code Prompt 设计** — Claude Code prompt / context 设计的中文 5 分钟导读
- **1 分钟读完这份 wiki** — 这份 LLM wiki 的中文 1 分钟导读

## Code Analysis — Implementation Deep Dives (11 pages)

- **Code Analysis Index** — Prompts, context, memory, and loop implementation map under `wiki/code-analysis/`

### Prompt Systems
- [Claude Code — Prompt System](references/code-analysis/prompts/claude-code-prompt-system.md) — Claude Code system prompt assembly and tool prompt structure
- [Codex — Prompt System](references/code-analysis/prompts/codex-prompt-system.md) — Codex prompt templates and instruction loading
- [OpenClaw — Prompt System](references/code-analysis/prompts/openclaw-prompt-system.md) — OpenClaw self-aware prompt and context-file pipeline

### Context and Memory
- [Claude Code — Context Management](references/code-analysis/context/claude-code-context-management.md) — Claude Code compaction, token budgeting, and context pipeline
- [Codex — Context Management](references/code-analysis/context/codex-context-management.md) — Codex CompactTask, environment context, and session prefixing
- [OpenClaw — Context Management](references/code-analysis/context/openclaw-context-management.md) — OpenClaw sessions, context, and agent-runner memory
- [Claude Code — Memory System](references/code-analysis/memory/claude-code-memory-system.md) — Claude Code Dream / memory subsystem analysis

### Agent Loops
- [Claude Code — Agent Loop](references/code-analysis/loops/claude-code-agent-loop.md) — Claude Code query, REPL, and tool orchestration flow
- [Codex — Agent Loop](references/code-analysis/loops/codex-agent-loop.md) — Codex Rust core loop and task execution flow
- [OpenClaw — Agent Loop](references/code-analysis/loops/openclaw-agent-loop.md) — OpenClaw agent-runner execution and self-modifying loop

## Entities — Products (4 pages)

- [Claude Code](references/entities/claude-code.md) — Anthropic's agentic coding tool
- [Codex](references/entities/codex.md) — OpenAI's open-source coding agent
- [OpenClaw](references/entities/openclaw.md) — Peter Steinberger's self-modifying agent
- [Hermes Agent](references/entities/hermes-agent.md) — Case-study agent operating layer mapped into the 9 dimensions

## Entities — People (8 pages)

- [Boris Cherny](references/entities/boris-cherny.md) — Creator of Claude Code
- [Cat Wu](references/entities/cat-wu.md) — Head of Product for Claude Code and Co-work
- [Luo Fuli](references/entities/luo-fuli.md) — Xiaomi large-model lead and OpenClaw agent-framework commentator
- [Greg Brockman](references/entities/greg-brockman.md) — OpenAI co-founder
- [Tibo Such](references/entities/tibo-such.md) — Codex engineering lead
- [Michael Bolin](references/entities/michael-bolin.md) — Codex open-source tech lead
- [Alex (Codex PM)](references/entities/alex-(codex-pm).md) — Codex PM lead
- [Peter Steinberger](references/entities/peter-steinberger.md) — Creator of OpenClaw

## Entities — Organizations (2 pages)

- [Anthropic](references/entities/anthropic.md) — Maker of Claude Code
- [OpenAI](references/entities/openai.md) — Maker of Codex

## Sources — Interviews (15 pages)

| Source | Product | Speaker | Date |
|---|---|---|---|
| [interview--claude-code--boris-cherny-2025-12-15](sources/interview-claude-code-boris-cherny-2025-12-15.md) | Claude Code | Boris Cherny | 2025-12-15 |
| [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md) | Claude Code | Boris Cherny | 2026-02-17 |
| [interview--claude-code--boris-cherny-2026-02-19](sources/interview-claude-code-boris-cherny-2026-02-19.md) | Claude Code | Boris Cherny | 2026-02-19 |
| [interview--claude-code--boris-cherny-2026-02-24](sources/interview-claude-code-boris-cherny-2026-02-24.md) | Claude Code | Boris Cherny | 2026-02-24 |
| [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md) | Claude Code | Boris Cherny | 2026-03-04 |
| [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md) | Claude Code | Cat Wu | 2026-04-23 |
| [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md) | Codex | Greg Brockman, Tibo Such | 2025-09-16 |
| [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md) | Codex | Michael Bolin | 2026-03-09 |
| [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md) | Codex | Alex, Roman | 2026-04-05 |
| [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md) | OpenClaw | Peter Steinberger | 2026-01-29 |
| [interview--openclaw--peter-steinberger-2026-02-01](sources/interview-openclaw-peter-steinberger-2026-02-01.md) | OpenClaw | Peter Steinberger | 2026-02-01 |
| [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md) | OpenClaw | Peter Steinberger | 2026-02-12 |
| [interview--openclaw--peter-steinberger-2026-02-25](sources/interview-openclaw-peter-steinberger-2026-02-25.md) | OpenClaw | Peter Steinberger | 2026-02-25 |
| [interview--openclaw--peter-steinberger-2026-02-25-b](sources/interview-openclaw-peter-steinberger-2026-02-25-b.md) | OpenClaw | Peter Steinberger | 2026-02-25 |
| [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md) | OpenClaw | Luo Fuli | 2026-04-23 |

## Sources — Claude Code Blogs (55 pages)

| Source | Title |
|---|---|
| [blog--claude-code--1m-context-ga](sources/blog-claude-code-1m-context-ga.md) | 1M Context Window GA |
| [blog--claude-code--auto-mode](sources/blog-claude-code-auto-mode.md) | Auto Mode |
| [blog--claude-code--automate-security-reviews-with-claude-code](sources/blog-claude-code-automate-security-reviews-with-claude-code.md) | Automate Security Reviews |
| [blog--claude-code--beyond-permission-prompts-making-claude-code-more-secure-and-autonomous](sources/blog-claude-code-beyond-permission-prompts-making-claude-code-more-secure-and-autonomous.md) | Beyond Permission Prompts |
| [blog--claude-code--build-responsive-web-layouts](sources/blog-claude-code-build-responsive-web-layouts.md) | Build Responsive Web Layouts |
| [blog--claude-code--building-agents-with-skills-equipping-agents-for-specialized-work](sources/blog-claude-code-building-agents-with-skills-equipping-agents-for-specialized-work.md) | Building Agents with Skills |
| [blog--claude-code--building-agents-with-the-claude-agent-sdk](sources/blog-claude-code-building-agents-with-the-claude-agent-sdk.md) | Agent SDK |
| [blog--claude-code--building-ai-agents-in-healthcare-and-life-sciences](sources/blog-claude-code-building-ai-agents-in-healthcare-and-life-sciences.md) | AI Agents in Healthcare |
| [blog--claude-code--building-companies-with-claude-code](sources/blog-claude-code-building-companies-with-claude-code.md) | Building Companies |
| [blog--claude-code--building-multi-agent-systems-when-and-how-to-use-them](sources/blog-claude-code-building-multi-agent-systems-when-and-how-to-use-them.md) | Multi-Agent Systems |
| [blog--claude-code--claude-code-and-new-admin-controls-for-business-plans](sources/blog-claude-code-claude-code-and-new-admin-controls-for-business-plans.md) | Admin Controls |
| [blog--claude-code--claude-code-and-slack](sources/blog-claude-code-claude-code-and-slack.md) | Slack Integration |
| [blog--claude-code--claude-code-on-the-web](sources/blog-claude-code-claude-code-on-the-web.md) | Claude Code on the Web |
| [blog--claude-code--claude-code-plugins](sources/blog-claude-code-claude-code-plugins.md) | Plugins |
| [blog--claude-code--claude-code-remote-mcp](sources/blog-claude-code-claude-code-remote-mcp.md) | Remote MCP |
| [blog--claude-code--claude-for-chrome](sources/blog-claude-code-claude-for-chrome.md) | Chrome Extension |
| [blog--claude-code--claude-managed-agents](sources/blog-claude-code-claude-managed-agents.md) | Managed Agents |
| [blog--claude-code--claude-team-updates](sources/blog-claude-code-claude-team-updates.md) | Team Updates |
| [blog--claude-code--code-review](sources/blog-claude-code-code-review.md) | Code Review |
| [blog--claude-code--code-with-claude-san-francisco-london-tokyo](sources/blog-claude-code-code-with-claude-san-francisco-london-tokyo.md) | Events |
| [blog--claude-code--complete-guide-to-building-skills-for-claude](sources/blog-claude-code-complete-guide-to-building-skills-for-claude.md) | Skills Guide |
| [blog--claude-code--contribution-metrics](sources/blog-claude-code-contribution-metrics.md) | Contribution Metrics |
| [blog--claude-code--cowork-for-enterprise](sources/blog-claude-code-cowork-for-enterprise.md) | Cowork Enterprise |
| [blog--claude-code--cowork-plugins](sources/blog-claude-code-cowork-plugins.md) | Cowork Plugins |
| [blog--claude-code--dispatch-and-computer-use](sources/blog-claude-code-dispatch-and-computer-use.md) | Dispatch & Computer Use |
| [blog--claude-code--driving-ai-transformation-with-claude](sources/blog-claude-code-driving-ai-transformation-with-claude.md) | AI Transformation |
| [blog--claude-code--eight-trends-defining-how-software-gets-built-in-2026](sources/blog-claude-code-eight-trends-defining-how-software-gets-built-in-2026.md) | Eight Trends 2026 |
| [blog--claude-code--equipping-agents-for-the-real-world-with-agent-skills](sources/blog-claude-code-equipping-agents-for-the-real-world-with-agent-skills.md) | Agent Skills |
| [blog--claude-code--fix-software-bugs-faster-with-claude](sources/blog-claude-code-fix-software-bugs-faster-with-claude.md) | Fix Bugs Faster |
| [blog--claude-code--harnessing-claudes-intelligence](sources/blog-claude-code-harnessing-claudes-intelligence.md) | Harnessing Intelligence |
| [blog--claude-code--how-ai-helps-break-cost-barrier-cobol-modernization](sources/blog-claude-code-how-ai-helps-break-cost-barrier-cobol-modernization.md) | COBOL Modernization |
| [blog--claude-code--how-anthropic-teams-use-claude-code](sources/blog-claude-code-how-anthropic-teams-use-claude-code.md) | Internal Usage |
| [blog--claude-code--how-anthropic-uses-claude-legal](sources/blog-claude-code-how-anthropic-uses-claude-legal.md) | Legal Team |
| [blog--claude-code--how-anthropic-uses-claude-marketing](sources/blog-claude-code-how-anthropic-uses-claude-marketing.md) | Marketing Team |
| [blog--claude-code--how-brex-improves-code-quality-and-productivity-with-claude-code](sources/blog-claude-code-how-brex-improves-code-quality-and-productivity-with-claude-code.md) | Brex Case Study |
| [blog--claude-code--how-enterprises-are-building-ai-agents-in-2026](sources/blog-claude-code-how-enterprises-are-building-ai-agents-in-2026.md) | Enterprise Agents |
| [blog--claude-code--how-to-configure-hooks](sources/blog-claude-code-how-to-configure-hooks.md) | Hooks Configuration |
| [blog--claude-code--how-to-create-skills-key-steps-limitations-and-examples](sources/blog-claude-code-how-to-create-skills-key-steps-limitations-and-examples.md) | Creating Skills |
| [blog--claude-code--improving-frontend-design-through-skills](sources/blog-claude-code-improving-frontend-design-through-skills.md) | Frontend Skills |
| [blog--claude-code--improving-skill-creator-test-measure-and-refine-agent-skills](sources/blog-claude-code-improving-skill-creator-test-measure-and-refine-agent-skills.md) | Skill Testing |
| [blog--claude-code--integrate-apis-seamlessly](sources/blog-claude-code-integrate-apis-seamlessly.md) | API Integration |
| [blog--claude-code--introduction-to-agentic-coding](sources/blog-claude-code-introduction-to-agentic-coding.md) | Intro to Agentic Coding |
| [blog--claude-code--key-benefits-transitioning-agentic-coding](sources/blog-claude-code-key-benefits-transitioning-agentic-coding.md) | Benefits of Agentic Coding |
| [blog--claude-code--making-claude-a-better-electrical-engineer](sources/blog-claude-code-making-claude-a-better-electrical-engineer.md) | Electrical Engineering |
| [blog--claude-code--optimize-code-performance-quickly](sources/blog-claude-code-optimize-code-performance-quickly.md) | Performance Optimization |
| [blog--claude-code--organization-skills-and-directory](sources/blog-claude-code-organization-skills-and-directory.md) | Org Skills Directory |
| [blog--claude-code--preview-review-and-merge-with-claude-code](sources/blog-claude-code-preview-review-and-merge-with-claude-code.md) | Preview Review Merge |
| [blog--claude-code--product-management-on-the-ai-exponential](sources/blog-claude-code-product-management-on-the-ai-exponential.md) | PM on AI Exponential |
| [blog--claude-code--scaling-agentic-coding](sources/blog-claude-code-scaling-agentic-coding.md) | Scaling Agentic Coding |
| [blog--claude-code--self-serve-enterprise](sources/blog-claude-code-self-serve-enterprise.md) | Self-Serve Enterprise |
| [blog--claude-code--skills](sources/blog-claude-code-skills.md) | Skills Overview |
| [blog--claude-code--subagents-in-claude-code](sources/blog-claude-code-subagents-in-claude-code.md) | Sub-agents |
| [blog--claude-code--using-claude-md-files](sources/blog-claude-code-using-claude-md-files.md) | CLAUDE.md Files |
| [blog--claude-code--web-search-api](sources/blog-claude-code-web-search-api.md) | Web Search API |
| [blog--claude-code--what-is-model-context-protocol](sources/blog-claude-code-what-is-model-context-protocol.md) | What is MCP |

## Sources — Codex Blogs (31 pages)

| Source | Title |
|---|---|
| [blog--codex--openai-developers-blog__2025-09-11__hello-world](sources/blog-codex-openai-developers-blog-2025-09-11-hello-world.md) | Hello World |
| [blog--codex--openai-developers-blog__2025-10-10__how-codex-ran-openai-devday-2025](sources/blog-codex-openai-developers-blog-2025-10-10-how-codex-ran-openai-devday-2025.md) | Codex at DevDay |
| [blog--codex--openai-developers-blog__2025-10-27__using-codex-for-education-at-dagster-labs](sources/blog-codex-openai-developers-blog-2025-10-27-using-codex-for-education-at-dagster-labs.md) | Codex at Dagster |
| [blog--codex--openai-developers-blog__2025-12-30__openai-for-developers-in-2025](sources/blog-codex-openai-developers-blog-2025-12-30-openai-for-developers-in-2025.md) | Developers in 2025 |
| [blog--codex--openai-developers-blog__2026-01-11__supercharging-codex-with-jetbrains-mcp-at-skyscanner](sources/blog-codex-openai-developers-blog-2026-01-11-supercharging-codex-with-jetbrains-mcp-at-skyscanner.md) | JetBrains MCP |
| [blog--codex--openai-developers-blog__2026-01-22__testing-agent-skills-systematically-with-evals](sources/blog-codex-openai-developers-blog-2026-01-22-testing-agent-skills-systematically-with-evals.md) | Skill Evals |
| [blog--codex--openai-developers-blog__2026-02-04__15-lessons-learned-building-chatgpt-apps](sources/blog-codex-openai-developers-blog-2026-02-04-15-lessons-learned-building-chatgpt-apps.md) | 15 Lessons |
| [blog--codex--openai-developers-blog__2026-02-11__shell-skills-compaction-tips-for-long-running-agents-that-do-real-work](sources/blog-codex-openai-developers-blog-2026-02-11-shell-skills-compaction-tips-for-long-running-agents-that-do-real-work.md) | Shell Compaction |
| [blog--codex--openai-developers-blog__2026-02-23__run-long-horizon-tasks-with-codex](sources/blog-codex-openai-developers-blog-2026-02-23-run-long-horizon-tasks-with-codex.md) | Long-Horizon Tasks |
| [blog--codex--openai-developers-blog__2026-02-26__building-frontend-uis-with-codex-and-figma](sources/blog-codex-openai-developers-blog-2026-02-26-building-frontend-uis-with-codex-and-figma.md) | Figma Integration |
| [blog--codex--openai-developers-blog__2026-03-09__using-skills-to-accelerate-oss-maintenance](sources/blog-codex-openai-developers-blog-2026-03-09-using-skills-to-accelerate-oss-maintenance.md) | OSS Skills |
| [blog--codex--openai-developers-blog__2026-03-11__from-prompts-to-products-one-year-of-responses](sources/blog-codex-openai-developers-blog-2026-03-11-from-prompts-to-products-one-year-of-responses.md) | Prompts to Products |
| [blog--codex--openai-developers-blog__2026-03-20__designing-delightful-frontends-with-gpt-5-4](sources/blog-codex-openai-developers-blog-2026-03-20-designing-delightful-frontends-with-gpt-5-4.md) | Delightful Frontends |
| [blog--codex--openai-news__2025-05-16__introducing-codex](sources/blog-codex-openai-news-2025-05-16-introducing-codex.md) | Introducing Codex |
| [blog--codex--openai-news__2025-09-15__addendum-to-gpt-5-system-card-gpt-5-codex](sources/blog-codex-openai-news-2025-09-15-addendum-to-gpt-5-system-card-gpt-5-codex.md) | GPT-5 System Card |
| [blog--codex--openai-news__2025-09-15__introducing-upgrades-to-codex](sources/blog-codex-openai-news-2025-09-15-introducing-upgrades-to-codex.md) | Codex Upgrades |
| [blog--codex--openai-news__2025-10-06__codex-is-now-generally-available](sources/blog-codex-openai-news-2025-10-06-codex-is-now-generally-available.md) | Codex GA |
| [blog--codex--openai-news__2025-11-19__building-more-with-gpt-5-1-codex-max](sources/blog-codex-openai-news-2025-11-19-building-more-with-gpt-5-1-codex-max.md) | GPT-5.1 Codex Max |
| [blog--codex--openai-news__2025-11-19__gpt-5-1-codex-max-system-card](sources/blog-codex-openai-news-2025-11-19-gpt-5-1-codex-max-system-card.md) | GPT-5.1 System Card |
| [blog--codex--openai-news__2025-12-12__how-we-used-codex-to-build-sora-for-android-in-28-days](sources/blog-codex-openai-news-2025-12-12-how-we-used-codex-to-build-sora-for-android-in-28-days.md) | Sora in 28 Days |
| [blog--codex--openai-news__2025-12-18__addendum-to-gpt-5-2-system-card-gpt-5-2-codex](sources/blog-codex-openai-news-2025-12-18-addendum-to-gpt-5-2-system-card-gpt-5-2-codex.md) | GPT-5.2 System Card |
| [blog--codex--openai-news__2025-12-18__introducing-gpt-5-2-codex](sources/blog-codex-openai-news-2025-12-18-introducing-gpt-5-2-codex.md) | GPT-5.2 Codex |
| [blog--codex--openai-news__2026-01-23__unrolling-the-codex-agent-loop](sources/blog-codex-openai-news-2026-01-23-unrolling-the-codex-agent-loop.md) | Agent Loop Deep Dive |
| [blog--codex--openai-news__2026-02-02__introducing-the-codex-app](sources/blog-codex-openai-news-2026-02-02-introducing-the-codex-app.md) | Codex App Launch |
| [blog--codex--openai-news__2026-02-04__unlocking-the-codex-harness-how-we-built-the-app-server](sources/blog-codex-openai-news-2026-02-04-unlocking-the-codex-harness-how-we-built-the-app-server.md) | App-Server Architecture |
| [blog--codex--openai-news__2026-02-05__gpt-5-3-codex-system-card](sources/blog-codex-openai-news-2026-02-05-gpt-5-3-codex-system-card.md) | GPT-5.3 System Card |
| [blog--codex--openai-news__2026-02-05__introducing-gpt-5-3-codex](sources/blog-codex-openai-news-2026-02-05-introducing-gpt-5-3-codex.md) | GPT-5.3 Codex |
| [blog--codex--openai-news__2026-02-12__introducing-gpt-5-3-codex-spark](sources/blog-codex-openai-news-2026-02-12-introducing-gpt-5-3-codex-spark.md) | Codex Spark |
| [blog--codex--openai-news__2026-03-06__codex-security-now-in-research-preview](sources/blog-codex-openai-news-2026-03-06-codex-security-now-in-research-preview.md) | Security Preview |
| [blog--codex--openai-news__2026-03-16__why-codex-security-doesn-t-include-a-sast-report](sources/blog-codex-openai-news-2026-03-16-why-codex-security-doesn-t-include-a-sast-report.md) | Why No SAST |
| [blog--codex--openai-news__2026-04-02__codex-now-offers-pay-as-you-go-pricing-for-teams](sources/blog-codex-openai-news-2026-04-02-codex-now-offers-pay-as-you-go-pricing-for-teams.md) | Pay-As-You-Go |

## Sources — Hermes Agent (2 pages)

| Source | Type | Date |
|---|---|---|
| [doc--hermes-agent--runtime-capabilities-2026-04-25](sources/doc-hermes-agent-runtime-capabilities-2026-04-25.md) | Runtime/tool context source note | 2026-04-25 |
| [article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24](sources/article-hermes-agent-self-evolving-prompt-context-harness-2026-04-24.md) | External article/source analysis on self-evolution, prompt, context, and harness | 2026-04-24 |

## Sources — External Analysis Articles (2 pages)

| Source | Product | Date |
|---|---|---|
| [article--claude-code--prompt-context-harness-2026-04-20](sources/article-claude-code-prompt-context-harness-2026-04-20.md) | Claude Code | 2026-04-20 |
| [article--openclaw--prompt-context-harness-2026-04-13](sources/article-openclaw-prompt-context-harness-2026-04-13.md) | OpenClaw | 2026-04-13 |

---

*156 total pages across navigation (4), design (4), Hermes case study (5), dimensions (9), comparisons (2), guides (2), code analysis (11), entities (14), sources (105).*

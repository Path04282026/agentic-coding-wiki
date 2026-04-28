---
tags: [dimension]
dimension: 07
title: Feature Gating and Internals
products: [Claude Code, Codex, OpenClaw]
source_count: 10
---

# Feature Gating and Internals

## Overview

Feature gating — how products control the rollout of new capabilities — reveals the gap between what's been built and what's been shipped. Claude Code hides the most from external users, with entire systems (KAIROS, Coordinator Mode, ULTRAPLAN) available only internally. Codex is the most open, with its Apache-2.0 codebase. OpenClaw takes a community-first approach where everything is public.

## Claude Code

**Dual build system:**
- **Compile-time gates** (`bun:bundle feature()`): Internal features stripped entirely from external builds via dead-code elimination
- **Runtime gates** (GrowthBook): Feature flags prefixed with `tengu_`, aggressively cached, enabling gradual rollout and A/B testing
- Two separate binaries: internal (`ant`) vs external

**Internal-only features revealed in source leak (March 31, 2026):**
- **KAIROS** — Always-on proactive assistant
- **Coordinator Mode** — Multi-agent orchestration with parallel phases
- **Bridge Mode** — Remote control via claude.ai
- **ULTRAPLAN** — 30-minute remote planning with Opus 4.6
- **Voice Mode** — Speech-based interaction
- **Buddy** — Tamagotchi companion
- **Transcript Classifier** — AFK auto-approval
- **Undercover Mode** — Prevent leaking internal information

**Feature velocity:** Tools are shipped and unshipped weekly. Boris: "We try so many different tools and just statistically most of them we throw away."

- Source citations: [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md), [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [blog--claude-code--claude-team-updates](sources/blog-claude-code-claude-team-updates.md), [blog--claude-code--self-serve-enterprise](sources/blog-claude-code-self-serve-enterprise.md), [blog--claude-code--claude-code-and-new-admin-controls-for-business-plans](sources/blog-claude-code-claude-code-and-new-admin-controls-for-business-plans.md)

## Codex

**Open-source approach:**
- `codex-features` crate for rollout control
- Per-stage deployment: internal → beta → stable
- Unstable features get warning notifications
- Most features are open source — less hidden capability than Claude Code
- Some features gated behind ChatGPT plan tiers

**Community as de facto beta testers:**
> "Whenever we're building a feature, we start getting complaints on Twitter that the feature — which by the way is not enabled in prod — is like broken because people are going in and changing the code." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

- Source citations: [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md), [blog--codex--openai-news__2026-04-02__codex-now-offers-pay-as-you-go-pricing-for-teams](sources/blog-codex-openai-news-2026-04-02-codex-now-offers-pay-as-you-go-pricing-for-teams.md)

## OpenClaw

**Everything is public:**
- Open source repository (MIT license, 175,000+ GitHub stars)
- Community-driven development
- Peter builds features publicly, often using the agent itself to build the agent
- "Prompt requests" — PRs from users who used agents to generate code

- Source citations: [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--peter-steinberger-2026-02-25](sources/interview-openclaw-peter-steinberger-2026-02-25.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Open source** | No (leaked March 2026) | Yes (Apache-2.0) | Yes (MIT) |
| **Internal features** | Extensive (KAIROS, Coordinator, ULTRAPLAN, Bridge, Buddy) | Minimal (most features public) | None (everything public) |
| **Feature flags** | Compile-time + runtime (GrowthBook) | Crate features + staged rollout | Configuration-driven |
| **Feature velocity** | Weekly add/remove of tools | Staged rollout | Continuous deployment |
| **Community contribution** | Limited (proprietary) | Active (open source + forks) | Active (prompt requests) |
| **Trust model** | Internal builds far ahead of external | Consistent across builds | Single public build |

## Key Quotes

> "Claude Code hides far more from external users. The leaked source revealed systems that are months or years ahead of the public product." — Analysis

> "We try so many different tools and just statistically most of them we throw away." — [Boris Cherny](references/entities/boris-cherny.md)

> "People are going in and changing the code themselves or forking it to get these new features working." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [blog--claude-code--claude-team-updates](sources/blog-claude-code-claude-team-updates.md)
- [blog--claude-code--self-serve-enterprise](sources/blog-claude-code-self-serve-enterprise.md)
- [blog--codex--openai-news__2026-04-02__codex-now-offers-pay-as-you-go-pricing-for-teams](sources/blog-codex-openai-news-2026-04-02-codex-now-offers-pay-as-you-go-pricing-for-teams.md)

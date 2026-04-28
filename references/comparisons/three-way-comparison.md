---
tags: [comparison]
title: Three-Way Comparison
products: [Claude Code, Codex, OpenClaw]
dimensions: [01, 02, 03, 04, 05, 06, 07, 08, 09]
---

# Three-Way Comparison: Claude Code vs Codex vs OpenClaw

## Master Comparison Table

| Dimension | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Core language** | TypeScript (Bun) | Rust (Tokio async) | TypeScript (Node.js) |
| **Agent loop** | Streaming event-driven | Async Rust + SSE | Self-modifying gateway |
| **Key metaphor** | "Don't box the model" | "Harness = body, model = brain" | "Closing the loop" |
| **Tools** | 40+ specialized | ~10 core + MCP | System-level access |
| **Code editing** | Find-and-replace | Unified diff | Via underlying agent |
| **Extension mechanism** | Plugins + Skills + MCP | MCP + open-source forking | Self-modification + plugins |
| **Safety architecture** | Swiss cheese (8 soft layers) | Kernel-enforced (4 hard layers) | Docker + prompt-level |
| **Permission model** | default / auto / bypass | suggest / auto-edit / full-auto | Configuration-driven |
| **Project config** | CLAUDE.md (hierarchical) | AGENTS.md (flat) | Soul.md + custom docs |
| **Context compaction** | Multi-strategy (snip, micro, reactive) | CompactTask | Model-dependent |
| **Memory system** | Dream (4-phase consolidation) | SQLite state DB | Markdown + vector DB |
| **Sub-agents** | Typed (Explore, Plan, general) | Ghost snapshots, emerging | Manual multi-terminal |
| **Multi-agent orchestration** | Coordinator Mode (internal) | Cloud agent fleets (vision) | Chess grandmaster pattern |
| **Primary UI** | Terminal CLI | Terminal CLI → App | Messaging apps |
| **Distribution surfaces** | CLI, IDE, mobile, web, Slack, desktop | CLI, IDE, App, web | WhatsApp, Telegram, Discord |
| **Open source** | No (leaked March 2026) | Yes (Apache-2.0) | Yes (MIT, 175K+ stars) |
| **Feature gating** | Compile-time + runtime (GrowthBook) | Crate features + staged rollout | Everything public |
| **Internal-only features** | KAIROS, Coordinator, ULTRAPLAN, Bridge | Minimal | None |
| **Model coupling** | Model-agnostic (Anthropic models) | Co-optimized (GPT-5 Codex specific) | Model-agnostic (multi-provider) |
| **Philosophy** | "Trust the model" | "Trust but verify" | "Agentic engineering" |
| **Target user** | Enterprise teams + individual devs | Enterprise teams + open community | Solo builders + small teams |
| **Key person** | [Boris Cherny](references/entities/boris-cherny.md) | [Greg Brockman](references/entities/greg-brockman.md), [Michael Bolin](references/entities/michael-bolin.md) | [Peter Steinberger](references/entities/peter-steinberger.md) |

## Architecture Decision Records

### Why TypeScript vs Rust vs TypeScript?

| | Claude Code (TypeScript) | Codex (Rust) | OpenClaw (TypeScript) |
|---|---|---|---|
| **Rationale** | Fast iteration, same language as model outputs | Performance, safety guarantees, kernel integration | Rapid prototyping, ecosystem access |
| **Startup time** | ~200ms (Bun bundled) | ~50ms (native binary) | Varies (Node.js) |
| **Packaging** | npm (`@anthropic-ai/claude-code`) | npm + Homebrew + GitHub releases | npm (pnpm) |
| **Tradeoff** | Slower execution, faster development | Faster execution, steeper learning curve | Balanced, community-friendly |

### Why Find-Replace vs Unified Diff?

| Approach | Claude Code (Find-Replace) | Codex (Unified Diff) |
|---|---|---|
| **Reliability** | Higher — exact match required, fails on ambiguity | Lower — diffs can misalign with slight context changes |
| **Expressiveness** | Lower — one replacement per call | Higher — additions, deletions, moves in one operation |
| **Large changes** | Multiple calls needed | Single diff covers large refactors |
| **Model success rate** | Higher per-operation | Lower per-operation but more efficient for big changes |

### Why Swiss Cheese vs Kernel Sandbox?

| Approach | Claude Code (Swiss Cheese) | Codex (Kernel) |
|---|---|---|
| **Can model bypass?** | Theoretically yes (prompt injection) | No (kernel-enforced) |
| **Iterability** | High — can add/remove layers easily | Low — kernel code harder to modify |
| **UX impact** | Lower — permission prompts are interactive | Higher — hard limits can block valid workflows |
| **Auditability** | Lower (proprietary) | Higher (open-source Rust) |

## What's Converging

Despite different approaches, all three are heading toward:

1. **Multi-agent cloud fleets** — Many agents working independently in the cloud
2. **Personal agent** — AI that knows you, remembers you, works while you sleep
3. **Non-engineer access** — Software building democratized beyond programmers
4. **Self-improving agents** — Agents that learn from use and get better over time
5. **MCP as the extension standard** — Common protocol for tool integration

## Sources

All 9 dimension pages + all entity pages. See [index](references/hermes/index.md) for the full catalog.

---
tags: [entity, product]
product: Codex
dimensions: ["01 - Agent Loop", "02 - Tool Systems", "03 - Security and Sandboxing", "04 - Context Management", "05 - Multi-Agent Architecture", "06 - UI and Distribution", "07 - Feature Gating and Internals", "08 - Shared Principles", "09 - Philosophical Divergence"]
---

# Codex

**Developer:** [OpenAI](references/entities/openai.md)
**Key people:** [Greg Brockman](references/entities/greg-brockman.md), [Tibo Such](references/entities/tibo-such.md), [Michael Bolin](references/entities/michael-bolin.md), [Alex (Codex PM)](references/entities/alex-(codex-pm).md)
**First version:** Early 2025 (internal "10x" prototype)
**Language:** Rust (93 crates in a Cargo workspace)
**Open source:** Yes (Apache-2.0)

## Origins

The earliest "sign of life" was feeding docstrings to GPT-3 and watching it auto-complete code. GitHub Copilot (2022) was the first real product, constrained by a 1,500ms latency requirement. Internally, OpenAI built a terminal prototype called "10x" — "because we felt like it was giving us this 10x productivity boost." They decided not to ship it, favoring an async cloud-first approach. After the cloud product proved premature, they pivoted back to CLI.

> "We actually had a prototype of it fully working in a terminal… We decided to not launch this as a product. It didn't feel polished enough. It was called 10x." — [Greg Brockman](references/entities/greg-brockman.md)

## Architecture

- **Agent loop**: Async Rust with Tokio, SSE streaming from Responses API
- **Tools**: ~10 core tools + MCP extensibility
- **Code editing**: Unified diff (`apply_patch`)
- **Security**: Kernel-enforced sandboxing (Seatbelt/Landlock/bubblewrap per platform)
- **Memory**: SQLite-backed state database
- **Context**: AGENTS.md + CompactTask for summarization
- **Sub-agents**: Ghost snapshots, emerging sub-agent support
- **UI**: ratatui (Rust native TUI)
- **Build**: Bazel + Cargo, compiled native binaries per platform
- **API**: app-server with JSON-RPC 2.0 over WebSocket

## Key Design Principles

1. **"The harness is your body, the model is your brain"** — Co-optimized model + harness
2. **Trust but verify** — Kernel-enforced safety, open source for auditability
3. **Intelligence × Convenience matrix** — Two axes of product value
4. **Build for yourself first** — Internal tools (10x, code review) shipped only above utility threshold

## Form Factor Evolution

1. 10x (internal CLI) → Not shipped
2. Codex Cloud (async remote) → "Great idea, a little early"
3. Codex CLI + IDE extension → "Growth started exploding, 20–30x in months"
4. Codex App → Progressive disclosure design inspired by power users' 18-terminal setups

## Distribution

CLI → VS Code → Cursor → Windsurf → Codex App → Web (chatgpt.com/codex)

## Notable Quotes About Codex

> "It was quite contentious — should we even be building an app?" — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

> "GPT-5 Codex's differentiator is sustained effort — grit on complex refactoring tasks, working up to seven hours." — [Greg Brockman](references/entities/greg-brockman.md)

## Linked Dimensions

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — Async Rust loop, SSE streaming
- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — Shell-centric, MCP extensibility
- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Kernel-enforced, open source
- [04 - Context Management](references/dimensions/04-context-management.md) — AGENTS.md, CompactTask
- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — Cloud agent fleets, ghost snapshots
- [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md) — 10x → Cloud → CLI → App
- [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) — Open source, staged rollout
- [08 - Shared Principles](references/dimensions/08-shared-principles.md) — Minimal scaffolding, CLI origins
- [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md) — "Trust but verify"

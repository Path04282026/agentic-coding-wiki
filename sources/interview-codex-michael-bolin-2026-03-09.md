---
tags: [source, interview]
product: Codex
date: 2026-03-09
dimensions: [01, 02, 03, 06]
speaker: Michael Bolin

---

# Interview — Codex — Michael Bolin (2026-03-09)

**Format:** Podcast interview
**Speaker:** [Michael Bolin](references/entities/michael-bolin.md) — Tech Lead, open-source [Codex](references/entities/codex.md)
**Existing summary:** `raw/summaries/CodeX_20260309_Summary.md`
**Tier:** **Tier 1** — Tech lead discussing architecture and open-source strategy

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Async Rust with Tokio runtime
- SSE streaming from Responses API
- CancellationToken for graceful interruption
- Central tool router enforces sandbox policies

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- Shell-centric: core operations through shell commands
- apply_patch for code editing (unified diff)
- Full MCP client/server implementation (`codex-mcp` crate)

### [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md)
- Platform-specific sandboxing: Seatbelt (macOS), Landlock + bubblewrap (Linux)
- Full-auto mode: network completely disabled at namespace level
- Sandbox code manually written in Rust for auditability
- .git and .codex directories remain read-only

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- ratatui-based terminal UI
- Bazel + Cargo build system
- 93 crates in a Cargo workspace
- app-server with JSON-RPC 2.0 over WebSocket

## Background: Buck & Nuclide

Michael's career at Meta provides crucial context:
- **Buck build system**: Built faster Android builds at Facebook, adopted by Uber, Airbnb
- **Nuclide IDE**: React-based Xcode replacement, first desktop app with React
- **Chickenfoot**: Thesis project — Firefox extension for end-user web programming (precursor to modern agent tools)
- Pattern: build something dramatically better → adoption follows

## Notable Quotes

> "Strict sandboxing written manually in Rust to guarantee the model stays within system bounds."

> "The questions that you asked the agent is going to affect the quality of the thing that you get out."

> "I almost feel a little bad writing code by hand."

> "The answer is almost always no [to writing code by hand]."

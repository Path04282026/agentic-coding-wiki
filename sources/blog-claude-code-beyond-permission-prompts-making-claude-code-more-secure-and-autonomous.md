---
tags: [source, blog]
product: Claude Code
date: 2025-10-08
dimensions: [03]

---

# Blog — Claude Code — Beyond Permission Prompts

**Source:** `raw/blogs/claude-code/beyond-permission-prompts-making-claude-code-more-secure-and-autonomous.md`
**Product:** [Claude Code](references/entities/claude-code.md)
**Published:** October 8, 2025
**URL:** https://claude.com/blog/beyond-permission-prompts-making-claude-code-more-secure-and-autonomous

## Summary

Introduces Claude Code's sandboxing architecture — the shift from permission-based to boundary-based security. Two new features: sandboxed bash tool (local OS-level sandboxing) and Claude Code on the web (cloud sandbox).

## Key Technical Details

### Problem: Approval Fatigue
- Default model is read-only — asks permission before all modifications
- Static analysis auto-allows safe commands (`echo`, `cat`)
- Constant approval clicking leads to "approval fatigue" — users stop paying attention

### Sandboxing Architecture (Two Boundaries)
1. **Filesystem isolation** — Claude can only access/modify specific directories
2. **Network isolation** — Claude can only connect to approved servers

> "Effective sandboxing requires both filesystem and network isolation. Without network isolation, a compromised agent could exfiltrate sensitive files; without filesystem isolation, a compromised agent could escape the sandbox."

### Local Sandbox (--sandbox flag)
- Built on OS-level primitives:
  - **Linux**: bubblewrap
  - **macOS**: Seatbelt (sandbox-exec)
- Covers all subprocesses spawned by commands
- Filesystem: read/write to CWD only, block modifications outside
- Network: traffic routed through Unix domain socket proxy
  - Proxy enforces domain restrictions
  - User confirmation for new domains
  - Customizable proxy rules for outgoing traffic

### Claude Code on the Web (Cloud Sandbox)
- Each session runs in an **isolated cloud sandbox**
- Credential separation: git credentials and signing keys **never enter** the sandbox
- Custom proxy service handles git interactions:
  - Sandbox git client authenticates with scoped credential
  - Proxy verifies contents of git interaction (e.g., only pushing to configured branch)
  - Proxy attaches real auth token before forwarding to GitHub

## Implementation Note

Claude Code's local sandbox approach is **very similar to Codex's kernel-enforced sandbox** (Seatbelt/bubblewrap). This blog post represents Claude Code converging toward Codex's security approach — a key data point for [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md).

## Relevant Dimensions

- [03 - Security and Sandboxing](references/dimensions/03-security-and-sandboxing.md) — Primary reference for Claude Code's OS-level sandboxing

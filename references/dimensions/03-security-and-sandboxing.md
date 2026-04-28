---
tags: [dimension]
dimension: 03
title: Security and Sandboxing
products: [Claude Code, Codex, OpenClaw]
source_count: 14
---

# Security and Sandboxing

## Overview

Security is where the three products diverge most dramatically. An agentic coding tool that can execute arbitrary commands, edit files, and browse the web presents genuine safety risks — from accidental data deletion to prompt injection attacks. Each product addresses these risks through fundamentally different architectural choices that reflect their philosophical stance on model trust.

Claude Code stacks many "soft" application-layer defenses (the "Swiss cheese model"). Codex enforces hard boundaries at the OS kernel level. OpenClaw initially relied on prompt-based behavioral constraints and has evolved toward Docker-based sandboxing.

## Claude Code

Claude Code's security is primarily **application-layer** — implemented in TypeScript with multiple overlapping defenses.

**The Swiss cheese model** (Boris's explicit framing):

```
┌─────────────────────────────────────┐
│  Layer 1: Model alignment           │  (Opus 4.6 training)
├─────────────────────────────────────┤
│  Layer 2: Runtime classifiers       │  (block bad requests)
├─────────────────────────────────────┤
│  Layer 3: Sub-agent summarization   │  (sanitize web content)
├─────────────────────────────────────┤
│  Layer 4: Permission prompts        │  (ask user before risky ops)
├─────────────────────────────────────┤
│  Layer 5: Allow-lists               │  (pre-approve safe commands)
├─────────────────────────────────────┤
│  Layer 6: Protected file lists      │  (.gitconfig, .bashrc, etc.)
├─────────────────────────────────────┤
│  Layer 7: VM sandbox (Co-work)      │  (for non-technical users)
├─────────────────────────────────────┤
│  Layer 8: YOLO classifier           │  (ML-based auto-approval)
└─────────────────────────────────────┘
```

**Permission system origin story**: When Boris built the first agentic tool, safety teams pushed back — "you can't just let the model run bash commands, that's unsafe." Boris and Ben Mann (Anthropic co-founder) invented permission prompts as the solution: "If you're not sure, just ask the human and they can decide."

**Permission modes:**
- `default` — Interactive approval for risky operations
- `auto` — ML classifier decides what's safe
- `bypass` — No restrictions (internal/development use)

**Risk classification**: LOW / MEDIUM / HIGH per tool, with conservative defaults. Even `find` isn't pre-allowed because it has flags for arbitrary code execution.

**Prompt injection defense (3 layers):**
1. Model alignment — Opus 4.6 trained to resist injection
2. Runtime classifiers — Block suspicious requests
3. Sub-agent summarization — Web content summarized by a sub-agent before reaching the main agent

**Co-work additional guardrails**: For non-technical users, Co-work adds VM sandboxing, deletion protections, and OS-level integrations to prevent accidentally destroying user data (e.g., family photos).

> "For things like safety and security, there's no one perfect answer. It's always a Swiss cheese model. You just need a bunch of layers and with enough layers, the probability of catching anything goes up." — [Boris Cherny](references/entities/boris-cherny.md)

- Source citations: [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md), [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [blog--claude-code--beyond-permission-prompts-making-claude-code-more-secure-and-autonomous](sources/blog-claude-code-beyond-permission-prompts-making-claude-code-more-secure-and-autonomous.md), [blog--claude-code--automate-security-reviews-with-claude-code](sources/blog-claude-code-automate-security-reviews-with-claude-code.md), [blog--claude-code--auto-mode](sources/blog-claude-code-auto-mode.md)

## Codex

Codex's security is **OS-kernel-enforced** — implemented in Rust using platform-native sandboxing mechanisms.

**Platform-specific sandboxing:**

```
┌─────────────────────────────────────┐
│  Layer 1: Approval modes            │  (suggest / auto-edit / full-auto)
├─────────────────────────────────────┤
│  Layer 2: Platform sandboxing       │
│    macOS: Seatbelt (sandbox-exec)   │  Kernel-enforced read-only jail
│    Linux: Landlock + bubblewrap     │  Namespace isolation
│    Windows: Restricted token        │  Process-level restriction
├─────────────────────────────────────┤
│  Layer 3: Network proxy             │  (monitor/block outbound traffic)
├─────────────────────────────────────┤
│  Layer 4: Process hardening         │  (syscall filtering)
└─────────────────────────────────────┘
```

**Key properties:**
- In full-auto mode, the network is **completely disabled at the namespace level** — the process has no network stack
- Writable directories are explicitly whitelisted (`$PWD`, `$TMPDIR`, `~/.codex`)
- `.git` and `.codex` directories remain read-only even in writable areas
- The Rust sandbox code is **manually written** (not generated) for auditability

**Open source as trust layer**: The entire sandbox implementation is Apache-2.0 licensed. Users can read, audit, and verify the security code themselves. This is deliberately positioned as a competitive advantage over proprietary approaches.

> "Strict sandboxing written manually in Rust to guarantee the model stays within system bounds." — [Michael Bolin](references/entities/michael-bolin.md)

**Graduated approval modes:**
- `suggest` — Model proposes, human approves everything
- `auto-edit` — Model can edit files autonomously, but shell commands need approval
- `full-auto` — Full autonomy within the kernel-enforced sandbox

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md), [blog--codex--openai-news__2026-03-06__codex-security-now-in-research-preview](sources/blog-codex-openai-news-2026-03-06-codex-security-now-in-research-preview.md), [blog--codex--openai-news__2026-03-16__why-codex-security-doesn-t-include-a-sast-report](sources/blog-codex-openai-news-2026-03-16-why-codex-security-doesn-t-include-a-sast-report.md)

## OpenClaw

OpenClaw's security story has evolved from informal to structured, reflecting its journey from personal project to community tool.

**Early approach — prompt-based trust:**
- Initially, Peter "just prompted it to like only listen to me" when exposing the bot on Discord
- No sandboxing in the early prototype — "no security because I hadn't built sandboxing in yet"
- The agent itself would solve security problems through self-modification when vulnerabilities were discovered

**Current approach — Docker/containerized sandboxing:**
- OpenClaw runs in Docker containers with configurable isolation
- The Dockerfile and sandbox configuration are part of the open-source repository
- Arch Linux-based container environment provides process isolation

**Self-modifying security implications:**
The most philosophically interesting security question: if the agent can modify its own source code, can it modify its own security constraints? Peter's approach is pragmatic — the system is designed for power users who understand the implications, not for non-technical users who need guardrails.

> "I watch my agent happily click the 'I'm not a robot' button." — [Peter Steinberger](references/entities/peter-steinberger.md)

**Community trust model:**
- Open source codebase — full transparency
- Active Discord community for security reporting
- Rapid response to vulnerabilities (driven by Peter's direct involvement)

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--peter-steinberger-2026-02-25](sources/interview-openclaw-peter-steinberger-2026-02-25.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Primary enforcement** | Application layer (TypeScript) | OS kernel (Seatbelt/Landlock/bwrap) | Docker containers + prompt-level |
| **Can the model bypass it?** | Theoretically via prompt injection | No — kernel-enforced | Theoretically yes (self-modifying) |
| **Network control** | Per-request classifiers | Full namespace isolation | Docker network configuration |
| **File access control** | Protected file list + permissions | Filesystem jail (can't see outside) | Docker volume mounts |
| **Trust model** | Swiss cheese (many soft layers) | Hard boundary (few but kernel-enforced) | Community trust + transparency |
| **Open for audit** | No (proprietary) | Yes (Apache-2.0) | Yes (MIT) |
| **Prompt injection defense** | 3-layer (alignment + classifiers + sub-agent) | Kernel prevents meaningful exploitation | Prompt-based behavioral constraints |
| **Non-technical user safety** | Co-work VM + classifiers + deletion protection | Sandbox enforced regardless | Power-user oriented |
| **Tradeoff** | More flexible, easier to iterate | More secure, harder to modify | Most open, least guardrailed |

## Key Quotes

> "For things like safety and security, there's no one perfect answer. It's always a Swiss cheese model." — [Boris Cherny](references/entities/boris-cherny.md)

> "You can't just let the model run bash commands, that's unsafe… this is not a solvable problem." — Anthropic safety teams (paraphrased by [Boris Cherny](references/entities/boris-cherny.md))

> "Strict sandboxing written manually in Rust to guarantee the model stays within system bounds." — [Michael Bolin](references/entities/michael-bolin.md)

> "One of the things that is incredibly important to solve is the safety, security, alignment of all of this so that agents can perform useful work but in a safe way." — [Greg Brockman](references/entities/greg-brockman.md)

> "No security because I hadn't built sandboxing in yet. I just prompted it to only listen to me." — [Peter Steinberger](references/entities/peter-steinberger.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [blog--claude-code--beyond-permission-prompts-making-claude-code-more-secure-and-autonomous](sources/blog-claude-code-beyond-permission-prompts-making-claude-code-more-secure-and-autonomous.md)
- [blog--claude-code--automate-security-reviews-with-claude-code](sources/blog-claude-code-automate-security-reviews-with-claude-code.md)
- [blog--claude-code--auto-mode](sources/blog-claude-code-auto-mode.md)
- [blog--codex--openai-news__2026-03-06__codex-security-now-in-research-preview](sources/blog-codex-openai-news-2026-03-06-codex-security-now-in-research-preview.md)
- [blog--codex--openai-news__2026-03-16__why-codex-security-doesn-t-include-a-sast-report](sources/blog-codex-openai-news-2026-03-16-why-codex-security-doesn-t-include-a-sast-report.md)
- [blog--codex--openai-news__2025-09-15__addendum-to-gpt-5-system-card-gpt-5-codex](sources/blog-codex-openai-news-2025-09-15-addendum-to-gpt-5-system-card-gpt-5-codex.md)

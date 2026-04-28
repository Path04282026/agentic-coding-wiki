---
tags: [dimension]
dimension: 06
title: UI and Distribution
products: [Claude Code, Codex, OpenClaw]
source_count: 16
---

# UI and Distribution

## Overview

How these tools reach users — and what the user experience looks like — reveals their distribution strategy, design philosophy, and target audience. All three started in the terminal and all three expanded beyond it, but the paths diverge dramatically: Claude Code went multi-surface (CLI, desktop, mobile, web, Slack, IDE); Codex went from cloud → CLI → Codex App; OpenClaw started as a WhatsApp relay and expanded to a full messaging-first agent.

## Claude Code

**Terminal UI:** Built on a custom fork of Ink (React for terminals), with 50+ UI components, a design system, and extensive iteration (the spinner alone went through "50–100 iterations, ~80% never shipped").

**Multi-surface expansion (chronological):**
1. Terminal CLI (original — Boris "didn't want to build a UI")
2. VS Code extension
3. JetBrains extension
4. iOS / Android apps
5. Desktop app
6. Web (claude.ai/code)
7. Slack integration
8. GitHub integration
9. **Co-work** — VM-sandboxed GUI for non-technical users

**Co-work genesis:** Finance team, designers, data scientists — all installing a terminal tool despite it not being designed for them. "People will only do a thing that they already do. You can't get people to do a new thing." Co-work was built by Felix (early Electron contributor) and team in ~10 days, 100% written by Claude Code.

**Boris's daily workflow:** Uses 5 terminal tabs, the desktop app for overflow, and the iPhone app for ~1/3 to 1/2 of his coding. "If you told me six months ago I'd be writing a third/half of my code on a phone, that's crazy."

**Cat Wu's product-surface map:** Claude Code CLI remains the most powerful surface for one-off coding tasks and newest features; desktop is best for visual/front-end work and a task control plane; web/mobile are for starting work on the go; Co-work is for non-code outputs such as decks, launch plans, docs, Slack/Gmail workflows, and customer materials.

**Built-in education:** `/powerup` was added because the product surface became too large for users to discover unaided. This reversed the earlier "no onboarding flow" principle in favor of helping users learn the core feature set.

> "It's unbelievable that we're still using a terminal. That was supposed to be the starting point. I didn't think that would be the ending point." — [Boris Cherny](references/entities/boris-cherny.md)

- Source citations: [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md), [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md), [blog--claude-code--claude-code-on-the-web](sources/blog-claude-code-claude-code-on-the-web.md), [blog--claude-code--claude-code-and-slack](sources/blog-claude-code-claude-code-and-slack.md), [blog--claude-code--cowork-for-enterprise](sources/blog-claude-code-cowork-for-enterprise.md), [blog--claude-code--claude-for-chrome](sources/blog-claude-code-claude-for-chrome.md)

## Codex

**Three pivots in form factor:**
1. **10x** (internal CLI prototype) — "Because we felt like it was giving us this 10x productivity boost." Worked but was not shipped.
2. **Codex Cloud** (async remote agent) — "Great idea. People were super excited. But it was a little early."
3. **Codex CLI + IDE extension** — Shipped with GPT-5 in August — "growth started exploding… we grew like 20 or 30x in a few months"
4. **Codex App** — Built when users were already t-muxing 18+ terminals. Progressive disclosure design — "like playing a game, you're just discovering what's next."

**Terminal UI:** Built on ratatui (Rust native TUI), with direct terminal buffer manipulation. Chat widget, file search, diff viewer.

**Distribution surfaces:**
- CLI (`@openai/codex` npm + Homebrew + GitHub releases)
- VS Code, Cursor, Windsurf IDE extensions
- Codex App (desktop — macOS, Linux, Windows)
- Web (chatgpt.com/codex — cloud agent)

**API layer:** The app-server provides versioned JSON-RPC 2.0 over WebSocket — a clean, extensible protocol for third-party integrations. Shared Rust core used by CLI, IDE extension, and app.

> "We started seeing crazy things on social media… Peter Steinberger with like 18 terminals across three monitors." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md), [blog--codex--openai-news__2025-05-16__introducing-codex](sources/blog-codex-openai-news-2025-05-16-introducing-codex.md), [blog--codex--openai-news__2026-02-02__introducing-the-codex-app](sources/blog-codex-openai-news-2026-02-02-introducing-the-codex-app.md), [blog--codex--openai-news__2025-10-06__codex-is-now-generally-available](sources/blog-codex-openai-news-2025-10-06-codex-is-now-generally-available.md)

## OpenClaw

**Messaging-first architecture:** OpenClaw's UI is fundamentally different — the primary interface is a messaging app, not a terminal or IDE.

**Distribution surfaces:**
- WhatsApp (original — the 1-hour prototype)
- Telegram
- Discord (where the community saw the agent in action)
- Signal, iMessage
- Terminal (for direct agent interaction)
- Web UI (MoldBook / community features)

**Origin — the WhatsApp relay:**
The first prototype was "hooking up WhatsApp to Claude Code — one-shot the CLI, message comes in, I call the CLI with minus P, it does its magic, I get the string back, I send it back to WhatsApp." Built in 1 hour.

**Why messaging won:**
- Accessible anywhere (even shaky Moroccan internet on a birthday trip)
- Natural interface — talking to a friend, not operating a tool
- Voice messages work naturally (the famous audio message incident)
- "There is something magical about that experience being able to use a chat client to talk to an agent versus sitting behind a computer."

**Thin visible UI, thick agent framework**: Luo Fuli argues that the visible interaction layer can be thin while the important logic sits in the framework: message channels, scheduled tasks, heartbeat tasks, model routing, and daily-life workflows that make OpenClaw more general than a coding-only tool.

**18 terminal windows:** Despite the messaging-first philosophy, Peter's development workflow uses 5–10 terminal windows across multiple monitors. This was the behavior that inspired the Codex team to build the Codex App.

**PSPDFKit background influence:** Peter's 13 years building a polished PDF framework instilled an obsession with feel — "Software is all about how it feels, much more so than the feature set."

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Primary interface** | Terminal CLI | Terminal CLI → App | Messaging apps |
| **UI framework** | Custom Ink (React for terminals) | ratatui (Rust native TUI) | Web + messaging gateways |
| **Desktop app** | Co-work (VM-sandboxed) | Codex App (progressive disclosure) | — |
| **IDE extensions** | VS Code, JetBrains | VS Code, Cursor, Windsurf | — |
| **Mobile** | iOS, Android | — | Via messaging apps |
| **Web** | claude.ai/code | chatgpt.com/codex | MoldBook community |
| **Messaging** | Slack integration | — | WhatsApp, Telegram, Discord, Signal |
| **API protocol** | MCP client | JSON-RPC 2.0 (app-server) | Gateway architecture |
| **Non-engineer UX** | Co-work (dedicated product) | Codex App (progressive disclosure) | Messaging-first (natural) |
| **Original form** | Terminal chatbot | Cloud async agent | WhatsApp relay |

## Key Quotes

> "It's unbelievable that we're still using a terminal." — [Boris Cherny](references/entities/boris-cherny.md)

> "If you told me six months ago I'd be writing a third/half of my code on a phone, that's crazy." — [Boris Cherny](references/entities/boris-cherny.md)

> "We started seeing Peter Steinberger with like 18 terminals across three monitors." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

> "There is something magical about being able to use a chat client to talk to an agent versus sitting behind a computer." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "We try to make it so it's almost like playing a game. You're just discovering what's next." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)
- [blog--claude-code--claude-code-on-the-web](sources/blog-claude-code-claude-code-on-the-web.md)
- [blog--claude-code--claude-code-and-slack](sources/blog-claude-code-claude-code-and-slack.md)
- [blog--claude-code--cowork-for-enterprise](sources/blog-claude-code-cowork-for-enterprise.md)
- [blog--codex--openai-news__2025-05-16__introducing-codex](sources/blog-codex-openai-news-2025-05-16-introducing-codex.md)
- [blog--codex--openai-news__2026-02-02__introducing-the-codex-app](sources/blog-codex-openai-news-2026-02-02-introducing-the-codex-app.md)

# Auto mode for Claude Code | Claude

## Auto mode for Claude Code

Auto mode provides a safer long-running alternative to `--dangerously-skip-permissions`.

-

Category

Claude Code

Product announcements

-

Product

Claude Code

-

Date

March 24, 2026

-

Reading time

5

min

-

Share
Copy link
https://claude.com/blog/auto-mode

Today, we're introducing auto mode, a new permissions mode in Claude Code where Claude makes permission decisions on your behalf, with safeguards monitoring actions before they run. It's available now as a research preview on the Team plan, and coming to the Enterprise plan and API users in the coming days.

### How it works

Claude Code's default permissions are purposefully conservative: every file write and bash command asks for approval. It’s a safe default, but it means you can't kick off a large task and walk away, since Claude will request frequent human approvals along the way. While some developers choose to bypass permission checks with --dangerously-skip-permissions, skipping permissions can result in dangerous and destructive outcomes and should not be used outside of isolated environments.

Auto mode is a middle path that lets you run longer tasks with fewer interruptions while introducing less risk than skipping all permissions. Before each tool call runs, a classifier reviews it to check for potentially destructive actions like mass deleting files, sensitive data exfiltration, or malicious code execution.

Actions that the classifier deems as safe proceed automatically, and risky ones get blocked, redirecting Claude to take a different approach. If Claude insists on taking actions that are continually blocked, it will eventually trigger a permission prompt to the user.

### What to expect

Auto mode reduces risk compared to --dangerously-skip-permissions but doesn't eliminate it entirely, and we continue to recommend using it in isolated environments. The classifier may still allow some risky actions: for example, if user intent is ambiguous, or if Claude doesn't have enough context about your environment to know an action might create additional risk. It may also occasionally block benign actions. We’ll continue to improve the experience over time.

Auto mode may have a small impact on token consumption, cost, and latency for tool calls.

### Getting started

Auto mode is available in Claude Code as a research preview for Claude Team users today, and will roll out to Enterprise and API users in the coming days. It works with both Claude Sonnet 4.6 and Opus 4.6.

- For admins: Auto mode will soon be available for all Claude Code users on Enterprise, Team, and Claude API plans. To disable it for the CLI and VS Code extension, set "disableAutoMode": "disable" in your managed settings. Auto mode is disabled by default on the Claude desktop app, and can be toggled on using Organization Settings -> Claude Code.
- For developers: Run `claude --enable-auto-mode` to enable auto mode, then cycle to it with Shift+Tab. On Desktop and in the VS Code extension, first toggle auto mode on in Settings -> Claude Code, then select it from the permission mode drop-down in a session.

Explore the docs for more information.

No items found.

PrevPrev

0/5

NextNext

Get Claude Code

-
Desktop

-
VS Code

-
JetBrains

-
On the web

-
Slack

curl -fsSL https://claude.ai/install.sh | bash

Copy command to clipboard

irm https://claude.ai/install.ps1 | iex

Copy command to clipboard

Or read the documentation

Try Claude Code

Try Claude CodeTry Claude Code

Developer docs

Developer docsDeveloper docs

eBook

###

FAQ

No items found.

### Related posts

Explore more product news and best practices for teams building with Claude.

Apr 9, 2026

#### The advisor strategy: Give agents an intelligence boost

Product announcements

The advisor strategy: Give agents an intelligence boostThe advisor strategy: Give agents an intelligence boost

The advisor strategy: Give agents an intelligence boostThe advisor strategy: Give agents an intelligence boost

Jun 25, 2025

#### Turn ideas into interactive AI-powered apps

Product announcements

Turn ideas into interactive AI-powered appsTurn ideas into interactive AI-powered apps

Turn ideas into interactive AI-powered appsTurn ideas into interactive AI-powered apps

Apr 9, 2026

#### Making Claude Cowork ready for enterprise

Product announcements

Making Claude Cowork ready for enterpriseMaking Claude Cowork ready for enterprise

Making Claude Cowork ready for enterpriseMaking Claude Cowork ready for enterprise

Apr 8, 2026

#### Claude Managed Agents: get to production 10x faster

Product announcements

Claude Managed Agents: get to production 10x fasterClaude Managed Agents: get to production 10x faster

Claude Managed Agents: get to production 10x fasterClaude Managed Agents: get to production 10x faster

### Transform how your organization operates with Claude

See pricing

See pricingSee pricing

Contact sales

Contact salesContact sales

Get the developer newsletter

Product updates, how-tos, community spotlights, and more. Delivered monthly to your inbox.

SubscribeSubscribe

Please provide your email address if you'd like to receive our monthly developer newsletter. You can unsubscribe at any time.

Thank you! You’re subscribed.

Sorry, there was a problem with your submission, please try again later.

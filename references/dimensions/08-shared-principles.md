---
tags: [dimension]
dimension: 08
title: Shared Principles
products: [Claude Code, Codex, OpenClaw]
source_count: 16
---

# Shared Principles

## Overview

Despite different implementations, philosophies, and creators, Claude Code, Codex, and OpenClaw converge on a remarkable set of shared principles. These convergences are significant because they emerged independently — the creators arrived at similar conclusions from different starting points.

## 1. Minimal Scaffolding / Don't Put the Model in a Box

All three avoid rigid workflows. The agent loop is simple: send tools + messages → get response → execute → loop. The model decides what to do, in what order.

> "Instead of explicitly programming step 1 and step 2, provide the model with a set of tools and a goal." — [Boris Cherny](references/entities/boris-cherny.md)

> "The AI model is the brain, the harness is the body." — [Greg Brockman](references/entities/greg-brockman.md)

> "I have a conversation with the model. I say 'let's discuss, give me options' — they will not build things until I say build." — [Peter Steinberger](references/entities/peter-steinberger.md)

## 2. Build for the Model 6 Months From Now

All three teams learned that investments in scaffolding become tech debt when the model improves.

- **Claude Code** built RAG, then deleted it when models got good enough at agentic search
- **Codex** shelved the CLI, built a cloud product, then pivoted back when the market demanded local
- **OpenClaw** started simple and let the agent's improving capabilities drive feature additions

> "Don't build for the model of today, build for the model 6 months from now." — [Boris Cherny](references/entities/boris-cherny.md)

> "Avoid medium-term product roadmaps. Plan for near-term (max 8 weeks) or long-term vision." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

> "Every time someone said 'the AI can't do this,' within months it could." — [Peter Steinberger](references/entities/peter-steinberger.md)

Luo Fuli adds a co-evolution version of the same principle: models and agent frameworks should move forward together. A strong framework can unlock mid-level or smaller models, while model improvements should reshape the framework.

## 3. Layered Safety (Swiss Cheese / Defense in Depth)

Neither relies on a single safety mechanism. All stack multiple independent layers:
- **Claude Code**: 8 layers from model alignment to VM sandboxing
- **Codex**: 4 layers from approval modes to kernel sandboxing
- **OpenClaw**: Prompt constraints + Docker isolation + community oversight

## 4. CLI as the Core, GUI as a Wrapper

All started in the terminal. All added GUIs later. All keep the CLI as the primary interface for power users.

- **Claude Code**: Terminal → Co-work GUI
- **Codex**: 10x CLI → Cloud → CLI again → Codex App
- **OpenClaw**: WhatsApp relay (CLI under the hood) → messaging-first

## 5. Users Will Surprise You

All were shocked by non-technical adoption:
- **Claude Code**: Finance team, designers, data scientists all installing terminal tools
- **Codex**: Users t-muxing 18 terminals across monitors; PM using it for Slack summaries
- **OpenClaw**: Design agency owner with "25 little web services that help my business"

> "People will only do a thing that they already do. You can't get people to do a new thing." — [Boris Cherny](references/entities/boris-cherny.md)

## 6. Sub-Agents as Architecture

All three implement or practice sub-agent spawning as a core capability:
- **Claude Code**: Typed sub-agents, Coordinator Mode, Agent Teams
- **Codex**: Ghost snapshots, emerging sub-agent support, cloud delegation vision
- **OpenClaw**: Manual orchestration of 5–10 parallel agents

The vision is identical: one orchestrator directing many workers with independent context windows.

Luo Fuli describes this as swarm intelligence: multiple users and sub-agents can improve the agent framework and evaluate research ideas in parallel.

## 7. MCP as the Extension Protocol

All three integrate or support MCP (Model Context Protocol) for connecting to external services. This is becoming the standard for agent tool extensibility.

## 8. Permission Escalation

All three use tiered permission models:
- **Claude Code**: default → auto → bypass / run once → session → always
- **Codex**: suggest → auto-edit → full-auto
- **OpenClaw**: Prompt-level constraints → Docker isolation

## 9. The "Builder" Identity

All three creators describe a shift from "software engineer" to "builder":
> "The year of ADHD — humans as managers, models as workers." — [Boris Cherny](references/entities/boris-cherny.md)
> "All of the lines between career ladders are blurring and we're all builders altogether." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)
> "I do agentic engineering. The word builder captures it." — [Peter Steinberger](references/entities/peter-steinberger.md)

## Comparison Table

| Principle | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Minimal scaffolding** | "Corollary of the Bitter Lesson" | "Harness matters but model leads" | "Just have a conversation" |
| **Future-proof** | Deleted RAG, rewrites constantly | Cloud → CLI → App evolution | Let model capabilities drive |
| **Layered safety** | 8 soft layers (Swiss cheese) | 4 hard layers (kernel-enforced) | Prompt + Docker + community |
| **CLI-first** | Accidental, then embraced | Strategic, then expanded | CLI under messaging layer |
| **Non-technical adoption** | Finance, sales, data science | PMs, designers writing code | Design agencies, non-coders |
| **Sub-agents** | Typed, with Coordinator Mode | Ghost snapshots, cloud fleets | Manual multi-terminal |
| **MCP support** | Full client + skills directory | Full client/server crate | Supports MCP via underlying agent |
| **Permission tiers** | 3 modes + graduated approval | 3 modes + kernel sandbox | Prompt-level + Docker |

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--alex-roman-2026-04-05](sources/interview-codex-alex-roman-2026-04-05.md)
- [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

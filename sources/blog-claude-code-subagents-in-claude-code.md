---
tags: [source, blog]
product: Claude Code
date: 2026-04-07
dimensions: [05, 01, 02, 04]

---

# Blog — Claude Code — Sub-agents in Claude Code

**Source:** `raw/blogs/claude-code/subagents-in-claude-code.md`
**Product:** [Claude Code](references/entities/claude-code.md)
**Published:** April 7, 2026
**URL:** https://claude.com/blog/subagents-in-claude-code

## Summary

Comprehensive guide to Claude Code's sub-agent architecture: when to use them, how to invoke them, and practical patterns for orchestration. Sub-agents are **isolated Claude instances with their own context windows** — "the browser tabs of a Claude Code session."

## Key Technical Details

### Sub-agent Types
- **General-purpose agents** — complex multi-step tasks
- **Plan agents** — research codebases before presenting implementation strategies
- **Explore agents** — fast, read-only code search

### Invocation Methods (escalating automation)
1. **Conversational** — natural language ("Use a subagent to explore...")
2. **Custom subagents** — persistent specialists defined in `.claude/agents/` (project) or `~/.claude/agents/` (user)
3. **CLAUDE.md instructions** — define policies for automatic delegation
4. **Skills** — reusable multi-step workflows in `.claude/skills/`
5. **Hooks** — event-driven automation (e.g., auto-review on every commit)

### Custom Sub-agent File Format
```yaml
---
name: security-reviewer
description: Reviews code changes for security vulnerabilities...
tools: Read, Grep, Glob
model: sonnet
---
[System prompt content]
```

### When to Use Sub-agents
- **Research-heavy tasks** — synthesize findings vs dumping raw files
- **Multiple independent tasks** — parallel execution (3 sub-agents finish faster)
- **Fresh perspective** — clean slate without conversation bias
- **Verification** — independent review before committing
- **Pipeline workflows** — distinct phases (design → implement → test)

### When NOT to Use
- Sequential, dependent work
- Same-file edits (conflict risk)
- Small/simple tasks
- Too many specialist agents (floods delegation)
- Tasks requiring inter-agent coordination (use Agent Teams instead)

### Hook Example: Stop Hook
A Stop hook can block Claude from ending its turn until tests pass — enabling automated quality gates without human intervention.

## Key Quote

> "A subagent is an isolated Claude instance with its own context window. It takes a task, does the work, and returns only the result. Think of subagents as the browser tabs of a Claude Code session."

## Relevant Dimensions

- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — Core sub-agent architecture and orchestration patterns
- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — How sub-agents integrate with the main loop
- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — Custom sub-agents as reusable tool definitions
- [04 - Context Management](references/dimensions/04-context-management.md) — Context isolation between main agent and sub-agents

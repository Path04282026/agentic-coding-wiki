---
tags: [source, blog]
product: Claude Code
date: 2026-01-23
dimensions: [05]

author: Cara Phillips
---

# Blog — Claude Code — Building Multi-Agent Systems

**Source:** `raw/blogs/claude-code/building-multi-agent-systems-when-and-how-to-use-them.md`
**Product:** [Claude Code](references/entities/claude-code.md)
**Published:** January 23, 2026
**URL:** https://claude.com/blog/building-multi-agent-systems-when-and-how-to-use-them

## Summary

Anthropic's authoritative guide to multi-agent system design. Focuses on the orchestrator-subagent pattern. **Key insight: most multi-agent systems are over-designed — start with a single agent.**

## Key Design Principles

### Three Valid Reasons for Multi-Agent
1. **Context protection** — When subtask generates >1000 tokens irrelevant to main task
2. **Parallelization** — Independent research paths that can explore larger search spaces
3. **Specialization** — Tool set (20+ tools degrades performance), system prompt, or domain expertise

### Critical Anti-Pattern: Problem-Centric Decomposition
> "Problem-centric decomposition (one agent writes features, another writes tests, a third reviews) creates constant coordination overhead."

**Instead:** Use **context-centric decomposition** — group work by what context it requires, not by type.

### The Verification Subagent Pattern
- One pattern that "consistently works well across domains"
- Verifier blackbox-tests output without needing implementation context
- **"Early victory problem"**: verifiers announce passing after minimal testing
- Mitigation: explicit instructions, negative tests, comprehensive checks

### Cost Reality
- Multi-agent systems use **3-10x more tokens** than single-agent for equivalent tasks
- Parallelism helps latency but increases total compute
- "The primary benefit of parallelization is **thoroughness, not speed**"

### Tool Search Tool
- When an agent has 15-20+ tools, consider Tool Search Tool: dynamic tool discovery
- Can reduce token usage by up to 85% while improving selection accuracy
- Use before reaching for multi-agent

## Key Quote

> "We've seen teams invest months building elaborate multi-agent architectures only to discover that improved prompting on a single agent achieved equivalent results."

## Relevant Dimensions

- [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md) — The authoritative Anthropic guide to multi-agent design

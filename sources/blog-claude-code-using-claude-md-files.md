---
tags: [source, blog]
product: Claude Code
date: 2025-11-25
dimensions: [04]

---

# Blog — Claude Code — Using CLAUDE.md Files

**Source:** `raw/blogs/claude-code/using-claude-md-files.md`
**Product:** [Claude Code](references/entities/claude-code.md)
**Published:** November 25, 2025
**URL:** https://claude.com/blog/using-claude-md-files

## Summary

The definitive guide to CLAUDE.md — Claude Code's project configuration system. CLAUDE.md becomes part of Claude's system prompt, loaded every conversation.

## Key Technical Details

### What CLAUDE.md Is
- Configuration file that lives in your repository
- Provides Claude with **persistent context** about your project
- Becomes part of Claude's **system prompt** — loaded automatically every conversation
- Eliminates repeating the same architectural decisions, testing requirements, coding standards

### Placement Hierarchy
- **Repository root** — shared with team (via version control)
- **Parent directories** — monorepo setups
- **Home folder** — universal across all projects

### Recommended Content
- Project summary and architecture
- Key directories (tree output)
- Main dependencies and patterns
- Common bash commands
- Code style guidelines
- Testing instructions
- Custom tool documentation (including MCP server usage)
- Standard workflows (explore-plan-code-commit, TDD, visual iteration)

### The `/init` Command
- Analyzes codebase: package files, docs, config, code structure
- Generates starter CLAUDE.md tailored to project
- Can be re-run on existing projects to suggest improvements

### Key Practices
- **Start simple, expand deliberately** — resist comprehensive files upfront
- **Keep concise** — added to context every time (context engineering concern)
- **Break out files** — reference separate markdown files from CLAUDE.md for large projects
- **No sensitive data** — no API keys, credentials, connection strings
- **Use `#` key** to add instructions during sessions (accumulates organically)
- **Use `/clear`** between distinct tasks to reset accumulated context

### Subagents + CLAUDE.md Interaction
- CLAUDE.md defines **policies** for when to delegate to subagents
- Custom subagents define **who** the specialists are
- Together they create automated delegation workflows

### Custom Slash Commands
- Stored as markdown files in `.claude/commands/`
- Support arguments via `$ARGUMENTS` or `$1`, `$2`
- Can be created by asking Claude to make them

## Key Design Insight

> "CLAUDE.md files turn Claude Code from a general-purpose assistant into a tool configured specifically for your codebase."

> "Treat customization as an ongoing practice rather than a one-time setup task."

## Relevant Dimensions

- [04 - Context Management](references/dimensions/04-context-management.md) — The authoritative reference for CLAUDE.md usage and design

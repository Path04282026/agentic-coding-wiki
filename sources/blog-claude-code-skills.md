---
tags: [source, blog]
product: Claude Code
date: 2025-10-16
dimensions: [02, 07]

---

# Blog — Claude Code — Skills Overview

**Source:** `raw/blogs/claude-code/skills.md`
**Product:** [Claude Code](references/entities/claude-code.md)
**Published:** October 16, 2025
**URL:** https://claude.com/blog/skills

## Summary

Launch announcement for Agent Skills — folders of instructions, scripts, and resources that Claude loads on demand for specialized work. Cross-platform (Claude apps, Claude Code, API).

## Key Technical Details

### What Skills Are
- Folders with `SKILL.md` manifest + instructions, scripts, and resources
- Loaded only when relevant to current task
- Model scans available skills and loads minimal information needed

### Properties
- **Composable** — stack together, Claude coordinates automatically
- **Portable** — same format everywhere (apps, Code, API)
- **Efficient** — only loads what's needed
- **Powerful** — can include executable code for reliability

### Cross-Platform Support
- **Claude apps** — auto-invoke based on task, visible in chain of thought
- **API** — Messages API + `/v1/skills` endpoint for programmatic management, requires Code Execution Tool beta
- **Claude Code** — install via plugins from `anthropics/skills` marketplace, share via version control

### Built-in Skills
- Excel spreadsheets with formulas
- PowerPoint presentations
- Word documents
- Fillable PDFs

### Skill Creation
- `skill-creator` skill provides interactive guidance
- Claude asks about workflow, generates folder structure, formats `SKILL.md`
- No manual file editing required

### Agent Skills as Open Standard
- Published December 2025 update
- Organization-wide management
- Partner directory (Box, Canva, Notion)
- Cross-platform portability standard

## Customer Quotes
- **Box**: "Transform stored files into PowerPoint, Excel, Word"
- **Canva**: "Unlocks new ways to bring Canva deeper into agentic workflows"
- **Notion**: "Less prompt wrangling, more predictable results"
- **Yusuke Kaji (management accounting)**: "What once took a day, we can now accomplish in an hour"

## Relevant Dimensions

- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — Skills as the primary tool extension mechanism
- [07 - Feature Gating and Internals](references/dimensions/07-feature-gating-and-internals.md) — Skills rollout strategy across platforms

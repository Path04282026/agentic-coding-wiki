---
name: ingest-source
description: >
  Ingest a new source document (blog post, interview transcript, article, or raw material)
  into the Agentic Coding Wiki knowledge base. Use this skill when the user wants to add,
  analyze, or integrate new material about agentic coding products (Claude Code, Codex,
  OpenClaw, Hermes Agent, or related topics). The skill reads the raw material, generates
  a structured summary, tags it to relevant dimensions, and updates the knowledge base.
disable-model-invocation: false
user-invocable: true
---

# Ingest Source — Workflow Instructions

This skill adds new source material to the knowledge base through a structured pipeline.

## When to Activate

- User says "add this article/blog/interview to the wiki"
- User provides a new source document and asks for analysis
- User wants to update the knowledge base with new information
- User mentions "ingest", "add source", or "update wiki"

## Step-by-Step Ingest Pipeline

### Step 1: Classify the Source

Determine the source type and metadata:

| Field | How to Determine |
|-------|-----------------|
| `type` | `blog`, `interview`, `article`, `doc` |
| `product` | `Claude Code`, `Codex`, `OpenClaw`, or multiple |
| `date` | Publication or event date |
| `speaker` | (interviews only) Who is speaking |
| `dimensions` | Which of the 9 dimensions this source covers (see below) |

### Step 2: Save the Raw Material

Save the original content to `sources/blogs/`, `sources/interviews/`, or `sources/summaries/` depending on type.

**Naming convention:** `{product-slug}/{kebab-case-title}.md`

Examples:
- `sources/blogs/claude-code/agent-skills-v2-update.md`
- `sources/interviews/codex-20260501-greg-brockman.md`

### Step 3: Generate a Structured Summary

Create a summary page at `sources/{type}-{product}-{slug}.md` following this template:

```markdown
---
tags: [source, {type}]
product: {Product Name}
date: {YYYY-MM-DD}
dimensions: [{list of dimension numbers}]
speaker: {Speaker Name}  # interviews only
---

# {Type} — {Product} — {Title or Speaker} ({Date})

**Format:** {Blog post / Podcast interview / Technical article / etc.}
**Speaker:** [{Speaker}](references/entities/{speaker-kebab}.md) — {Role} at [{Org}](references/entities/{org-kebab}.md)

## Key Takeaways by Dimension

### [0X - Dimension Name](references/dimensions/0X-dimension-slug.md)
- Key point 1 (specific, factual, sourced)
- Key point 2
- Key point 3

### [0Y - Another Dimension](references/dimensions/0Y-dimension-slug.md)
- Key point 1
- Key point 2

## Notable Quotes

> "Direct quote from the source" — **Speaker Name**

> "Another significant quote" — **Speaker Name**
```

### Step 4: Update Dimension Files

For each relevant dimension in `references/dimensions/0X-*.md`:

1. Read the existing dimension file
2. Find the section for the relevant product
3. Add new evidence points with citation links
4. Update the `source_count` in YAML frontmatter
5. Update comparison tables if the new source reveals differences

### Step 5: Update Entity Files

For each person or product mentioned:

1. Check if an entity page exists in `references/entities/`
2. If yes, add new facts or quotes
3. If no, create a new entity page following existing format

### Step 6: Update Index

Append the new source to `references/index.md` in the appropriate section.

## The 9 Dimensions — Quick Reference

Use these to tag every source:

| # | Dimension | Tag When Source Discusses... |
|---|-----------|---------------------------|
| 01 | Agent Loop | Core loop architecture, streaming, event handling, tool execution flow |
| 02 | Tool Systems | Tool design, tool counts, MCP, skills, code editing approach |
| 03 | Security & Sandboxing | Permissions, sandboxing, trust models, safety boundaries |
| 04 | Context Management | System prompts, CLAUDE.md, compaction, memory, token budgets |
| 05 | Multi-Agent Architecture | Sub-agents, orchestration, coordinator patterns, swarm |
| 06 | UI & Distribution | CLI, desktop, web, mobile, Slack, IDE extensions |
| 07 | Feature Gating & Internals | Feature flags, internal features, open vs proprietary |
| 08 | Shared Principles | Cross-product patterns: minimal scaffolding, CLI-first, MCP |
| 09 | Philosophical Divergence | Design philosophy, "trust the model" vs alternatives |

## Quality Rules

1. **Every claim needs a source** — link back to the summary page
2. **Quotes must be attributed** — link to the speaker's entity page
3. **Use dimension numbers consistently** — always 2-digit (01–09)
4. **Three products, always** — if updating a dimension, ensure all three products have sections
5. **Comparison tables are mandatory** — update the 3-way table if new evidence changes the picture
6. **Filenames in kebab-case** — no spaces, all lowercase

## After Ingestion — Publish to GitHub

### Step 7: Branch, Commit, Push, and Create PR

After all files are created/updated, automatically publish the changes:

```bash
# 1. Ensure we are in the skill repo
cd ~/.claude/skills/agentic-coding-wiki  # or the project-level path

# 2. Create a feature branch
git checkout -b ingest/{type}-{product}-{slug}

# 3. Stage all changes
git add -A

# 4. Commit with a structured message
git commit -m "Ingest: {Type} — {Product} — {Title} ({Date})

Source: {URL or description}

New files:
- {list of created files}

Updated files:
- {list of modified dimension/entity pages}

Dimensions tagged: {list of dimension numbers}"

# 5. Push to remote
git push origin ingest/{type}-{product}-{slug}

# 6. Create a Pull Request (requires `gh` CLI)
gh pr create \
  --title "Ingest: {Type} — {Product} — {Title}" \
  --body "## New Source\n- **Type:** {type}\n- **Product:** {product}\n- **Date:** {date}\n- **Dimensions:** {dims}\n\n## Changes\n- {summary of files created/updated}" \
  --base main

# 7. Return to main branch
git checkout main
```

**Important:** Always use a feature branch + PR instead of pushing directly to `main`, so other team members can review the ingested content.

If the user says "push directly" or "no PR needed", you may push to `main` directly:

```bash
git add -A && git commit -m "Ingest: {Type} — {Product} — {Title} ({Date})" && git push origin main
```

## Report to User

After completing the full pipeline, report:

1. ✅ **Source added:** type, product, date, URL
2. 📊 **Dimensions updated:** which dimensions received new evidence
3. 📝 **Files created:** list of new files
4. ✏️ **Files updated:** list of modified files with what changed
5. 👤 **Entities:** any new entity pages created or existing ones updated
6. 🔗 **GitHub:** PR URL or commit URL for the pushed changes

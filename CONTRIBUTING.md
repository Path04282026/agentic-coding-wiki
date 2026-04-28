# Contributing to the Agentic Coding Wiki

## Adding New Source Material

This wiki grows through team contributions. Here's how to add new material.

### Quick Start (Claude Code)

```bash
# 1. Clone the repo (if you haven't already)
git clone https://github.com/Path04282026/agentic-coding-wiki.git ~/.claude/skills/agentic-coding-wiki

# 2. Create a branch
cd ~/.claude/skills/agentic-coding-wiki
git checkout -b add/your-source-name

# 3. Drop your raw material into the right place
#    Blog post → sources/blogs/{product}/
#    Interview → sources/interviews/
#    Article   → sources/blogs/{product}/

# 4. Ask Claude Code to ingest it
#    In Claude Code, simply say:
```

**Example prompts for Claude Code:**

```
> Analyze the new blog post I added at sources/blogs/claude-code/new-article.md
  and integrate it into the wiki

> I added a new interview transcript at sources/interviews/codex-20260501.md,
  please ingest it into the knowledge base

> Update the wiki with this new article about OpenClaw's latest release
```

Claude will automatically:
1. Read the raw material
2. Generate a structured summary with YAML frontmatter
3. Tag it to relevant dimensions (01–09)
4. Update dimension pages with new evidence
5. Update entity pages with new facts
6. Add it to the index

### Then submit your changes:

```bash
git add -A
git commit -m "Add source: {type}--{product}--{slug}"
git push origin add/your-source-name
# Open a PR on GitHub
```

## What You Can Contribute

| Material Type | Where to Put It | Example |
|--------------|-----------------|---------|
| Blog post | `sources/blogs/{product}/` | Anthropic engineering blog |
| Interview/podcast | `sources/interviews/` | Podcast transcript |
| Technical article | `sources/blogs/{product}/` | Deep-dive analysis |
| Product documentation | `sources/summaries/` | Official docs excerpt |

## Supported Products

- `claude-code` — Anthropic's Claude Code
- `codex` — OpenAI's Codex
- `openclaw` — Peter Steinberger's OpenClaw/ClawBot
- `hermes` — Hermes Agent (case study)

## File Naming Rules

- All lowercase, kebab-case: `my-new-article.md` ✅ not `My New Article.md` ❌
- Source summaries: `{type}-{product}-{slug}-{date}.md`
  - Example: `blog-claude-code-skills-v2-update-2026-05-01.md`
- Raw material: descriptive name under the right product folder
  - Example: `sources/blogs/claude-code/skills-v2-update.md`

## Quality Checklist

Before submitting a PR, verify:

- [ ] Raw material saved in the correct `sources/` subfolder
- [ ] Structured summary has valid YAML frontmatter
- [ ] At least one dimension tagged
- [ ] Key takeaways organized by dimension
- [ ] Notable quotes attributed with speaker name
- [ ] Relevant dimension page(s) updated with new evidence
- [ ] `references/index.md` updated with new entry
- [ ] All filenames are kebab-case

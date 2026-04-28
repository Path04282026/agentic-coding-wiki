---
tags: [source, blog]
product: Codex
date: 2026-02-11
dimensions: [02, 04]

---

# Blog — Codex — Shell Skills Compaction Tips

**Source:** `raw/blogs/codex/openai-developers-blog__2026-02-11__shell-skills-compaction-tips-for-long-running-agents-that-do-real-work.md`
**Product:** [Codex](references/entities/codex.md)
**Published:** February 11, 2026
**URL:** https://developers.openai.com/blog/skills-shell-tips/

## Summary

Practical guide to three agentic primitives: Skills (reusable versioned procedures), Shell (hosted execution), and server-side Compaction. Based on developer feedback and Glean customer experience.

## Key Technical Details

### Skills as "Procedures"
- Bundle of files + `SKILL.md` manifest with frontmatter and instructions
- Loaded on demand — platform shows name/description/path, model decides when to invoke
- Aligned with the **Agent Skills open standard**

### Shell Tool
- Hosted containers managed by OpenAI or local shell (same semantics, you control machine)
- Full terminal environment: install dependencies, run scripts, write outputs
- `/mnt/data` as the standard artifact handoff boundary

### Server-Side Compaction
- **New**: When context crosses threshold, compaction runs automatically in-stream
- **Standalone**: `/responses/compact` for explicit control
- Keeps long runs moving without manual context surgery

### 10 Practical Tips
1. **Write skill descriptions like routing logic** — "Use when / Don't use when" blocks
2. **Add negative examples** — Glean saw 20% drop in triggering, recovered after negatives
3. **Put templates inside skills** — loaded only when triggered (effectively free otherwise)
4. **Design for long runs early** — container reuse + `previous_response_id`
5. **Explicit invocation** — "Use the X skill" for deterministic routing
6. **Skills + networking = high risk** — keep network allowlists strict
7. **`/mnt/data`** = artifact boundary
8. **Two-layer allowlists** — org-level + request-level
9. **`domain_secrets`** — model sees placeholders, sidecar injects real values
10. **Same APIs locally and in cloud** — portable between dev and production

### Glean Customer Results
- Salesforce skill: eval accuracy **73% → 85%**, TTFT reduced **18.1%**
- Key tactics: careful routing, negative examples, embedded templates

### Three Build Patterns
1. **Install → Fetch → Artifact** — simplest shell pattern
2. **Skills + Shell** — repeatable workflows with encoded procedures
3. **Skills as Enterprise Workflow Carriers** — living SOPs updated as org evolves

## Key Quote

> "Skills become living SOPs (standard operating procedures): updated as your org evolves, and executed consistently by agents."

## Relevant Dimensions

- [02 - Tool Systems](references/dimensions/02-tool-systems.md) — Skills architecture and shell tool design
- [04 - Context Management](references/dimensions/04-context-management.md) — Server-side compaction strategies

---
tags: [navigation]
title: Operations Log
---

# Operations Log

> Append-only log of all batch operations performed on this wiki.

---

## 2026-04-11 — Initial Wiki Build

**Operator:** LLM (Claude Opus 4.6 via Antigravity)
**Operation:** Full wiki initialization

### Actions Taken

1. **Created directory structure:**
   - `raw/blogs/claude-code/`, `raw/blogs/codex/`, `raw/interviews/`, `raw/summaries/`
   - `wiki/.obsidian/`, `wiki/sources/`, `wiki/entities/`, `wiki/dimensions/`, `wiki/comparisons/`

2. **Copied raw source files:**
   - 57 Claude Code blog posts → `raw/blogs/claude-code/`
   - 31 Codex blog posts → `raw/blogs/codex/`
   - 13 interview transcripts → `raw/interviews/`
   - 8 existing summaries → `raw/summaries/`

3. **Created Layer 3 schema:** `CLAUDE.md` (governance file)

4. **Created 9 dimension pages:**
   - `01 - Agent Loop.md`
   - `02 - Tool Systems.md`
   - `03 - Security and Sandboxing.md`
   - `04 - Context Management.md`
   - `05 - Multi-Agent Architecture.md`
   - `06 - UI and Distribution.md`
   - `07 - Feature Gating and Internals.md`
   - `08 - Shared Principles.md`
   - `09 - Philosophical Divergence.md`

5. **Created 9 entity pages:**
   - Products: `Claude Code.md`, `Codex.md`, `OpenClaw.md`
   - People: `Boris Cherny.md`, `Greg Brockman.md`, `Peter Steinberger.md`, `Michael Bolin.md`, `Alex (Codex PM).md`, `Tibo Such.md`

6. **Created navigation pages:**
   - `overview.md` (Start Here)
   - `timeline.md` (chronological milestones)
   - `index.md` (page catalog)
   - `log.md` (this file)

7. **Created comparison page:**
   - `Three-Way Comparison.md`

### Sources Ingested

- 13 interview transcripts (via 8 existing summaries + direct transcript reading)
- Comparative analysis from existing `Codex-vs-Claude-Code-Comparison.md`

### Pending Work (Batch 1)

- [x] ~~Create `wiki/sources/` pages for each of the 13 interview transcripts~~ ✅
- [x] ~~Create `wiki/sources/` pages for each of the 88 blog posts~~ ✅
- [x] ~~Create organization entity pages~~ ✅
- [ ] Enrich blog source pages with deeper content extraction
- [ ] Cross-reference blog evidence into dimension pages
- [ ] Run lint workflow to check for orphan pages and thin coverage

---

## 2026-04-11 — Source Page Ingestion (Batch 2)

**Operator:** LLM (Claude Opus 4.6 via Antigravity)
**Operation:** Bulk creation of 99 source pages (interviews + blogs)

### Actions Taken

1. **13 interview source pages** — each with YAML frontmatter, dimension tags, key takeaways, notable quotes
2. **55 Claude Code blog source pages** — with product/dimension tags and summaries
3. **31 Codex blog source pages** — with product/dimension tags and summaries
4. **2 organization entity pages** — Anthropic.md, OpenAI.md
5. Updated `index.md` with complete 124-page catalog

### Final Wiki Size: **124 pages**

| Section | Count |
|---|---|
| Dimensions | 9 |
| Entities | 11 |
| Comparisons | 1 |
| Navigation | 4 |
| Sources | 99 |
| **Total** | **124** |

---

## 2026-04-11 — Blog Enrichment & Link Repair (Batch 3)

**Operator:** LLM (Claude Opus 4.6 via Antigravity)
**Operation:** Deep-read enrichment of high-signal blog sources + wikilink repair

### Actions Taken

1. **Deep-read 8 blog posts** and enriched their source pages with:
   - Full technical details, architecture diagrams, and code examples
   - Key quotes and design principles
   - Cross-references to relevant dimensions
   
   Enriched blogs:
   - `subagents-in-claude-code` — Sub-agent types, invocation methods, custom agent file format
   - `openai-news__2026-01-23__unrolling-the-codex-agent-loop` — Prompt construction, SSE streaming, caching strategy
   - `beyond-permission-prompts` — OS-level sandboxing (Seatbelt/bubblewrap), convergence with Codex
   - `using-claude-md-files` — CLAUDE.md hierarchy, /init command, custom commands
   - `openai-news__2026-03-06__codex-security` — Codex Security agent, 14 CVEs discovered
   - `building-multi-agent-systems` — Context-centric decomposition, verification subagent pattern
   - `shell-skills-compaction` — Skills + Shell + Compaction tips, Glean case study
   - `skills` — Agent Skills launch, cross-platform support, open standard

2. **Fixed 11 broken wikilinks** across 4 dimension pages (02, 04, 06, 07)
   - Short slugs → full slugs matching actual source filenames
   
3. **Added new source references** to dimensions 01, 03, 05
   - Including newly enriched blog sources and additional relevant blogs

4. **Link integrity audit**: Zero broken `**blog--**` or `**interview--**` wikilinks remaining in any dimension page.

---

## 2026-04-14 — Chinese Guide: Claude Code Prompt Design

**Operator:** Hermes Agent (gpt-5.4 via OpenAI Codex)
**Operation:** Added a Chinese-language 5-minute reading guide for Claude Code prompt/context design

### Actions Taken

1. Read and synthesized evidence from:
   - `wiki/sources/blog--claude-code--using-claude-md-files.md`
   - `wiki/sources/interview--claude-code--boris-cherny-2026-02-17.md`
   - `wiki/sources/interview--claude-code--boris-cherny-2026-03-04.md`
   - `wiki/dimensions/04 - Context Management.md`
   - `wiki/code-analysis/prompts/Claude Code — Prompt System.md`
   - `wiki/code-analysis/context/Claude Code — Context Management.md`

2. Created guide page:
   - `wiki/5 分钟读完 Claude Code Prompt 设计.md`

3. Updated `wiki/index.md`:
   - Incremented total page count from 124 → 125
   - Added a new `Guides` section
   - Indexed the new Chinese guide page

4. Preserved raw/source immutability:
   - No files under `raw/` were modified

---

## 2026-04-14 — Diagram: Claude Code Prompt Overview

**Operator:** Hermes Agent (gpt-5.4 via OpenAI Codex)
**Operation:** Added an Obsidian-friendly overview diagram for the Chinese Claude Code prompt design guide

### Actions Taken

1. Created diagram file:
   - `wiki/Claude Code Prompt 设计总览.excalidraw`

2. Updated guide page embed:
   - `wiki/5 分钟读完 Claude Code Prompt 设计.md`
   - Added `!**Claude Code Prompt 设计总览.excalidraw**` near the top for direct Obsidian viewing

3. Diagram content covers:
   - `CLAUDE.md`
   - environment / runtime context
   - memory
   - tool prompts
   - prompt assembly engine
   - model + tools loop
   - compaction / long-term memory feedback

4. Preserved raw/source immutability:
   - No files under `raw/` were modified

---

## 2026-04-14 — Chinese Guide: 1-Minute Wiki Intro

**Operator:** Hermes Agent (gpt-5.4 via OpenAI Codex)
**Operation:** Added a Chinese-language 1-minute reading guide for the overall LLM wiki

### Actions Taken

1. Read and synthesized evidence from:
   - `wiki/overview.md`
   - `wiki/index.md`
   - `wiki/comparisons/Three-Way Comparison.md`
   - `wiki/dimensions/08 - Shared Principles.md`
   - `wiki/dimensions/09 - Philosophical Divergence.md`
   - `CLAUDE.md`

2. Created guide page:
   - `wiki/1 分钟读完这份 wiki.md`

3. Updated `wiki/index.md`:
   - Incremented total page count from 125 → 126
   - Expanded `Guides` from 1 → 2 pages
   - Indexed the new Chinese guide page

4. Preserved raw/source immutability:
   - No files under `raw/` were modified

---
---

## 2026-04-25 — Query: Hermes Agent integration planning

**Operator:** Hermes Agent
**Operation:** Inspected current wiki structure and content to discuss more efficient usage patterns and a potential Hermes Agent integration plan.

### Actions Taken

1. Read governing schema: `CLAUDE.md`
2. Read navigation and synthesis pages: `wiki/index.md`, `wiki/overview.md`, `wiki/comparisons/Three-Way Comparison.md`, `wiki/1 分钟读完这份 wiki.md`
3. Scanned wiki/raw structure and searched for existing Hermes-related content
4. Identified that no existing Hermes Agent pages or sources are present; raw sources were not modified

### Notes

- This was an advisory/query operation only; no new wiki content pages were created.

---

## 2026-04-25 — Refactor: Product Design Knowledge Base Entry

**Operator:** Hermes Agent
**Operation:** Reoriented the wiki from a source/archive-first repository toward an agentic coding product design knowledge base.

### Actions Taken

1. Created `wiki/design/` entry layer:
   - `wiki/design/index.md`
   - `wiki/design/Agentic Coding Product Design Questions.md`
   - `wiki/design/Agentic Coding Product Design Patterns.md`

2. Updated navigation:
   - `wiki/index.md`
   - Total page count updated from 126 → 158 to include existing code-analysis, Copilot/imported notes, miscellaneous workspace pages, and the new design pages.
   - Added sections for Design Knowledge Base, Code Analysis, and Workspace / Imported Notes.

3. Updated front page:
   - `wiki/overview.md`
   - Added a product-design entry point and reframed the wiki as a source-backed design knowledge base.

4. Preserved raw/source immutability:
   - No files under `raw/` were modified.

### Purpose

The wiki can now be used question-first: start from agentic coding product design problems, map them to the 9 comparative dimensions, then follow source-backed evidence and implementation deep dives.

---

## 2026-04-25 — Create: Hermes Agent Case Study

**Operator:** Hermes Agent
**Operation:** Added a Hermes Agent case-study section and mapped Hermes into the wiki's 9-dimensional agentic coding product design framework.

### Actions Taken

1. Created Hermes source/entity/case-study pages:
   - `wiki/sources/doc--hermes-agent--runtime-capabilities-2026-04-25.md`
   - `wiki/entities/Hermes Agent.md`
   - `wiki/hermes/index.md`
   - `wiki/hermes/Hermes Agent Through the 9 Dimensions.md`

2. Updated navigation and entry points:
   - `wiki/index.md`
   - `wiki/overview.md`
   - `wiki/design/index.md`

3. Kept Hermes as a case study rather than changing the main three-product schema:
   - The 9 core dimension pages remain governed by `CLAUDE.md` and still compare Claude Code, Codex, and OpenClaw.
   - Hermes is mapped through a dedicated `wiki/hermes/` section until enough source evidence exists to justify a full schema expansion.

4. Preserved raw/source immutability:
   - No files under `raw/` were modified.

### Updated Count

- `wiki/index.md` total page count updated from 158 → 162.
---

## 2026-04-25 — Create: Hermes Agent Deep Dives

**Operator:** Hermes Agent
**Operation:** Added two Hermes Agent deep-dive pages for tool-system design and context/memory architecture.

### Actions Taken

1. Created Hermes deep-dive pages:
   - `wiki/hermes/Hermes Tool System Deep Dive.md`
   - `wiki/hermes/Hermes Memory, Skills, and Session Search.md`

2. Updated navigation and entry points:
   - `wiki/index.md`
   - `wiki/overview.md`
   - `wiki/hermes/index.md`

3. Preserved the main three-product schema:
   - Hermes remains a dedicated case study under `wiki/hermes/`.
   - The 9 core dimension pages still compare Claude Code, Codex, and OpenClaw as governed by `CLAUDE.md`.

4. Preserved raw/source immutability:
   - No files under `raw/` were modified.

### Updated Count

- `wiki/index.md` total page count updated from 162 → 164.
---

## 2026-04-25 — Ingest: Hermes Self-Evolution Article

**Operator:** Hermes Agent
**Operation:** Ingested the WeChat/阿里云开发者 Hermes Agent analysis article and added a self-evolution deep dive to the Hermes case-study section.

### Source Added

- `wiki/sources/article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24.md`
  - External source: https://mp.weixin.qq.com/s/2xFei8dMx99lc-iyrZZrww
  - Author/publication: 飞樰 / 阿里云开发者
  - Focus: dynamic skill generation, background review, trajectory capture/compression, RL training, prompt/context/harness design.

### Pages Created

- `wiki/hermes/Hermes Self-Evolution Loop.md`

### Pages Updated

- `wiki/index.md`
- `wiki/overview.md`
- `wiki/hermes/index.md`
- `wiki/entities/Hermes Agent.md`
- `wiki/hermes/Hermes Agent Through the 9 Dimensions.md`
- `wiki/hermes/Hermes Tool System Deep Dive.md`
- `wiki/hermes/Hermes Memory, Skills, and Session Search.md`

### Schema Notes

- Hermes remains a case study, not a fourth first-class comparison column.
- The core 9 dimension pages remain three-product pages for Claude Code, Codex, and OpenClaw.
- The Hermes thesis was refined from `agent operating layer` to `self-evolving agent operating layer` in Hermes-specific pages.

### Raw Immutability

- No files under `raw/` were modified.
- The WeChat article was summarized as an external source page under `wiki/sources/`.

### Updated Count

- `wiki/index.md` total page count updated from 164 → 166.



## 2026-04-25 — Ingest: Prompt / Context / Harness Articles for Claude Code and OpenClaw

**Operator:** Hermes Agent

**Operation:** Added two WeChat / 阿里云开发者 external analysis articles into the wiki and created a design synthesis page connecting them with the existing Hermes self-evolution article.

**Sources added:**
- `wiki/sources/article--claude-code--prompt-context-harness-2026-04-20.md` — 飞樰 article on Claude Code Prompt / Context / Harness design.
- `wiki/sources/article--openclaw--prompt-context-harness-2026-04-13.md` — 飞樰 article on OpenClaw Prompt / Context / Harness design.

**Pages created:**
- `wiki/design/Prompt Context Harness Design Lens.md` — synthesis page treating Prompt Engineering, Context Engineering, and Harness Engineering as a reusable product-design lens across Claude Code, OpenClaw, and Hermes Agent.

**Pages updated:**
- `wiki/index.md` — total pages 166 → 169; design pages 3 → 4; sources 101 → 103; added external analysis article section.
- `wiki/overview.md` — added the Prompt / Context / Harness design lens to the product-design entry point.
- `wiki/design/index.md` — added the new design lens to recommended product-design reading paths.
- `wiki/entities/Claude Code.md` — linked the new Claude Code article summary.
- `wiki/entities/OpenClaw.md` — linked the new OpenClaw article summary.

**Schema notes:**
- `raw/` remains unchanged; article URLs are recorded in source-page frontmatter rather than copied into the raw corpus.
- The two new articles are treated as external technical analysis, not official Claude Code or OpenClaw specifications.
- Core three-product dimension pages remain unchanged; the new synthesis lives in the design layer.

---

## 2026-04-27 — Create: OpenClaw vs Hermes Agent comparison

**Operator:** Hermes Agent

**Operation:** Added a focused Markdown comparison page capturing Hermes Agent content and the OpenClaw-vs-Hermes distinction while preserving the existing three-product schema.

**Page created:**
- `wiki/comparisons/OpenClaw vs Hermes Agent.md` — contrasts OpenClaw's messaging-native personal-agent route with Hermes Agent's self-evolving agent operating layer.

**Pages updated:**
- `wiki/index.md` — total pages 169 → 170; comparisons 1 → 2; added the new comparison.
- `wiki/overview.md` — linked the focused comparison from the design and comparison navigation sections.
- `wiki/design/index.md` — added the OpenClaw-vs-Hermes page to the Hermes reading path and extended reading list.
- `wiki/hermes/index.md` — added the comparison to the Hermes case-study start-here list.
- `wiki/entities/Hermes Agent.md` — linked the new comparison from related pages.
- `wiki/entities/OpenClaw.md` — linked the new comparison from article-analysis notes.
- `wiki/design/Prompt Context Harness Design Lens.md` — linked the new comparison from related pages.

**Schema notes:**
- Hermes remains a case-study product rather than a fourth first-class column in the nine core dimension pages.
- The new comparison cites existing source pages: `doc--hermes-agent--runtime-capabilities-2026-04-25`, `article--hermes-agent--self-evolving-prompt-context-harness-2026-04-24`, and `article--openclaw--prompt-context-harness-2026-04-13`.
- No files under `raw/` were modified.

---

## 2026-04-27 — Add: Raw YouTube transcripts

**Operator:** Codex

**Operation:** Added two YouTube caption transcripts as raw interview sources.

**Raw sources added:**
- `raw/interviews/OpenClaw_20260423_Luo_Fuli.md` — Luo Fuli interview from Zhang Xiaojun Podcast, including English AI-translated captions and the Simplified Chinese caption track.
- `raw/interviews/Claude_Code_20260423_Cat_Wu.md` — Cat Wu interview from Lenny's Podcast, using the English auto-generated caption track.

**Schema notes:**
- Only raw transcript files were added.
- No `wiki/sources/` summaries, index entries, dimension pages, or entity pages were generated in this pass.

---

## 2026-04-27 — Ingest: Cat Wu Claude Code transcript

**Operator:** Codex

**Operation:** Promoted the Cat Wu raw YouTube transcript into the wiki layer.

**Source page created:**
- `wiki/sources/interview--claude-code--cat-wu-2026-04-23.md` — source summary for Lenny's Podcast interview on Claude Code, Co-work, AI-native PM, action-based agents, and multi-agent scaling.

**Entity page created:**
- `wiki/entities/Cat Wu.md`

**Pages updated:**
- `wiki/index.md` — total pages 170 → 172; entities 12 → 13; sources 103 → 104; interviews 13 → 14.
- `wiki/entities/Claude Code.md` — added Cat Wu as product lead and linked the interview note.
- `wiki/entities/Anthropic.md` — added Cat Wu and AI-native product-speed note.
- `wiki/dimensions/01 - Agent Loop.md` — added assistant-to-agent evidence; source_count 18 → 19.
- `wiki/dimensions/02 - Tool Systems.md` — added tooling-as-capability-elicitation evidence; source_count 15 → 16.
- `wiki/dimensions/04 - Context Management.md` — added connected Co-work context and model-introspection evidence; source_count 14 → 15.
- `wiki/dimensions/05 - Multi-Agent Architecture.md` — added many-remote-Claude-instances and code-review-agent evidence; source_count 12 → 13.
- `wiki/dimensions/06 - UI and Distribution.md` — added Cat Wu's product-surface map and `/powerup`; source_count 14 → 15.
- `wiki/dimensions/09 - Philosophical Divergence.md` — added current-model product taste and action-based product evidence; source_count 15 → 16.

---

## 2026-04-27 — Ingest: Luo Fuli OpenClaw transcript

**Operator:** Codex

**Operation:** Promoted the Luo Fuli raw YouTube transcript into the wiki layer.

**Source page created:**
- `wiki/sources/interview--openclaw--luo-fuli-2026-04-23.md` — source summary for Zhang Xiaojun Podcast interview on OpenClaw as an agent framework, model/framework co-evolution, swarm intelligence, and multi-agent productivity.

**Entity page created:**
- `wiki/entities/Luo Fuli.md`

**Pages updated:**
- `wiki/index.md` — total pages 172 → 174; entities 13 → 14; sources 104 → 105; interviews 14 → 15.
- `wiki/entities/OpenClaw.md` — added Luo Fuli's agent-framework interpretation and linked the interview note.
- `wiki/dimensions/01 - Agent Loop.md` — added framework-as-middle-layer evidence; source_count 19 → 20.
- `wiki/dimensions/02 - Tool Systems.md` — added model/tool orchestration evidence; source_count 16 → 17.
- `wiki/dimensions/04 - Context Management.md` — added persistent-memory/context-framework evidence; source_count 15 → 16.
- `wiki/dimensions/05 - Multi-Agent Architecture.md` — added swarm-intelligence evidence; source_count 13 → 14.
- `wiki/dimensions/06 - UI and Distribution.md` — added thin-visible-UI/thick-framework evidence; source_count 15 → 16.
- `wiki/dimensions/08 - Shared Principles.md` — added model/framework co-evolution and swarm-intelligence notes; source_count 15 → 16.
- `wiki/dimensions/09 - Philosophical Divergence.md` — added agent-framework-as-product interpretation; source_count 16 → 17.

---

## 2026-04-27 — Cleanup: Repository Structure Reorganization

**Operator:** LLM (Claude Opus 4.6 via Antigravity)
**Operation:** Structural audit and cleanup of the repository to align actual layout with the CLAUDE.md schema.

### Problems Identified

1. Four top-level directories (`Blog/`, `Interview/`, `InterviewSummary/`, `source/`) were redundant copies of content already in `raw/`.
2. Seven empty/orphan files in `wiki/` root (empty canvases, daily notes, incomplete dimension stub).
3. Chinese reading guides scattered in `wiki/` root instead of a dedicated subdirectory.
4. Copilot plugin configuration files indexed as wiki content.

### Actions Taken

1. **Deleted 4 redundant directories:**
   - `Blog/` — MD5-identical to `raw/blogs/` (56 + 32 files)
   - `Interview/` — MD5-identical to `raw/interviews/` (13 files; 2 newer files only in raw/)
   - `InterviewSummary/` — older version of `raw/summaries/` (diff confirmed: typos, less structured)
   - `source/` — 1 file, MD5-identical to `raw/summaries/`

2. **Deleted 7 empty files:**
   - `wiki/04 -.md` (empty)
   - `wiki/2026-04-12.md` (empty)
   - `wiki/2026-04-27.md` (empty)
   - `wiki/Untitled.canvas` (empty)
   - `wiki/Untitled 1.canvas` (empty)
   - `wiki/Untitled 2.canvas` (empty)
   - `wiki/Untitled.base` (empty)

3. **Moved guides to `wiki/guides/`:**
   - `1 分钟读完这份 wiki.md`
   - `5 分钟读完 Claude Code Prompt 设计.md`
   - `Claude Code Prompt 设计总览.excalidraw`

4. **Removed Copilot plugin notes:**
   - `wiki/copilot/` (16 files: 15 custom prompts + 1 conversation)

5. **Updated metadata files:**
   - `CLAUDE.md` — directory structure updated to reflect `code/`, `guides/`, `design/`, `hermes/`, `code-analysis/`
   - `wiki/index.md` — guide paths updated; copilot/workspace section removed; total pages 174 → 156
   - `wiki/log.md` — this entry

6. **Cleaned `.DS_Store` files** throughout the repository.

### Updated Count

- `wiki/index.md` total page count updated from 174 → 156.

### Schema Compliance

Repository now has exactly 3 top-level items (`CLAUDE.md`, `raw/`, `wiki/`) plus the preserved `code/` reference directory — matching the Layer 1 / Layer 2 / Layer 3 schema defined in `CLAUDE.md`.

# Agentic Coding Wiki

A Claude Code Skill providing a comprehensive, source-backed knowledge base comparing three agentic coding products.

## Products Covered

| Product | Creator | Architecture |
|---------|---------|-------------|
| **Claude Code** | Anthropic (Boris Cherny) | Terminal-first agentic coding tool |
| **Codex** | OpenAI | Open-source Rust-based coding agent |
| **OpenClaw / ClawBot** | Peter Steinberger | Self-modifying personal coding assistant |

## 9 Analytical Dimensions

1. **Agent Loop** — Core architecture, streaming, tool-driven loops
2. **Tool Systems** — Tool design, MCP, skills, code editing
3. **Security & Sandboxing** — Permission models, trust boundaries
4. **Context Management** — System prompts, CLAUDE.md, compaction, memory
5. **Multi-Agent Architecture** — Sub-agents, orchestration, swarm patterns
6. **UI & Distribution** — CLI, desktop, web, mobile, IDE extensions
7. **Feature Gating & Internals** — Feature flags, open source vs proprietary
8. **Shared Principles** — Common patterns across all three products
9. **Philosophical Divergence** — "Trust the model" vs "Trust but verify" vs "Agentic engineering"

## Knowledge Base Stats

- **105** structured source summaries
- **88** raw blog posts and interview transcripts
- **9** comparative dimension analyses
- **14** entity reference cards
- **11** implementation deep-dives
- **5** Hermes Agent case study pages

## Installation

### As a project skill (team-wide)

```bash
# Add as a git submodule to your project
git submodule add <repo-url> .claude/skills/agentic-coding-wiki
```

### As a personal skill (global)

```bash
# Clone to your personal Claude Code skills directory
git clone <repo-url> ~/.claude/skills/agentic-coding-wiki
```

## Usage

Once installed, Claude will automatically activate this skill when you ask about:

- Agentic coding product architecture or features
- Comparisons between Claude Code, Codex, and OpenClaw
- Agent loop design, tool systems, or context management patterns
- Hermes Agent implementation details
- Prompt engineering for coding agents

### Example Queries

```
> How does Claude Code's agent loop differ from Codex?
> What are the security models across the three products?
> Explain OpenClaw's self-evolution mechanism
> What did Boris Cherny say about tool design philosophy?
```

## Contributing

This wiki is a living knowledge base. To contribute:

1. Add new source material to `sources/`
2. Update relevant dimension analyses in `references/dimensions/`
3. Keep entity cards current in `references/entities/`
4. Submit a PR with your changes

## Structure

```
agentic-coding-wiki/
├── SKILL.md                    ← Skill manifest (Claude reads this first)
├── references/                 ← Curated analysis (loaded on demand)
│   ├── overview.md
│   ├── dimensions/             ← 9 comparative dimensions
│   ├── entities/               ← Product & people reference cards
│   ├── comparisons/            ← Head-to-head analyses
│   ├── hermes/                 ← Hermes Agent case study
│   ├── code-analysis/          ← Implementation deep-dives
│   └── guides/                 ← Chinese reading guides
├── sources/                    ← Evidence layer
│   ├── *.md                    ← 105 structured source summaries
│   ├── blogs/                  ← Raw blog posts
│   ├── interviews/             ← Raw transcripts
│   └── summaries/              ← Pre-existing summaries
└── README.md                   ← This file
```

---

*Built with ❤️ as a team knowledge-sharing initiative.*
*Schema version: 1.0 — April 2026*

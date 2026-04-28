---
tags: [source, blog]
product: Codex
date: 2026-01-23
dimensions: [01, 04]

author: Michael Bolin
---

# Blog — Codex — Unrolling the Agent Loop

**Source:** `raw/blogs/codex/openai-news__2026-01-23__unrolling-the-codex-agent-loop.md`
**Product:** [Codex](references/entities/codex.md)
**Author:** [Michael Bolin](references/entities/michael-bolin.md), Member of the Technical Staff
**Published:** January 23, 2026
**URL:** https://openai.com/index/unrolling-the-codex-agent-loop/

## Summary

The definitive technical deep-dive into the Codex agent loop. First in a planned series. Covers prompt construction, SSE streaming, tool call handling, context management, and prompt caching optimization.

## Key Technical Details

### Agent Loop Flow
```
User input → Build prompt → HTTP POST to Responses API → 
  SSE stream → Parse events → Execute tool calls → 
  Append results to input → Re-query model → Loop until assistant message
```

### Prompt Construction (4 layers)
1. **`instructions`** — Model-specific instructions (e.g., `gpt-5.2-codex_prompt.md`) or user-configured `model_instructions_file`
2. **`tools`** — Shell + update_plan + web_search + MCP tools
3. **`input[0]`** (role=developer) — Sandbox permissions from templates (`workspace_write.md`, `on_request.md`)
4. **`input[1]`** (role=developer, optional) — `developer_instructions` from user config
5. **`input[2]`** (role=user, optional) — Aggregated user instructions:
   - `AGENTS.override.md` / `AGENTS.md` from `$CODEX_HOME`
   - `AGENTS.md` from Git root → cwd (max 32 KiB)
   - Skills metadata + usage section
6. **`input[3]`** (role=user) — Environment context (`<cwd>`, `<shell>`)
7. **`input[4]`** (role=user) — User's actual message

### SSE Event Processing
Events consumed from Responses API stream:
- `response.reasoning_summary_text.delta/done` — Reasoning trace
- `response.output_item.added/done` — New items (reasoning, tool calls)
- `response.output_text.delta/done` — Assistant text output
- `response.completed` — Turn complete

### Prompt Caching Strategy
**Critical optimization** — prompt is designed so old prompt is exact prefix of new prompt:
- Config changes mid-conversation → **append** new message (don't edit earlier ones)
- Sandbox/approval mode changes → new `role=developer` message
- CWD changes → new `role=user` `<environment_context>`
- Cache-miss risks: changing tools, changing model, MCP tool reordering (bug found and fixed)

### Compaction
- Original: manual `/compact` command → LLM summarizes → summary replaces input
- Current: Responses API `/responses/compact` endpoint → returns `type=compaction` item with `encrypted_content`
- Auto-compact triggered when `auto_compact_limit` exceeded

### ZDR (Zero Data Retention) Support
- No `previous_response_id` used — requests are fully stateless
- Encrypted reasoning (`encrypted_content`) can still be decrypted server-side
- Compatible with enterprise ZDR requirements

## Key Quotes

> "Because the agent can execute tool calls that modify the local environment, its 'output' is not limited to the assistant message."

> "Prompt caching is so important, as it enables us to reuse computation from a previous inference call. When we get cache hits, sampling the model is linear rather than quadratic."

> "Context window management is one of the agent's many responsibilities."

## Relevant Dimensions

- [01 - Agent Loop](references/dimensions/01-agent-loop.md) — The authoritative reference for Codex's agent loop implementation
- [04 - Context Management](references/dimensions/04-context-management.md) — Prompt construction, compaction, caching

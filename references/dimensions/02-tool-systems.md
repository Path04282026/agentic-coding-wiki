---
tags: [dimension]
dimension: 02
title: Tool Systems
products: [Claude Code, Codex, OpenClaw]
source_count: 17
---

# Tool Systems

## Overview

Tools are how agents interact with the world — reading files, executing commands, editing code, browsing the web, and spawning sub-agents. The design of the tool system reveals each product's philosophy about model autonomy, safety boundaries, and extensibility.

The three products occupy distinct points on the spectrum: Claude Code provides 40+ specialized tools with rich type annotations; Codex offers ~10 core tools with MCP extensibility and an open-source harness; OpenClaw treats the entire system as a tool, with the agent capable of modifying its own tooling via "bespoke prompts."

## Claude Code

Claude Code provides the richest tool set of the three products, with 40+ specialized tools organized by function.

**Tool categories:**
- **File operations**: Read, Edit, Write, Glob, Grep, NotebookEdit — each with clear boundaries
- **Shell execution**: Bash, PowerShell, REPL (internal)
- **Code modification**: Edit (exact string find-and-replace with `old_string` → `new_string`)
- **Web**: WebFetch, WebSearch, WebBrowser
- **Agent spawning**: AgentTool (typed sub-agents: general-purpose, Explore, Plan, claude-code-guide)
- **Task management**: TaskCreate, TaskGet, TaskList, TaskUpdate
- **Planning**: EnterPlanMode, ExitPlanMode
- **Git**: EnterWorktree, ExitWorktree
- **MCP**: ListMcpResources, ReadMcpResource, MCPTool
- **Scheduling**: CronCreate, CronDelete, RemoteTrigger
- **Skills**: WorkflowTool, SkillTool

**Tool type system** (`Tool.ts`):
```typescript
interface Tool {
  name: string
  description: string
  inputSchema: JSONSchema
  execute(input, context): Promise<ToolResult>
  isReadOnly?: boolean
  isConcurrencySafe?: boolean  // Enables automatic parallelization
}
```

The `isConcurrencySafe` annotation is architecturally significant — it enables Claude Code to automatically run read-only tools in parallel without user intervention, a feature unique among the three products.

**Code editing approach**: The Edit tool uses exact string replacement. The model must provide the precise text to find (`old_string`) and its replacement (`new_string`). Fails if the search string isn't unique. This is more conservative than diff-based approaches but less error-prone for the model.

**Skills system**: Claude Code has a layered skill system — organization-level skills, project-level skills, and dynamically loaded skills via the Skills Directory. Skills are essentially structured prompts that configure the agent for specialized tasks.

**Tooling as capability elicitation**: Cat Wu describes the PM challenge as helping users interact with model strengths and patch current model weaknesses. She also warns that skills, MCPs, and workflow customization are useful only when they support the actual product or feature being built.

- Source citations: [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md), [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md), [blog--claude-code--skills](sources/blog-claude-code-skills.md), [blog--claude-code--equipping-agents-for-the-real-world-with-agent-skills](sources/blog-claude-code-equipping-agents-for-the-real-world-with-agent-skills.md), [blog--claude-code--complete-guide-to-building-skills-for-claude](sources/blog-claude-code-complete-guide-to-building-skills-for-claude.md), [blog--claude-code--claude-code-plugins](sources/blog-claude-code-claude-code-plugins.md)

## Codex

Codex takes a minimalist approach — fewer, broader tools that rely on MCP for extensibility.

**Core tools (~10):**
- **Shell**: The primary tool — file operations, builds, tests all run through shell commands
- **apply_patch**: Code editing via unified diff format
- **Agent**: Sub-agent spawning
- **MCP tools**: Arbitrary extensions via Model Context Protocol
- **JS REPL**: JavaScript execution for data processing

**Code editing approach**: The `apply_patch` tool generates unified diffs. More flexible for large changes (can express additions, deletions, and moves in a single operation) but arguably harder for the model to generate correctly compared to exact string replacement.

**MCP as the extension layer**: Rather than building specialized tools for every use case, Codex provides a full MCP client/server implementation (`codex-mcp` crate). Users connect external services — Figma, databases, custom APIs — through MCP rather than requesting new built-in tools.

**Open-source harness**: Because the Rust harness is Apache-2.0 licensed, power users fork and extend the tool set directly. The team observes users enabling unreleased features from the codebase before they're officially shipped.

> "Whenever we're building a feature, we start getting complaints on Twitter that the feature — which by the way is not enabled in prod — is like broken because people are going in and changing the code themselves." — [Alex (Codex PM)](references/entities/alex-(codex-pm).md)

**Skills crate**: Codex has a dedicated skills system implemented as a Rust crate, enabling configurable tool behaviors and workflow automation.

- Source citations: [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md), [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md), [blog--codex--openai-developers-blog__2026-03-09__using-skills-to-accelerate-oss-maintenance](sources/blog-codex-openai-developers-blog-2026-03-09-using-skills-to-accelerate-oss-maintenance.md), [blog--codex--openai-developers-blog__2026-01-22__testing-agent-skills-systematically-with-evals](sources/blog-codex-openai-developers-blog-2026-01-22-testing-agent-skills-systematically-with-evals.md), [blog--codex--openai-developers-blog__2026-02-26__building-frontend-uis-with-codex-and-figma](sources/blog-codex-openai-developers-blog-2026-02-26-building-frontend-uis-with-codex-and-figma.md)

## OpenClaw

OpenClaw's tool system is the most fluid of the three — the agent itself can create, modify, and extend its own tools through self-modification.

**Core approach:**
- **System-as-tool**: The agent has access to its entire runtime environment. It knows its source code, its configuration, which model it's running, and where documentation lives.
- **Voice-driven tool use**: Peter uses voice input extensively — "bespoke prompts" spoken rather than typed. The agent accepts multi-modal inputs (text, images, voice memos) through messaging gateways.
- **Self-modifying tools**: The agent can modify its own tool implementations. When a user requests a capability that doesn't exist, the agent writes the code to add it.
- **Plugin architecture**: OpenClaw has a plugin system where functionality is organized into modular, independently loadable plugins.

**The audio message incident** — a canonical example of emergent tool use:
Peter sent an audio message via WhatsApp (not a supported feature). The agent examined the file header, identified the format as Opus audio, used ffmpeg to convert it, found the OpenAI API key, used curl to send it to Whisper for transcription, and replied with the text. All without any pre-built audio handling.

> "I literally wrote 'How the f*** do you do that?' And it was like, 'Yeah, the method did the following…' It checked the file header, used ffmpeg, found the OpenAI key, and used curl to Whisper." — [Peter Steinberger](references/entities/peter-steinberger.md)

**Self-introspection for debugging**: Peter regularly uses the agent to debug itself — "Hey, what tools do you see? Can you call the tool yourself? Read the source code. Figure out what's the problem."

**Model and tool orchestration**: Luo Fuli emphasizes OpenClaw's ability to route around model limitations by orchestrating tools and models. Instead of requiring the user to manually attach a video model, for example, the framework can locate the right capability inside the workflow.

- Source citations: [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md), [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md), [interview--openclaw--peter-steinberger-2026-02-25](sources/interview-openclaw-peter-steinberger-2026-02-25.md), [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)

## Comparison Table

| Aspect | Claude Code | Codex | OpenClaw |
|---|---|---|---|
| **Total tools** | 40+ specialized | ~10 core + MCP | System-level access + self-modifying |
| **Code editing** | Find-and-replace (`Edit`) | Unified diff (`apply_patch`) | Via underlying coding agent |
| **Extension mechanism** | Plugins + Skills + MCP | MCP + open-source forking | Self-modification + plugins |
| **Tool typing** | Rich (JSONSchema, concurrency flags) | Rust structs via router | Fluid, configuration-driven |
| **Parallelization** | Automatic via `isConcurrencySafe` | Manual/user-directed | Manual (multiple terminals) |
| **Input modality** | Text (CLI/IDE) | Text (CLI/IDE/App) | Text + voice + images |
| **Web access** | WebFetch, WebSearch, WebBrowser | Via MCP tools | Via underlying agent tools |
| **Philosophy** | "More tools = more model autonomy" | "Fewer tools, model composes primitives" | "The system is the tool" |

## Key Quotes

> "We try so many different tools and just statistically most of them we throw away." — [Boris Cherny](references/entities/boris-cherny.md)

> "Agentic search just outperformed everything… and when I say agentic search, this is a fancy word for glob and grep. That's all it is." — [Boris Cherny](references/entities/boris-cherny.md)

> "I just use bespoke prompts to build my software. These hands are too precious for writing now." — [Peter Steinberger](references/entities/peter-steinberger.md)

> "The tooling, everything else matters — can make such a big difference." — [Greg Brockman](references/entities/greg-brockman.md)

## Sources

- [interview--claude-code--boris-cherny-2026-02-17](sources/interview-claude-code-boris-cherny-2026-02-17.md)
- [interview--claude-code--boris-cherny-2026-03-04](sources/interview-claude-code-boris-cherny-2026-03-04.md)
- [interview--claude-code--cat-wu-2026-04-23](sources/interview-claude-code-cat-wu-2026-04-23.md)
- [interview--codex--greg-brockman-tibo-such-2025-09-16](sources/interview-codex-greg-brockman-tibo-such-2025-09-16.md)
- [interview--codex--michael-bolin-2026-03-09](sources/interview-codex-michael-bolin-2026-03-09.md)
- [interview--openclaw--peter-steinberger-2026-01-29](sources/interview-openclaw-peter-steinberger-2026-01-29.md)
- [interview--openclaw--peter-steinberger-2026-02-12](sources/interview-openclaw-peter-steinberger-2026-02-12.md)
- [interview--openclaw--luo-fuli-2026-04-23](sources/interview-openclaw-luo-fuli-2026-04-23.md)
- [blog--claude-code--skills](sources/blog-claude-code-skills.md)
- [blog--claude-code--equipping-agents-for-the-real-world-with-agent-skills](sources/blog-claude-code-equipping-agents-for-the-real-world-with-agent-skills.md)
- [blog--claude-code--claude-code-plugins](sources/blog-claude-code-claude-code-plugins.md)
- [blog--claude-code--what-is-model-context-protocol](sources/blog-claude-code-what-is-model-context-protocol.md)
- [blog--codex--openai-developers-blog__2026-03-09__using-skills-to-accelerate-oss-maintenance](sources/blog-codex-openai-developers-blog-2026-03-09-using-skills-to-accelerate-oss-maintenance.md)
- [blog--codex--openai-developers-blog__2026-01-22__testing-agent-skills-systematically-with-evals](sources/blog-codex-openai-developers-blog-2026-01-22-testing-agent-skills-systematically-with-evals.md)
- [blog--codex--openai-developers-blog__2026-02-26__building-frontend-uis-with-codex-and-figma](sources/blog-codex-openai-developers-blog-2026-02-26-building-frontend-uis-with-codex-and-figma.md)

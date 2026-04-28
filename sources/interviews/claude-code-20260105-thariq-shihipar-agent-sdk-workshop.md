# Claude Agent SDK [Full Workshop] — Thariq Shihipar, Anthropic

**Source:** https://www.youtube.com/watch?v=TqC1qOfiVcQ
**Speaker:** Thariq Shihipar, Anthropic
**Published:** January 5, 2026
**Format:** Full technical workshop / masterclass (~1 hour)
**Related Docs:** https://platform.claude.com/docs/en/agent-sdk/overview

## Overview

A hands-on workshop by Thariq Shihipar (Anthropic) demonstrating how to build autonomous AI agents using the Claude Agent SDK (formerly Claude Code SDK). The workshop moves from high-level agent theory to live-coding demonstrations, covering the agent loop, shell-tool integration, and handling persistent state across multi-turn, long-running tasks.

## Core Concepts Covered

### Agents vs. Features vs. Workflows

Shihipar defines a progression in AI engineering:
- **LLM Features:** Simple categorization or single-turn API calls.
- **Workflows:** Structured, multi-step chains (like RAG or sequential API calls).
- **Agents:** Autonomous systems that manage their own context, decide their own trajectories, and operate persistently over time.

### The "Harness" Architecture

The workshop demonstrates building an agent "Harness" from scratch. Key components:
- **The Agent Loop:** Context (gathering info) → Thought (reasoning) → Action (tool execution) → Observation (evaluating the outcome)
- **Tooling — "Bash is all you need":** The SDK encourages using Unix primitives (Bash, shell commands, file systems) as the primary interface, which is more composable and powerful than building many rigid, custom tools.
- **Context Engineering:** Agents use the file system as "external memory." Skills are collections of files that an agent can read, modify, or execute as needed — a "file-system-pilled" approach.

### Progressive Disclosure

To prevent context rot, agents use structured disclosure (e.g., `SKILL.md` vs. `CLAUDE.md`) to pull in only necessary context at specific stages of a task.

### Safety — The "Swiss Cheese" Defense

Multi-layered security model:
1. **Model Alignment:** Internal Anthropic alignment efforts.
2. **Harness Permissioning:** Defining read-only vs. read-write zones.
3. **Sandboxing:** Running agents in isolated environments (containers, Cloudflare sandboxes) to contain actions.

### Hooks & Refactoring

A pattern for "deterministic overrides" — developers inject feedback or constraints (e.g., "always read before writing") without retraining the model.

## SDK Technical Details

### Installation
```bash
# TypeScript
npm install @anthropic-ai/claude-agent-sdk

# Python
pip install claude-agent-sdk
```

### Core API — `query()` Function
```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    async for message in query(
        prompt="Find and fix the bug in auth.py",
        options=ClaudeAgentOptions(allowed_tools=["Read", "Edit", "Bash"]),
    ):
        print(message)

asyncio.run(main())
```

### Key Capabilities
- **Built-in tools:** Read, Write, Edit, Bash, Glob, Grep, etc.
- **Hooks:** PreToolUse, PostToolUse, Stop, SessionStart, SessionEnd, UserPromptSubmit
- **Subagents:** Define specialized agents (e.g., code-reviewer) with scoped tool access
- **MCP integration:** Connect to external tools (Playwright, etc.)
- **Sessions:** Resume conversations with full context
- **Permissions:** Fine-grained tool access control
- **Skills:** Loads from `.claude/skills/*/SKILL.md`
- **Slash commands:** From `.claude/commands/*.md`
- **Memory:** Via `CLAUDE.md` files

### Agent SDK vs Client SDK
- **Client SDK:** Developer implements the tool loop manually
- **Agent SDK:** Claude handles tools autonomously — you just stream messages

### Deployment Options
- Direct API (Anthropic)
- Amazon Bedrock (`CLAUDE_CODE_USE_BEDROCK=1`)
- Google Vertex AI (`CLAUDE_CODE_USE_VERTEX=1`)
- Microsoft Azure (`CLAUDE_CODE_USE_FOUNDRY=1`)

## Strategic Takeaways

- **Why the SDK Exists:** Anthropic was rebuilding the same agent infrastructure (context management, tool execution, state handling) internally for every project. The SDK packages this production-tested infrastructure for public use.
- **Best Use Cases:** Agents are most effective for tasks where the output is **verifiable**. If you can verify the agent's work, it is a strong candidate for autonomous development.
- **Developer Mindset:** Building agents is not just about LLM capabilities — it's about infrastructure design. Teams should be "opinionated" about their stack and prioritize shipping functional agents that can be improved over time.

## Notable Quotes

> "Bash is all you need." — Thariq Shihipar

> "If you can verify the agent's work, it is a strong candidate for autonomous development." — Thariq Shihipar

> "Building agents is not just about LLM capabilities; it is about infrastructure design." — Thariq Shihipar

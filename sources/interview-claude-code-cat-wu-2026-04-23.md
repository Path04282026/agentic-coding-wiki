---
tags: [source, interview]
product: Claude Code
date: 2026-04-23
dimensions: [01, 02, 04, 05, 06, 09]
speaker: Cat Wu

---

# Interview - Claude Code - Cat Wu (2026-04-23)

**Format:** Podcast interview on Lenny's Podcast
**Speaker:** [Cat Wu](references/entities/cat-wu.md) - Head of Product for [Claude Code](references/entities/claude-code.md) and Co-work at [Anthropic](references/entities/anthropic.md)
**Raw transcript:** `raw/interviews/Claude_Code_20260423_Cat_Wu.md`

## Key Takeaways by Dimension

### [01 - Agent Loop](references/dimensions/01-agent-loop.md)
- Cat frames the product challenge as eliciting the maximum capability from the current model, not designing for a hypothetical super-AGI model.
- The agent loop must include verification: an agent should fully verify work before the human accepts completion.
- Claude Code's evolution is described as a shift from assistant to full-on agent, with product work focused on enabling that transition.

### [02 - Tool Systems](references/dimensions/02-tool-systems.md)
- Tooling should guide users onto the model's "golden path" and patch current model weaknesses.
- Managed agents are positioned as a developer-platform capability: users build agents and Anthropic hosts them.
- Skills, MCPs, connected tools, and workflow customizations are useful, but over-customizing can distract from the actual product or feature being built.

### [04 - Context Management](references/dimensions/04-context-management.md)
- Co-work quality depends heavily on connected context: Calendar, Slack, Gmail, Google Drive, design templates, and source-of-truth docs.
- Cat uses model introspection as a harness-debugging technique: ask the model why it skipped verification or made an unexpected choice, then adjust prompts, tools, or flow.
- Reusable automations need durable feedback loops: users should teach preferences into skills until the workflow becomes reliable.

### [05 - Multi-Agent Architecture](references/dimensions/05-multi-agent-architecture.md)
- The near-term scaling path is from one successful task to many simultaneous tasks, then to dozens or hundreds of remote Claude instances.
- Future UI needs to help humans manage many remote agents, identify tasks needing attention, and trust completion claims.
- Code review is an example of model-improvement unlocking true multi-agent work: multiple review agents can traverse the codebase and synthesize actionable issues.

### [06 - UI and Distribution](references/dimensions/06-ui-and-distribution.md)
- Cat distinguishes Claude Code surfaces by job: CLI for powerful one-off coding tasks, desktop for visual/front-end work and task control, web/mobile for kicking off work remotely, and Co-work for non-code outputs.
- `/powerup` exists because the fast-growing feature surface needs built-in education, even though the original principle was "no onboarding flow."
- Co-work extends Claude's action-taking model into non-code PM work: decks, launch plans, docs, inbox/Slack workflows, and customer materials.

### [09 - Philosophical Divergence](references/dimensions/09-philosophical-divergence.md)
- Cat's core PM principle is that as code gets cheaper, deciding what to build becomes more valuable.
- Products should be built for the current model frontier: neither underestimating model progress nor pretending the model is already omnipotent.
- Newer models remove scaffolding: prompts and product crutches should be revisited and deleted when the model no longer needs them.
- The important shift from 2024-style chat products to Claude Code-style products is action: the agent does the work, not just advises the user.

## Notable Quotes

> "As code becomes much cheaper to write, the thing that becomes more valuable is deciding what to write."

> "The hard thing is figuring out for the current model. How do you elicit the maximum capability?"

> "A lot of the changes that we make with a new model is removing features that are no longer needed."

> "The 2024 generation of products were chat-based and the Claude Code generation of products is action-based."

> "The agent can actually just do it itself."

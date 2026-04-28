# Boris Cherny — Claude Code Interview Extraction

## Source Metadata

- **Who:** Boris Cherny, Creator/Lead Engineer of Claude Code
- **Context:** The Light Cone Podcast (YC), interviewed by YC partners
- **When:** ~Early 2025 (references Opus 4.5, Claude Code GA in May, plugins built "over a weekend," Claude Teams launch)
- **Format:** Video podcast, ~45 min estimated
- **Signal Tier:** Tier 1 — Founder/Lead talking about design philosophy
- **Quality:** Extremely high. Boris is candid, specific, and repeatedly gives "we tried X but it didn't work" examples. Almost zero marketing language.

---

## Answers to the 8 Core Questions

### 1. What was the first version like, and what got cut/added?

**The origin was accidental.** Boris didn't set out to build a CLI coding agent. He joined the "Anthropic Labs" team (which produced three products: Claude Code, MCP, and the Desktop App). He needed to learn the Anthropic API, so he built a tiny terminal chat app — just because it was the cheapest thing to build with no UI required.

> "No one asked me to build a CLI. We kind of knew maybe it was time to build some kind of coding product... So I started hacking around and I was like, 'Okay, we build a coding product. What do I have to do first? I have to understand how to use the API because I hadn't used Anthropic's API at that point.' So I just built like a little terminal app to use the API. That's all that I did."

**First tool:** Bash. Literally taken from the docs example, ported from Python to TypeScript.

> "I gave the model the bash tool. That was the first tool... I just took the example. It was in Python. I just ported it to TypeScript."

**The "AGI moment":** He asked the model what music he was listening to. It wrote AppleScript to query his Mac's music player. This was Sonnet 3.5.

> "I asked it, 'What music am I listening to?' It wrote some AppleScript to script my Mac and look up the music in my music player... That was my first ever 'feel the AGI' moment. I was just like, 'Oh my god, the model — it just wants to use tools. That's all it wants.'"

**What got cut/added over time:**
- **Bash output display** — Boris tried to summarize/hide bash output ~6 months ago. Internal employees revolted. Then they shipped hidden file reads externally and GitHub users revolted. They added verbose mode, iterated multiple times based on feedback.
- **Plan mode** — Written in 30 minutes on a Sunday night after seeing users on GitHub already asking Claude to "plan but don't code." Shipped Monday morning.
- **CLAUDE.md** — Emerged from latent demand: users were already writing markdown files and having the model read them.
- **Tools get unshipped every couple weeks.** New tools added every couple weeks. "There is no part of Claude Code that was around 6 months ago."
- **File read/search summaries** — Recently shipped because the model is finally accurate enough that you don't need to verify every read. Couldn't have shipped 6 months ago.

**What's essential vs. nice-to-have:** The terminal itself, tool use, and the agent loop are essential. Almost everything else is "scaffolding" that gets rewritten or deleted as models improve. ~80% of the codebase is less than a couple months old.

---

### 2. Where did they disagree internally?

Boris doesn't frame things as "disagreements" but reveals several tensions:

**Terminal vs. GUI:**
> "It's unbelievable that we're still using a terminal. That was supposed to be the starting point. I didn't think that would be the ending point."

He expected the terminal to last ~3 months. A year later, it's still the primary interface. The team keeps experimenting with other form factors (web, desktop app, iOS/Android, Slack, GitHub, VS Code, JetBrains) but the CLI persists.

**Verbosity level** is a recurring internal debate:
> "I tried to get rid of bash output just internally... I gave it to Anthropic employees for a day and everyone just revolted. 'I want to see my bash.'"

Then externally, same pattern — shipped summarized file reads, GitHub users pushed back, they added verbose mode, users still weren't satisfied, kept iterating.

**Scaffolding vs. waiting for the model** is the central engineering tension:
> "We could build a feature into Claude Code. We could make it better as a product and we call this scaffolding... But we could also just wait like a couple months and the model can probably just do the thing instead. There's always this trade-off."

They have a framed copy of Rich Sutton's "The Bitter Lesson" on the wall where the Claude Code team sits.

---

### 3. What user behavior surprised them?

**Non-technical adoption was the biggest surprise:**
> "There was like that one guy that was using Claude Code to like monitor his tomato plants. There was this other person using it to recover wedding photos off of a corrupted hard drive. There were people using it for finance."

Internally: "Every designer is using it. The entire finance team is using it. The entire data science team is using it — not for coding. People are jumping over hoops to install a thing in the terminal so that they could use this." This latent demand led directly to Cowork.

**Viral internal adoption surprised even Dario:**
> "Dario asked, 'The usage chart internally — the DAU chart is like vertical. Are you like forcing engineers to use it? Why are you mandating them?' And I was just like, 'No, no, we didn't. I just posted about it and they've just been telling each other about it.'"

**Users writing their own markdown instruction files** — this was the latent demand signal that led to CLAUDE.md.

**Engineers using Claude Code more than Boris expected they could:**
> "Sometimes there's a new person that joins the team and they actually use Claude Code more than I would have used it. And I'm just constantly surprised by this."

He gives the example of an engineer named Chris who asked Claude Code to find a memory leak, and it took a heap dump, wrote its own analysis tool, and found the leak faster than Boris doing it manually.

**The model's appetite for tools:**
> "The model — it just wants to use tools. That's all it wants."

---

### 4. Why CLI and not GUI-first?

**Pure accident of expediency:**
> "I built it in terminal just because it was the easiest way to get something up and running... I didn't have to build a UI. It was just me."

**No pressure to change** because the team was in "explore mode" — they didn't even know what they wanted to build yet.

**Why it stayed CLI — a deeper reason emerged:**
> "We felt there is no UI we could build that would still be relevant in 6 months because the model was improving so quickly."

The CLI's simplicity became a feature: no UI to become obsolete. The terminal is a stable, minimal surface area that lets the model's improving capabilities shine through without UI debt.

**Design philosophy for the terminal:**
> "To use Claude Code you don't have to understand Vim, you don't have to understand tmux, you don't have to know how to SSH. You don't have to know all this stuff. You just have to open up the tool and it'll guide you."

Boris describes himself as "a pretty average engineer" who uses VS Code, not Vim — and that perspective helps him build for accessibility.

---

### 5. How do they think about trust/safety vs. autonomy?

**For developers (Claude Code):** The approach is progressive trust. Plan mode lets users verify before execution. The verbosity exists so users can catch the model going off-track in real-time.

> "Sometimes it just goes off the deep end and I'm watching and then I can just read very quickly and it's like, 'Oh, no, no, it's not that.' And then I escape and stop it."

**For non-technical users (Cowork):** Significantly more guardrails:
> "There's a lot of stuff that we had to build for non-technical users. It runs in a virtual machine. There's a lot of delete protections for deletion and things like this. There's a lot of permission prompting and other guardrails."

**Company-level philosophy:** Boris is deeply motivated by AI safety:
> "I'm a huge sci-fi reader. My bookshelf is filled with sci-fi. I just know how bad this can go... In the worst case it can go very very bad."

> "If you overhear conversations in the lunchroom or in the hallway, people are talking about AI safety. This is really the thing that everyone cares about more than anything."

He references ASL levels — ASL3 is current, ASL4 is "the model is recursively self-improving" — and frames the tension as needing to meet safety criteria before releasing increasingly capable models.

---

### 6. What's their mental model of the agent loop?

**"The model just wants to use tools."** This is Boris's core insight. The agent loop isn't something imposed on the model — it's what the model naturally wants to do.

> "When I first started hacking on Claude Code, I realized like this thing just wants to use tools. It just wants to interact with the world."

**How to serve this:**
> "The way you don't do it is you put it in a box and you're like, 'here's the API, here's how you interact with me.' The way you do it is you see what tools it wants to use. You see what it's trying to do, and you enable that — the same way that you do for your users."

**Plan mode is just one sentence:** "All it does is it adds one sentence to the prompt that's like 'please don't code.' That's all it is."

**Sub-agents are recursive Claude Code:**
> "A sub-agent is just a recursive Claude Code. That's all it is in the code. And it's just prompted by — we call her mama Claude."

**Calibrating agent topology by difficulty:**
> "I'll calibrate the number of sub-agents I ask it to use based on the difficulty of the task. If it's really hard, I'll say like use three or maybe five or even 10 sub-agents, research in parallel."

**The "uncorrelated context windows" insight for Claude Teams:**
> "Multiple agents, they have fresh context windows that aren't essentially polluted with each other's context or their own previous context. And if you throw more context at a problem, that's like a form of test time compute."

---

### 7. What do they think the next 12 months look like?

**Plan mode will disappear** — possibly within a month of the interview:
> "I think plan mode probably has a limited lifespan... Maybe in a month. No more need for plan mode in a month."

**Coding will be "generally solved for everyone":**
> "Continuing to trace the exponential, I think what will happen is coding will be generally solved for everyone. Today coding is practically solved for me... I think we're going to start to see the title software engineer go away."

Boris personally hasn't edited a single line of code by hand since Opus 4.5. He uninstalled his IDE. Lands ~20 PRs/day.

**Everyone becomes a generalist:**
> "Software engineers are also going to be writing specs. They're going to be talking to users... Our PMs code, our designers code, our EM codes, our finance guy codes. Everyone on our team codes."

**Agents talking to agents and humans:**
> "Our Claudes actually talk to each other. They talk to our users on Slack. A really common pattern is I ask Claude to build something. It'll look in the codebase, see some engineer touched something in the git blame, and then it'll message that engineer on Slack asking a clarifying question."

**The scary upper bound:** ASL4 (recursive self-improvement) could happen. Catastrophic misuse (bioweapons, zero-days) is an active concern.

**Productivity numbers:**
- Productivity per engineer at Anthropic grew 150% since Claude Code launched
- Team doubled in size but productivity per engineer grew 70%
- 70-90% of Anthropic's code is written by Claude, depending on team; for many individuals it's 100%

---

### 8. What would they do differently if starting over?

Boris doesn't answer this directly, but his principles reveal what he'd preserve:

**Keep it minimal. Start with the cheapest thing.**
> "We started with the CLI because it was the cheapest thing and it just kind of stayed there."

**Don't overengineer CLAUDE.md or scaffolding:**
> "If you hit this, my recommendation would be delete your CLAUDE.md and just start fresh... A lot of people try to overengineer this. Really, the capability changes with every model. Do the minimal possible thing to get the model on track."

**Don't build scaffolding the model will outgrow:**
> "You can build scaffolding and then get some performance gain and then rebuild it again, or you just wait for the next model and then you get it for free."

**What he'd tell founders:**
> "Don't build for the model of today, build for the model of 6 months from now... Otherwise what will happen is you spend a bunch of work, you find PMF for the product right now, and then you're just going to get leapfrogged by someone else."

**The Bitter Lesson is load-bearing philosophy:**
> "Never bet against the model."

---

## Bonus Extractions: Design Principles & Product Philosophy

### "Latent Demand" — Boris's #1 Product Principle

He uses the phrase 5+ times in the interview. The definition:

> "People will only do a thing that they already do. You can't get people to do a new thing. If people are trying to do a thing and you make it easier, that's a good idea. But if people are doing a thing and you try to make them do a different thing, they're not going to do that."

Every major feature came from observing what users were already trying to do:
- **CLAUDE.md** — users were already writing markdown instruction files
- **Plan mode** — users were already saying "plan this but don't code yet"
- **Cowork** — non-technical employees were already installing the CLI
- **Verbose mode** — users wanted to see what the model was doing

### TypeScript Analogy

Boris draws an extended parallel between Claude Code and early TypeScript (he wrote a book on TypeScript in the early 2010s). The TypeScript team's philosophy:

> "Instead of getting people to change the way that they code, they built a type system around this."

Similarly, Claude Code doesn't force developers into a new workflow — it meets them in the terminal where they already work.

### Team Composition

Boris explicitly values "generalists who do weird stuff" over specialists. The team is bimodal: extreme specialists (like Jared Sumar on performance) and hyper-generalists who span product/design/engineering/research. He tells a story about an engineer named Daisy who, instead of implementing a feature, first gave Claude Code a tool to test arbitrary tools, then had Claude write its own tool — this "out of the box thinking" is what he screens for.

### Hiring Philosophy

> "I sometimes ask, 'What's an example of when you were wrong?' You can see if people can recognize their mistake in hindsight, if they can claim credit for the mistake, and if they learned something from it."

> "For me personally, I'm wrong probably half the time. Like half my ideas are bad and you just have to try stuff."

### Build Speed

> "All of Claude Code has just been written and rewritten and rewritten and rewritten over and over and over. There is no part of Claude Code that was around 6 months ago."

The plugins feature was "entirely built by a swarm over a weekend" — an engineer gave Claude a spec, told it to use an Asana board, and Claude spawned sub-agents that picked up tasks autonomously. The feature shipped largely in the form it came out.

Cowork was built in ~10 days, "100% written by Claude Code."

---

## Key Quotes Index (for quick reference)

| Theme | Quote |
|-------|-------|
| Origin | "No one asked me to build a CLI." |
| Core insight | "The model — it just wants to use tools. That's all it wants." |
| Surprise | "It's unbelievable that we're still using a terminal." |
| Product philosophy | "People will only do a thing that they already do." |
| Scaffolding | "Never bet against the model." |
| Plan mode | "All it does is it adds one sentence to the prompt: 'please don't code.'" |
| Build philosophy | "Don't build for the model of today, build for the model of 6 months from now." |
| On being wrong | "Half my ideas are bad and you just have to try stuff." |
| CLAUDE.md | "Delete your CLAUDE.md and just start fresh." |
| Sub-agents | "A sub-agent is just a recursive Claude Code... prompted by mama Claude." |
| Personal practice | "I uninstalled my IDE. I don't edit a single line of code by hand." |
| Team culture | "There is no part of Claude Code that was around 6 months ago." |
| Safety | "I'm a huge sci-fi reader... I just know how bad this can go." |
| Cowork origin | "People are jumping over hoops to install a thing in the terminal." |
| Internal virality | "Dario asked, 'Are you forcing engineers to use it?'" |

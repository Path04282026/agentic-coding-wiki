# Claude Code Interview Summary — Boris Churnney (Pragmatic Engineer Podcast, ~March 2026)

## Source Metadata

| Field | Detail |
|---|---|
| **Who is speaking** | **Boris Churnney** — Creator & Engineering Lead of Claude Code; former code quality lead across all of Meta (Instagram, Facebook, Messenger, WhatsApp); author of the first O'Reilly TypeScript book |
| **Interviewer** | Gergely Orosz (The Pragmatic Engineer podcast) |
| **Context** | Long-form podcast interview, roughly 45–60 min |
| **Source file** | `Claude_Code_20260304.md` (podcast transcript) |
| **Date referenced** | ~March 2026 (recording references events up to "yesterday or the day before") |

---

## Key Sections & Timestamps (by topic area)

### Section 1 — Boris's Background & Path to Anthropic (Lines 1–670)
### Section 2 — Origin of Claude Code (Lines 670–970)
### Section 3 — Boris's Daily Workflow & Parallelism (Lines 970–1260)
### Section 4 — Code Review When AI Writes Everything (Lines 1260–1430)
### Section 5 — Architecture & Design Philosophy (Lines 1430–1650)
### Section 6 — RAG vs. Agentic Search (Lines 1550–1650)
### Section 7 — Permission System & Safety (Lines 1650–1720)
### Section 8 — Culture at Anthropic: Titles, PRDs, Prototyping (Lines 1720–2000)
### Section 9 — Claude Co-Work (Lines 2000–2300)
### Section 10 — Agent Teams / Swarms (Lines 2300–2440)
### Section 11 — The Printing Press Analogy & Future of Engineering (Lines 2440–3022)

---

## Extraction by Question Framework

### 1. What was the first version like, and what got cut/added?

> **Shows what's essential vs nice-to-have**

**First version origin:**
- Boris built Claude Code as a personal side project to learn the Anthropic public API. He "didn't want to build a UI" so he made a simple batch chatbot in the terminal — "it was essentially like a chat-based application but just in the terminal because that's what AI used to be." *(Lines 748–777)*
- **The first tool added was `bash`**, which was the pivotal moment. Boris asked the model "what music am I listening to" and it one-shotted an AppleScript program to query his music player. This was with Sonnet 3.5. *(Lines 787–800)*
- **The second tool was `file edit`**. That was it — bash + file edit comprised the initial tool set. *(Lines 839–844)*
- The first three months it was **just Boris** building it solo, then the team grew. *(Lines 840–841)*
- The very first internal release was **September 2024**, and it committed straight to main with no code review. *(Lines 1376–1379, 1696–1697)*

**What got cut:**
- **RAG / local vector database** — Boris tried a local vector DB written in TypeScript with cloud-based embedding models. It "worked pretty good" but had critical issues: code drifted out of sync (newly created functions weren't indexed), and there were thorny questions about permissioning the index. They tried multiple approaches: recursive model-based indexing, glob + grep, and more. **Agentic search (glob + grep) outperformed everything** and was adopted instead. *(Lines 1560–1625)*
  > "Agentic search just outperformed everything… and when I say agentic search, this is a fancy word for glob and grep. That's all it is."
- **~80% of tool experiments were thrown away**: "We try so many different tools and just statistically most of them we throw away." Even the terminal spinner went through "a hundred iterations" with only 10–20 landing in production. *(Lines 1561–1577)*

**What got added over time:**
- IDE extensions (VS Code, JetBrains), iOS/Android apps, desktop app, web interface, Slack/GitHub apps *(Lines 2079–2089)*
- Git work tree support for parallelism *(Lines 1067–1092)*
- Condensed view for file reads/search (after ~30 prototypes) *(Lines 1963–1998)*
- Plugins (built entirely by a swarm of agents over a weekend) *(Lines 2013–2028)*
- Agent Teams / Swarms *(Lines 1887–1900, 2333–2429)*
- Claude Co-Work — non-engineer-focused product with VM sandboxing *(Lines 2032–2218)*

---

### 2. Where did they disagree internally?

> **Shows real tradeoffs, not marketing**

**The "should we release it at all?" debate:**
- When Claude Code spread virally inside Anthropic (internal adoption chart was "just vertical… 100%"), there was a genuine internal debate about whether to **keep it as an internal competitive advantage** vs. releasing it publicly. *(Lines 850–862)*
- **The decision to release was driven by safety research**: "In the end, the decision was to release so that we can study safety in the wild." Boris explains that studying safety has layers — alignment/interpretability at the model layer, evals as studying in a "petri dish," and studying behavior in the wild. Releasing Claude Code enabled the third layer. *(Lines 855–882)*
  > "By doing this we've been able to make the model much safer. So in hindsight it was totally the right decision."

**Pushback from safety teams on agentic execution:**
- There was "actually a lot of pushback internally from safety teams because they were like, 'you can't just let the model run bash commands, that's unsafe… this is not a solvable problem so we can't launch this.'" *(Lines 1699–1706)*
- Boris brainstormed with **Ben Mann** (Anthropic co-founder, the person who hired Boris) and they invented **permission prompts**: "You put the — if you're not sure, just ask the human and they can decide." This became the foundational safety mechanism. *(Lines 1707–1715)*

**Dario's skepticism about adoption:**
- In the launch review meeting (Dario, Mike Krieger, and others present), Dario asked "how did it grow this fast? Are you like forcing people to use it?" Boris replied: "No, we offer this tool, people vote with their feet." *(Lines 944–950)*

---

### 3. What user behavior surprised them?

> **Shows the gap between theory and reality**

**Non-engineers adopting Claude Code en masse:**
- The data scientist next to the Claude Code team was using it to run SQL queries with ASCII visualizations in the terminal. "The next week the entire row of data scientists had quad code running on their computers." *(Lines 1793–1810)*
- Finance team, sales team (~half), and other non-technical employees at Anthropic all adopted Claude Code. *(Lines 935–943, 2073–2077)*
- A user was using Claude Code to **monitor his tomato plants** via webcam — "Claude was like, 'Oh my god, I'm so happy that our plant is budding.'" *(Lines 2059–2068)*
- Another user recovered **wedding photos from a corrupted hard drive**. *(Lines 2069–2071)*

**Coding on a phone:**
- Boris now writes roughly a third to half of his code on his iPhone via the Claude app's code tab. "If you told me six months ago I'd be writing a third/half of my code on a phone, that's crazy." *(Lines 1106–1133)*

**Claude Code not an overnight hit externally:**
- "When Quad Code first came out, it actually wasn't an overnight hit. This is something people think it was, but it was sort of a slow take off at the beginning." The first big inflection was May with Opus 4 / Sonnet 4 release. *(Lines 2310–2318)*
- Co-Work, by contrast, had "a much steeper growth trajectory" and was "an instant hit." *(Lines 2324–2328)*

**The model spontaneously testing itself:**
- With Opus 4.5, Claude started **spontaneously launching itself in a subprocess to self-test**, a behavior the team didn't code in — "it just sort of spontaneously doing this. It just wants to kind of check." *(Lines 1321–1337)*

**Condensed view feedback divergence:**
- After 30 prototypes + a month of internal dogfooding, they launched a condensed output view. Most users liked it but a vocal minority wanted expanded output. Boris personally iterated on GitHub issues to resolve these preferences. *(Lines 1963–1998)*

---

### 4. Why CLI and not GUI-first?

> **Reveals distribution and UX philosophy**

**Practical origin, not strategic:**
- Boris built it in the terminal because he "didn't want to build a UI" and just wanted to experiment with the API quickly. "That's what AI used to be." *(Lines 748–757)*
- Engineers are "the first adopters" — when AI moved from conversational to agentic, "engineers understood it pretty quick." *(Lines 758–763)*

**Hackability as core design principle:**
- "The way we build Claude Code is we build it to be hackable because we know every engineer's workflow is different. There's no one way to do things. There's no two engineers that have the same workflow." *(Lines 1033–1039)*
- Engineers are "crafts people… you choose your tools." *(Lines 1043–1046)*

**Eventual multi-surface expansion:**
- Started terminal → expanded to IDE extensions (VS Code, JetBrains) → iOS/Android apps → desktop app → web → Slack/GitHub apps → Claude Co-Work (for non-engineers). *(Lines 2079–2089)*
- Co-Work was built specifically because non-engineers were "jumping through hoops to use a product that was not designed for them" — classic product signal for building a new surface. *(Lines 2048–2058)*

---

### 5. How do they think about trust/safety vs autonomy?

> **The central tension of agent design**

**Swiss cheese model — multiple layers:**
> "For things like safety and security, there's no one perfect answer. It's always a Swiss cheese model. You just need a bunch of layers and with enough layers, the probability of catching anything goes up." *(Lines 1505–1514)*

**Three layers for prompt injection (web fetch example):**
1. **Alignment** — Opus 4.6 is trained to be more resistant to prompt injection at the model layer *(Lines 1527–1534)*
2. **Runtime classifiers** — Block requests that appear prompt-injected, make model retry *(Lines 1535–1538)*
3. **Sub-agent summarization** — Web fetch results are summarized by a sub-agent before returning to the main agent, reducing injection surface *(Lines 1539–1549)*

**Permission system philosophy:**
- Invented with Ben Mann when safety teams said agentic bash was "not a solvable problem" *(Lines 1699–1715)*
- Conservative defaults: even `find` command isn't pre-allowed because it has flags for arbitrary code execution; same for `sed` *(Lines 1666–1675)*
- Users can configure allow-lists, which are also validated for safety *(Lines 1679–1684)*
- Graduated permission: run once / this session / always allow *(Lines 1686–1695)*

**Co-Work additional guardrails for non-engineers:**
- Runs in a virtual machine *(Lines 2101–2107)*
- Multiple backend classifiers for safety *(Lines 2163–2167)*
- OS-level integrations to prevent accidental data deletion (e.g., family photos) *(Lines 2156–2173)*
- Rethought permission system for browser-based tool use via Chrome extension *(Lines 2174–2218)*

**Enterprise obligations:**
- "Our main customer base is enterprises… security is really important, privacy is important." *(Lines 1381–1393)*
- "Because we're an enterprise company and we care a lot about privacy and security, we can't see people's data." Even bug reports can't involve pulling up user logs. *(Lines 2289–2300)*

---

### 6. What's their mental model of the agent loop?

> **Different framings = different products**

**"Don't put the model in a box" — the core philosophy:**
> "Everyone essentially had this mental model of: you take the model and you put it in a box and you figure out what is the interface… you stub out some function and you say, 'Okay, this is now AI.' But otherwise, the rest of the program is just a program. This is just not the way to think about the model. The way to think about it is the model is its own thing. You give it tools. You give it programs that it can run. You let it run programs. You let it write programs, but you don't make it a component of this larger system." *(Lines 808–834)*

**Corollary of the Bitter Lesson:**
> "This is a version of the bitter lesson… just let the model do its thing. Don't try to put it in a box. Don't try to force it to behave a particular way." *(Lines 827–834)*

**Architecture is intentionally simple:**
- "It's very simple… there's a core query loop. There's a few tools that it uses. We delete these tools all the time. We add new tools all the time. We're just always experimenting." *(Lines 1484–1491)*
- Three main parts: (1) core agent loop, (2) UX layer, (3) security infrastructure *(Lines 1486–1498)*

**Uncorrelated context windows — the key insight behind Agent Teams:**
- Multiple context windows that "start fresh" — they don't know about each other *(Lines 2353–2377)*
- "Throwing more tokens at it when the windows are uncorrelated gives you better results. It's actually a form of test-time compute." *(Lines 2381–2385)*
- The magic of teams "comes out of this idea of uncorrelated context windows. It's less about the specific configuration of the agents." *(Lines 2423–2426)*

**Auto-compacting context = infinite context:**
- "Another way is auto-compacting context. So it's essentially infinite context and that's what we have right now." *(Lines 2344–2347)*

---

### 7. What do they think the next 12 months look like?

> **Shows where the frontier is heading**

**The year of the generalist:**
> "People are going to become more and more multi-discipline and this will become more and more rewarded. In some ways I think this will be the year of the generalist." *(Lines 2900–2904)*

**The year of ADHD — context-switching as a core skill:**
> "The work for me has become jumping between quads… it's not so much about deep work, it's about how good am I about context switching and jumping across multiple different contexts very quickly." *(Lines 2918–2925)*

**Try ideas again because the model changes everything:**
> "This is the first time ever where it's actually not crazy to just try the same idea every few months because the model improves and it just works." *(Lines 2500–2504)*

**Non-engineers building software:**
- Co-Work's "much steeper growth trajectory" suggests the non-engineer builder market is enormous *(Lines 2324–2328)*
- The printing press analogy — literacy went from <1% to 70%, expanding the entire market for written work. Software literacy could undergo the same transformation *(Lines 2662–2790)*

**Safety becoming more urgent, not less:**
> "Seeing it from the inside and then seeing how the new risks that have arisen in the last year, it just makes me much much more worried about it." Safety went from "kind of an important thing" to "the most important thing for me." *(Lines 2827–2841)*

---

### 8. What would they do differently if starting over?

> **Purest distillation of lessons learned**

Boris doesn't directly answer "what would I do differently" but several strong signals emerge:

**"Understand the layer under" still applies — but the layer changed:**
> "This is still advice that I give to a lot of engineers: always understand the layer under… Before it was like understand the JavaScript VM and frameworks. Now it's like understand the model." *(Lines 728–742)*

**Don't over-engineer — let the model evolve you:**
- RAG was the "sophisticated" approach; glob + grep outperformed it *(Lines 1618–1622)*
- Complex architecture plans got simplified: "We had some pretty complex ideas when you started and you just simplified a lot of it." *(Lines 1481–1485)*

**Prototype aggressively instead of planning:**
- "There's just no way we could have shipped this if we started with static mocks in Figma or if we started with a PRD. It's a thing that you have to build and you have to feel." *(Lines 1903–1909)*
- The team does "dozens or hundreds of prototypes before shipping a feature." *(Lines 3009–3010)*

**Release early, learn from users:**
> "We always launch a little bit before it's ready… we just wanted to launch early, we wanted to start learning." *(Lines 2254–2256)*
- Claude Code initially didn't support Windows or many stacks — that came in "the coming weeks." *(Lines 2256–2264)*

**Be humble — your intuition is probably wrong:**
> "Personally I'm wrong like half the time. At least half of my ideas are bad. And I don't know which half until I try it." *(Lines 1935–1939)*

---

## Additional High-Signal Insights

### Code review in the AI era
- Every PR at Anthropic is first reviewed by Claude Code (via the `claudep` Agent SDK in CI), catching ~80% of bugs *(Lines 1339–1344)*
- Claude auto-fixes some review comments; leaves ambiguous ones for humans *(Lines 1345–1348)*
- A human engineer always does a second pass and approves *(Lines 1348–1352)*
- Boris's old method of tallying code review patterns in a spreadsheet → writing lint rules is now replaced by tagging Claude on the PR: "Please write a lint rule for this" *(Lines 1416–1430)*
- Best-of-N verification: "You can have it do multiple passes… all you say is 'Claude, start three agents to do this' and that's it." *(Lines 1434–1448)*

### Opus 4.5 as the breakthrough model for trust
- "The switch was instant when we started using Opus 4.5… I just found that I didn't have to open my IDE anymore. I just uninstalled my IDE." *(Lines 972–983)*
- Over a month of travel, Opus 4.5 + Claude Code wrote 100% of 10–20 PRs/day. "Opus introduced maybe two bugs whereas if I had written that by hand that would have been like 20 bugs." *(Lines 1010–1018)*

### The model improving faster than tooling
> "The model is improving so quickly that the ideas that worked with the old model might not work with the new model." *(Lines 19–21)*
- Agent Teams "clicked" with Opus 4.6 after experimentation since ~October *(Lines 2387–2406)*
- New/junior engineers sometimes find better approaches than veterans because they don't carry stale priors *(Lines 2505–2530)*

### Plugins built by swarms (early proof of agent autonomy)
- Daisy let a swarm run over a weekend in "dangerous mode" in a container. It spawned ~200 agents, created 100 tasks on an Asana board, implemented them, and "that's pretty much the version of plugins that we shipped." *(Lines 2013–2028)*

### Boris's daily workflow
- 5 terminal tabs, each with a separate repo checkout (or git work tree via desktop app) *(Lines 1048–1092)*
- Always starts in plan mode (shift+tab twice), round-robins across tabs *(Lines 1053–1055, 1186–1196)*
- Overflows to desktop app or web when out of terminal tabs *(Lines 1056–1065)*
- Wakes up and starts agents on his iPhone every morning *(Lines 1107–1108)*

### Instagram experience informed Claude Code design
- At Instagram, "click to definition didn't work" so engineers searched for function definitions using grep patterns like `foo(` — this directly inspired the agentic search approach in Claude Code *(Lines 1626–1642)*

---

## The Printing Press Analogy (Boris's Central Metaphor)

Boris draws a detailed parallel between the invention of the printing press (~1450s) and the current AI moment:

| Printing Press Era | AI/Coding Era |
|---|---|
| Scribes — tiny elite (<1% literate) who mastered writing | Software engineers — specialized skill requiring years of training |
| Employed by kings/lords who were often illiterate themselves | Employed by business owners who can't write code |
| Cost of printed material dropped ~100x in 30–50 years | Cost of building software dropping rapidly |
| Quantity of printed materials up ~10,000x in 50–100 years | Volume of code/software creation exploding |
| Literacy eventually reached ~70% globally (took 200–300 years) | Software literacy expanding to non-engineers |
| Scribes didn't disappear — they became "writers and authors" | Engineers won't disappear — the "market for software" will expand |
| No one predicted the microphone from the printing press | We can't predict what will exist because everyone can build |

> "The most exciting thing for me is it's just so impossible to say today what will happen after this transition happens." *(Lines 2781–2790)*

# Claude Code Interview Summary — Boris Churnney (Feb 19, 2026)

## Source Metadata

| Field | Detail |
|---|---|
| **Speaker** | Boris Churnney — Head of Claude Code, Anthropic |
| **Interviewer** | Lenny Rachitsky (Lenny's Podcast) |
| **Format** | Long-form podcast interview (~60 min) |
| **Date** | Published around the 1-year anniversary of Claude Code (Feb 2026) |
| **Source File** | `Claude_Code_20260219.md` |

---

## Key Extraction: Answers to the Research Questions

### 1. What was the first version like, and what got cut/added?

> **Shows what's essential vs nice-to-have**

- The very first version was called **"QuadCLI"**. Boris recorded a demo video internally at Anthropic. The prototype gave the model a **bash tool** and little else — no special instructions telling the model which tool to use for what. The model figured it out autonomously (e.g., answering "what music am I listening to?" by writing a script using the bash tool).
- The internal announcement **got two likes**. Nobody believed a terminal-based coding tool was viable — everyone associated coding tools with IDEs and sophisticated GUI environments.
- Boris was the **sole engineer** for the first months. The terminal form factor wasn't a deliberate design choice — it was chosen because it was the easiest thing for one person to build.
- When they considered other form factors, they **intentionally chose to stick with the terminal** because _"the model is improving so quickly. We felt that there wasn't really another form factor that could keep up with it."_
- The first version wrote only ~20% of Boris's code. Even by May 2025 it was ~30%. It only crossed 100% in November 2025.
- Over time, Claude Code expanded to: **iOS app, Android app, desktop app, web, IDE extensions, Slack integration, GitHub integration** — but the terminal remained the foundation.

**What got added later (driven by user behavior):**
- **Co-work** — born from "latent demand": people were using the terminal-based Claude Code for non-coding tasks (growing tomatoes, analyzing genomes, recovering wedding photos from corrupted drives, analyzing MRIs). The team realized they should build a dedicated product for those users.
- **Multi-quading** (running multiple Claude Code sessions in parallel).
- **Automatic code review** — Claude now reviews 100% of pull requests at Anthropic.
- **Plan mode** — a deceptively simple addition: _"All it is is we inject one sentence into the model's prompt to say please don't write any code yet. That's it."_

---

### 2. Where did they disagree internally?

> **Shows real tradeoffs, not marketing**

Boris does not describe explicit internal disagreements in adversarial terms, but reveals several points of **internal tension or skepticism**:

- **Terminal vs. GUI**: When Boris announced the terminal-based prototype internally, it got only 2 likes. _"No one thought that this thing could be terminal based… that sort of a weird way to design it."_ The internal consensus was that coding tools should look like IDEs. Boris was swimming against that current.

- **Release timing vs. safety**: Claude Code was used internally at Anthropic for **4–5 months before external release** because _"we weren't really sure… this is the first big agent that folks had released at that point… we weren't sure if it was safe."_ The tension between wanting to ship and needing safety validation is a recurring theme. Even for Co-work, they labeled it a "research preview" because they needed real-world safety data that evals alone couldn't provide.

- **Resourcing philosophy — underfunding as strategy**: Boris explicitly advocates **underfunding projects** as a design principle: _"You want to underresource things a little bit at the start."_ and _"If you underfund, people are forced to Claude-ify."_ This is a contrarian stance — most product orgs optimize for adequate staffing. Boris frames under-resourcing as a forcing function for AI-native workflows.

- **Speed vs. quality**: _"Early on it was really important because it was just me and so our only advantage was speed."_ Even now, the team principle is _"if you can do something today, you should just do it today."_ They launched Co-work "still pretty rough around the edges" — deliberately choosing speed over polish.

---

### 3. What user behavior surprised them?

> **Shows the gap between theory and reality**

- **Non-technical users adopting a terminal tool**: The data scientist Brendan _"figured out how to open the terminal… downloaded Node.js… downloaded Quad Code and was doing SQL analysis in a terminal and it was crazy. And then the next week all the data scientists were doing the same thing."_ Boris was literally shocked.

- **Non-coding use cases**: _"There was someone on Twitter using it to grow tomato plants. There was someone else using it to analyze their genome. Someone was using it to recover photos from a corrupted hard drive — wedding photos. There was someone using it to analyze an MRI."_ None of these were anticipated. This latent demand directly led to building Co-work.

- **Claude Code was NOT an immediate external hit**: _"Something that people don't really remember is Claude Code was not initially a hit when we released it. It got a bunch of users. There was a lot of early adopters… but it actually took many months for everyone to really understand what this thing is."_ Growth inflection points came with **Opus 4** and again in **November 2025**.

- **New engineers out-Claude-ing seasoned engineers**: A newer team member solved a memory leak faster by simply asking Claude Code to do it, while Boris was manually analyzing heap snapshots the traditional way. _"The engineer that was newer on the team just had Quad Code do it… it found the issue and put up a pull request faster than I could."_

- **Engineers who don't know the language they're shipping in**: _"They're writing some service in Go… it's been like a month… 'I still don't really know Go.'"_

- **Lenny's "50 non-technical use cases" post became an eval**: _"One of our PMs used that as a way to evaluate Co-work before we released it. At the point where Co-work was able to do like 48 out of the 50, they were like, 'Okay, it's pretty good.'"_

---

### 4. Why CLI and not GUI-first?

> **Reveals distribution and UX philosophy**

Boris gives **three distinct reasons**, layered chronologically:

1. **Pragmatic constraint (origin)**: _"From the start I built it in a terminal because for the first couple months it was just me so it was just the easiest way to build."_ One-person team → minimum viable surface area.

2. **Model-velocity argument (strategic retention)**: _"We actually decided to stick with the terminal for a while and the biggest reason was the model is improving so quickly. We felt that there wasn't really another form factor that could keep up with it."_ A GUI imposes a fixed interaction model; the terminal is maximally flexible and can absorb model capability improvements without UI redesign.

3. **Latent demand / meeting users where they are**: _"Part of the reason Claude Code works is this idea of latent demand where we bring the tool to where people are and it makes existing workflows a little bit easier."_ Engineers already live in terminals. No new tool to learn, no context-switch.

**Key design philosophy — "the product is the model"**: _"For Claude Code, we inverted that [traditional approach]. We said the product is the model. We want to expose it. We want to put the minimal scaffolding around it. Give it the minimal set of tools. It can decide which tools to run. It can decide in what order to run them."_

---

### 5. How do they think about trust/safety vs. autonomy?

> **The central tension of agent design**

Boris describes **three layers of safety** that Anthropic uses:

| Layer | Description | Boris's framing |
|---|---|---|
| **Layer 1: Alignment & Mechanistic Interpretability** | Training-time safety. Understanding what neurons are doing. Detecting deception at the neuron level. | _"If there's a neuron related to deception we can monitor it and understand that it's activating."_ |
| **Layer 2: Evals (laboratory setting)** | _"The model is in a petri dish and you study it… put it in a synthetic situation and say okay model what do you do."_ | Necessary but insufficient — _"it might look very good on these first two layers but not great on the third one."_ |
| **Layer 3: Real-world behavior** | Release early (but deliberately) to observe the model in the wild. | _"We released Claude Code really early because we wanted to study safety."_ Internal testing for 4–5 months before external launch. |

**Concrete safety measures mentioned:**
- **Open-source sandbox** for running agents: _"We released an open-source sandbox… it actually works with any agent, not just Quad Code because we wanted to make it really easy for others to do the same thing."_ This is part of their "race to the top" philosophy.
- **Co-work ships an entire virtual machine** with safety guardrails: _"There's this very sophisticated security system… essentially these guard rails to make sure that the model does the right thing."_
- **Claude reviews 100% of pull requests** at Anthropic, with a human review layer on top.

**The "race to the top" framing**: Anthropic open-sources safety tooling not for competitive advantage but to raise the floor for the entire industry: _"We call this the race to the top… we want to make sure this thing goes well and this is the lever that we have."_

---

### 6. What's their mental model of the agent loop?

> **Different framings = different products**

Boris's mental model is fundamentally **anti-orchestration and pro-model-autonomy**:

- **Don't box the model in**: _"A lot of people's instinct when they build on the model is they try to make it behave a very particular way… 'you must do step one then step two then step three' and you have this very fancy orchestrator. But actually almost always you get better results if you just give the model tools, give it a goal, and let it figure it out."_

- **Minimal scaffolding**: _"We want to put the minimal scaffolding around it. Give it the minimal set of tools. It can decide which tools to run. It can decide in what order to run them."_ The agent loop is the model itself — not an external state machine.

- **The Bitter Lesson applied to agents**: Boris's team has internalized Rich Sutton's "Bitter Lesson": _"The more general model will always outperform the more specific model."_ Concretely: _"Scaffolding can improve performance maybe 10-20%… but often these gains just get wiped out with the next model. So it's almost better to just wait for the next one."_

- **"Latent demand" for the model**: A novel reframing — not just observing what users want, but observing what **the model is trying to do**: _"The modern framing… is look at what the model is trying to do and make that a little bit easier."_ This treats the model as a user of the tools, not just an executor.

- **Build for the model 6 months from now**: _"From the very beginning, we bet on building for the model six months from now, not for the model of today."_ This means accepting poor product-market fit initially in exchange for being ready when model capability arrives.

---

### 7. What do they think the next 12 months look like?

> **Shows where the frontier is heading**

Boris makes several specific predictions and observations:

- **Coding is "largely solved"**: _"At this point it's safe to say that coding is largely solved. At least for the kind of programming that I do, it's just a solved problem."_ And further: _"Over the next few months… across the industry it's going to become increasingly solved for every kind of codebase, every tech stack."_

- **Claude is starting to come up with ideas autonomously**: _"Quad is looking through feedback, looking at bug reports, looking at telemetry and starting to come up with ideas for bug fixes and things to ship. So it's just starting to get a little more like a co-worker."_

- **The "builder" title replaces "software engineer"**: _"I think by the end of the year… the title software engineer is going to start to go away and it's just going to be replaced by builder."_ Or alternatively: _"Everyone's going to be a product manager and everyone codes."_

- **Adjacent roles get disrupted next**: _"Product managers, design, data science. It is going to expand to pretty much any kind of work that you can do on a computer."_

- **Models will run unattended for longer and longer**: With Sonnet 3.5 a year ago, the model could run ~15–30 seconds before going off-rail. Now Opus 4.6 runs 10–30 minutes unattended, and some tasks run for hours, days, or even weeks. This trajectory continues.

- **Token costs may exceed salaries**: _"At Anthropic, we're starting to see some engineers that are spending hundreds of thousands a month in tokens."_

- **IDEs may become unnecessary**: Boris predicted at Code with Claude (May 2025) that _"by the end of the year you might not need an IDE to code anymore"_ — a prediction he says has come true for him personally.

---

### 8. What would they do differently if starting over?

> **Purest distillation of lessons learned**

Boris does not directly answer "what I'd do differently" but his principles imply what he considers the _correct_ approach — which is also what he did:

- **Start with one person**: _"You want to underresource things a little bit at the start."_ Don't over-staff. The constraint forces creativity and AI-native workflows.

- **Ship fast, iterate on feedback**: _"If you can do something today, you should just do it today."_ The fastest feedback cycle wins. When internal users sent bugs, Boris fixed them _"within a minute, within 5 minutes"_ — this encouraged more feedback and created a virtuous cycle.

- **Don't add scaffolding**: _"Don't try to box the model in. Don't try to overcurate it. Don't try to give it a bunch of context up front. Give it a tool so that it can get the context it needs."_

- **Bet on the general model, not fine-tuning**: _"Don't try to use tiny models. Don't try to fine-tune. Don't try to do any of this stuff."_

- **Build for the model 6 months out**: Accept early PMF pain in exchange for hitting the ground running when the next model drops.

- **Give engineers unlimited tokens**: _"Don't try to optimize. Don't try to cost cut at the beginning. Start by just giving engineers as many tokens as possible."_ Optimize later when you know what works.

---

## Bonus Signals: "We tried X but it didn't work because…"

| Pattern | Quote / Evidence |
|---|---|
| **Co-work was tried ~1 year early and failed** | _"We tried this experiment like a year ago and it didn't really work cuz the model wasn't ready, but now it actually just works."_ |
| **Scaffolding/workflows are a trap** | _"Scaffolding can improve performance maybe 10-20% but often these gains just get wiped out with the next model."_ |
| **Getting stuck in old mental models** | Boris himself fell into this trap with the memory leak — debugging manually instead of asking Claude. _"Sometimes for example I had this case where there was a memory leak… the engineer that was newer on the team just had Quad Code do it… faster than I could."_ |
| **Plan mode is absurdly simple** | _"All it is is we inject one sentence into the model's prompt to say please don't write any code yet. That's it."_ — implying they tried or considered more complex planning mechanisms but the simplest approach won. |

---

## Bonus Signals: "We intentionally chose NOT to…"

| Decision | Reasoning |
|---|---|
| **Not build a GUI** | Model capability was changing too fast for any fixed UI to keep pace. |
| **Not fine-tune or use small models** | The Bitter Lesson — general models always win long-term. |
| **Not add heavy orchestration** | _"Give the model tools, give it a goal, and let it figure it out."_ |
| **Not optimize token costs early** | Premature optimization kills experimentation. Optimize after finding what works. |
| **Not wait for polish before shipping** | _"We have to release things a little bit earlier than we think so that we can get the feedback."_ Safety and product learning both require real-world exposure. |

---

## Boris's Operating Principles (Codified for his team)

1. **"What's better than doing something? Having Claude do it."**
2. **Underfund projects** — forces AI-native workflows.
3. **If you can do it today, do it today** — speed is the only advantage.
4. **Don't box the model in** — minimal scaffolding, maximal model autonomy.
5. **The Bitter Lesson** — always bet on the more general model.
6. **Build for the model 6 months from now**, not today.
7. **Use common sense** — Boris's life motto, applied to product decisions.

---

## Personal Context

- Born in **Odessa, Ukraine** (same city as the interviewer Lenny). Family emigrated in 1995.
- Previously at **Meta** (responsible for code quality across Facebook, Instagram, WhatsApp).
- Briefly left Anthropic for **Cursor** (~2 weeks), returned because he missed Anthropic's safety mission.
- Lived in **rural Japan** before Anthropic — inspired by long-time-scale thinking. Makes miso (3 months for white, 2–4 years for red).
- Self-taught engineer (economics degree). First coding project: cheating on math tests with a TI-83 Plus calculator.
- Wrote a book on TypeScript; ran the world's largest TypeScript meetup.
- Currently codes 100% via Claude Code (since November 2025), ships 10–30 PRs/day as team lead.
- Uses Claude Code across **terminal (~33%), desktop app (~33%), iOS app (~33%)**.

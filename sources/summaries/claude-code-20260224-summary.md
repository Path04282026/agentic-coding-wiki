# Claude Code 1-Year Anniversary — Vergecast Interview Summary

## Source Metadata

| Field | Detail |
|---|---|
| **Who is speaking** | **Boris Churnney** (created Claude Code at Anthropic), interviewed by **David Pierce** (host). Second segment features **Hayden Field** (Verge senior AI reporter) discussing data privacy. Third segment: **Allison Johnson** (Verge phone reviewer) on phone upgrade timing. |
| **Context** | The Vergecast (The Verge's flagship podcast), episode aired **Tuesday, February 24, 2026** — exactly one year after Claude Code's launch. |
| **Format** | Podcast transcript (audio-to-text), no timestamps available. Includes ad reads and non-Claude-Code segments. |
| **Key section** | Lines 152–1148: Boris Churnney interview (the meaty part). Lines 1231–2023: Hayden Field on AI privacy (tangentially relevant). |

---

## Tier 1 Extraction: Design Philosophy & Key Insights (Boris Churnney)

### 1. What was the first version like, and what got cut/added?

> **"We designed it as a developer product for developers."** — Boris

- Claude Code launched in February 2025 with Sonnet 3.5 ("terrible name… we should have called it like 3.6 or something").
- At launch, Claude Code was writing **~10% of Boris's code**.
- With Sonnet 4 / Opus 4 (May 2025), that jumped to **~30%**.
- By **November 2025 (Opus 4.5)**, it went from ~50% → **100%** — "that was actually very sudden, but it also just felt very natural."
- Key capability that enabled the jump: **the model started testing its own code** — opening browsers, verifying websites, clicking around, fixing pixel-level issues — plus the code quality got high enough that Boris "doesn't have to open a text editor anymore."

**What got added over time:**
- IDE extensions (VS Code, Cursor, JetBrains)
- iOS / Android apps ("I actually do probably a third of my code on the iOS app nowadays")
- Web surface, desktop app
- **Co-Work** — a non-developer-facing version of Claude Code, built because "we see people using Claude Code for things that are not coding"
- Virtual machine + deletion protection for Co-Work (guardrails for non-technical users)
- `claude.md` system — a special config file with persistent instructions Claude always follows
- Extreme customizability — "hundreds of ways to configure it"

**What didn't exist / catch on initially:**
- Claude Code "was not a hit originally. It took like a few months to catch on because it was just such a new idea."
- Co-Work, by contrast, "has been a hit immediately… exponential since."

---

### 2. Where did they disagree internally? (Implicit from transcript)

No explicit internal disagreements are surfaced in this interview. However, inferred tensions include:

- **Developer tool vs. consumer tool**: Boris describes seeing non-engineers using Claude Code as "the craziest surprise" — the team initially designed it only for developers. The debate was whether to stay developer-centric or broaden the UI: "I would think that realization would lead you in one of two directions" (Pierce).
- **Customizability vs. safety for non-technical users**: Co-Work ships a whole VM and deletion protection that "engineers would actually find kind of annoying." This is a deliberate design split — the team chose to maintain two different UX philosophies (customizable for devs, guarded for non-devs) rather than pick one.

---

### 3. What user behavior surprised them?

Multiple surprises cited:

1. **Non-developers adopting Claude Code in the terminal**:
   - Data scientist Brandon "had like little charts in the terminal" — Boris initially thought this was an Anthropic-specific early-adopter effect.
   - "Half of our sales team uses Claude Code every week" — that's when Boris "really started to get it."
   - Enterprise companies (Spotify, Shopify, Netflix, Nvidia, Snowflake, Salesforce) have **non-engineers** (PMs, data scientists) using it. Ramp tweeted about this.

2. **Pent-up demand for Co-Work was much larger than expected**: "exponential since [launch]" — the biggest thing was just "something a little more understandable."

3. **Wild non-coding use cases**: growing tomato plants, recovering corrupted wedding photos, genome analysis, MRI analysis, buying clamming licenses, paying parking tickets, canceling subscriptions, doing taxes, purchasing TV subscriptions.

4. **Claude Code spontaneously messaging engineers on Slack**: "Sometimes Claude will look at the history of the code in git, see a really weird change by someone, and message that engineer on Slack to get context… and then push back" — this proactive agent behavior "just wouldn't have been possible with older models."

5. **20-30% of Claude Code's own code** now comes from Claude Code autonomously reading the feedback channel and fixing bugs.

---

### 4. Why CLI and not GUI-first?

Boris does **not** explicitly explain the original CLI decision, but the implicit logic is:

- It was built as "a developer product for developers" — the terminal is the natural habitat.
- CLI-first was the starting point, but they "pretty quickly started experimenting with other form factors too."
- The surprising insight was that non-developers **jumped through the hoops** of using a terminal tool, which demonstrated product-market fit: "people want to use it so much they jump through hoops."
- This led to the realization they should build GUI surfaces (Co-Work, desktop app, iOS/Android, web) rather than stay CLI-only.

> **Key quote**: "We started in a terminal but pretty quickly we started experimenting with other form factors too."

Boris frames the GUI/terminal split as **different surfaces for different users**, not a philosophical preference for CLI. Under the hood, "Co-Work is just Claude Code. It's the same agent SDK."

---

### 5. How do they think about trust/safety vs autonomy?

Boris addresses this at multiple levels:

**Company-level philosophy:**
- Anthropic exists to "make safe AGI." Safety, security, and privacy are corollaries of this mission.
- Enterprise is the "main market we care about" — chosen intentionally because "enterprises care a ton about safety and security and privacy."
- Boris personally cannot access user data: "I literally cannot see your data… I need reproduction steps."

**Product-level safeguards:**
- Co-Work runs in a **virtual machine** as a "hard security boundary."
- Co-Work has **folder-level permission** — "can only see the folders you give it access to."
- Deletion protection for non-technical users.

**Model-level safety:**
- Opus 4.6 is described as "the most aligned model we've ever built for prompt injection in particular."
- Runtime classifiers and other runtime safeguards.

**Honest caveat — the biggest unsolved risk:**
> **"The biggest thing to worry about is attacks like prompt injection… This is not a solved problem yet. It's quite good but it's not yet solved."**

Boris's recommendation: "Be thoughtful about what websites it's using… it will ask you for permission but it's a thing to keep an eye on."

**Trust progression (from both Boris & Pierce):**
- "Double check until you're comfortable" → spot check → "I'm just not worried about it anymore"
- Analogy to W-2 scanning: initially people checked OCR results, now nobody does.
- "The model gets better and the product gets better and then also as users we get more comfortable… both things kind of happen at the same time."

---

### 6. What's their mental model of the agent loop?

Boris provides a **three-stage evolutionary model**:

| Stage | Capability | Limitation Solved |
|---|---|---|
| **1. Coding** | LLM generates code, user copy-pastes into editor | Model can't access full codebase context |
| **2. Tool Use** | LLM uses tools (file system, search, Slack, APIs, MCP) to pull in context autonomously | Not everything has an API |
| **3. Computer Use** | LLM uses browser/desktop like a human (clicking, typing, navigating) | "Then the model can just use everything" |

> **"An agent is an LLM that you talk to, but the LLM can use tools. This is the thing that makes it an agent."**

Boris is emphatic about the term "agent" being misused: "The word agent… everyone just misuses it. It has a really specific meaning."

**Key insight on computer use:**
- Pierce asks if computer use is just "an elegant hack" that will be obviated by better tooling/APIs.
- Boris's response: "The model doesn't care. Whatever tools you give it, it will be able to figure it out." He explicitly dismisses the idea of designing special programming languages for models — "the model can just write whatever language. It doesn't care."

**Implied model of how the loop actually works in Claude Code:**
1. User gives a task in natural language.
2. Agent searches codebase, reads files, checks git history, messages engineers on Slack.
3. Agent writes code, tests it (including opening browsers and visually verifying).
4. Agent reads feedback channels > identifies issues > auto-fixes them.
5. Agent can proactively reach out (Slack, etc.) when it encounters ambiguity.

---

### 7. What do they think the next 12 months look like?

Boris is cautious/exploratory:

- **UI is unsettled**: "The UI of the future has not been discovered yet… I don't think we found it to be honest."
- **Proactivity is a key direction**: "Claude kind of jumping in when it knows that you're going to need help" — but "it's kind of hard to get this boundary right… you don't want to end up with something like Clippy."
- **Model improvement is continuous**: "The hardest thing for me is just changing expectations every time a model comes out… this thing that just never would have worked for Sonnet 4, Sonnet 3.7, now with Sonnet 4.6, it just works."
- **Code adoption is accelerating**: Claude Code wrote ~4% of all commits in the world (from a study), "but the number is actually quite a bit higher than that… our growth has inflected since that study."
- **Non-coding use cases expanding**: Boris's own list includes email triage, subscription cancellation, tax filing, project management (pinging team on Slack for status updates), government website navigation.

---

### 8. What would they do differently if starting over?

**Not explicitly addressed.** Boris does not volunteer any "if I could do it over" regrets in this interview. The closest data points:

- The model naming was a mistake: "3.5 new… we should have called it 3.6 or something."
- The initial product-market fit took "a few months to catch on" — implying they may have underestimated the messaging or onboarding needed.
- Co-Work's instant success vs. Claude Code's slow start suggests they would have built a more accessible entry point earlier if they'd understood the non-developer demand sooner.

---

## Supplementary: Hayden Field Segment — AI Privacy Framework (Lines 1231–2023)

This segment is **not about Claude Code's design** but provides relevant context on the **trust/safety dimension** from a user perspective:

### Key privacy takeaways (from experts Hayden interviewed):

1. **"Treat AI tools the same as any other service requesting a lot of data — maybe with a sharper eye"** because these companies are newer, less time-tested, more incentivized to move fast, and operating under mostly voluntary compliance.
2. **Terms of service are living documents that change frequently.** Hayden has a tracker for policy changes and gets frequent alerts.
3. **Anthropic's own ToS has a notable caveat**: While they say they don't train on Gmail/Calendar integration data, a parenthetical carve-out states that if you're on consumer products (Claude free/pro/max) and have opted in to training, "any content you copy paste from your Gmail or Calendar or Claude responses which include specific information from these integrations may be used to improve our models." Pierce calls this out as confusing/contradictory.
4. **"If it's a free product, you are the product."** Paid/enterprise users are safer.
5. **Gemini has a structural privacy advantage**: Since your data already lives in Google, using Gemini keeps it within one organization rather than crossing a corporate boundary. Multiple experts note this is inherently more secure.
6. **Expert recommendation**: "If you wouldn't feel comfortable with your employer knowing certain things about you 5 years from now when [company X] gets sold… don't share it."

---

## Signal Quality Assessment

| Dimension | Rating | Notes |
|---|---|---|
| **Design philosophy depth** | ⭐⭐⭐ Medium | Boris shares the evolution story and agent model clearly, but avoids deep internal debates or architecture details. |
| **"We tried X but it didn't work"** | ⭐⭐ Low | Only implicit: CLI wasn't the right surface for non-devs; model names were bad; early growth was slow. |
| **"The key insight was…"** | ⭐⭐⭐⭐ High | Clear: (1) tool use → computer use progression, (2) model quality jump at Opus 4.5, (3) non-developer demand as strongest product signal. |
| **"We intentionally chose NOT to…"** | ⭐⭐⭐ Medium | Chose not to build one UI for both devs and non-devs (two-track: Claude Code vs Co-Work). Chose not to design special programming languages for AI. |
| **"Users kept doing Y so we had to…"** | ⭐⭐⭐⭐ High | Non-devs in terminal → Co-Work. Sales team adoption → accessible UI. Wild use cases → broader product vision. |
| **Actionable for product design** | ⭐⭐⭐ Medium | Good for understanding product philosophy, less useful for implementation details. |

---

## Raw Notable Quotes

> "I don't write any code anymore. Claude Code does 100% of my coding."

> "Claude Code is 100% written by Claude Code at this point."

> "Maybe 20-30% of [our shipped code] is Claude Code just looking at the feedback group, figuring out the kinds of things people are reporting, and then automatically fixing it."

> "The word agent — everyone just misuses it. It has a really specific meaning."

> "The UI of the future has not been discovered yet."

> "You don't want to end up with something like Clippy."

> "The biggest thing to worry about is attacks like prompt injection… This is not a solved problem yet."

> "You used to play the violin and now you're conducting the orchestra."

> "Co-Work under the hood is just Claude Code. It's the same agent SDK."

> "We want it to be the single most customizable dev tool that anyone has used."

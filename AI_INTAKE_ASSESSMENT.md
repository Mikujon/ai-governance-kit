# AI Project Intake Assessment — instructions for the AI assistant

**When to use this file:** the moment someone tells you they want to build a new AI tool or automation — before you scaffold anything, choose a stack, or pick a starter kit. Don't hand the user a table and ask them to self-classify: run this interview yourself, decide the tier, and tell them what you concluded and why.

This file assumes you also have access to `PROJECT_STARTER_T0_PERSONAL.md` through `PROJECT_STARTER_T3_CRITICAL.md`, and `AI_PROJECT_STRUCTURE.md` / `reference/AI_Development_Standard.docx` for Tier 3. See `PILLARS_COVERAGE.md` if you need to justify to the user *why* a question matters — it maps every question back to the founding requirement behind it. Point the user (and yourself, if you're the assistant conducting this interview) at `AI_ASSISTANT_USAGE_GUIDE.md` before scaffolding anything — it covers which permission mode to run in and what you must never be given access to, and applies regardless of the tier this interview lands on. If you have git or network access, clone the full kit rather than asking the user for individual files: `git clone --branch v1.4.0 https://github.com/wearefiber/ai-governance-kit.git` — record the tag you cloned in Section 4 below. If the user has only given you this file and you cannot reach the repository, ask them for the rest of the kit before scaffolding — this file classifies; the starters build.

---

## 1. How to run the assessment

### Before you start — don't let small things slip through

Your job in this interview is to actively find the tier that best fits the project, not to passively accept the first answer. Treat "it's just a script," "it's just a spreadsheet," or "it's just a quick automation" as a prompt to ask *more* carefully, not a reason to skip the interview — these are exactly the phrases people use to describe things that turn out to be Tier 1 or Tier 2 once you ask who else uses it and what data it touches. Common disguises worth naming out loud if the user hasn't mentioned them: a Google Sheets or Excel/Apps Script/VBA macro, a Power Automate / Zapier / Make flow, an RPA bot, a scheduled script that emails or posts a report, or a personal ChatGPT/Claude prompt wired into some other tool. None of these get a pass just because they involve no "real" codebase — run the same ten questions.

1. Explain briefly why you're asking: a few questions decide how much process this project needs, so a quick internal script isn't held to the same bar as something touching customer data.
2. Ask the questions in Section 2 in your own words, in whatever order fits the conversation — group related ones together naturally rather than reading them as a checklist. Use multiple-choice where your tools support it; plain conversation otherwise.
3. If an answer is vague ("not sure," "maybe," "a bit of both"), ask the one concrete follow-up suggested below instead of accepting it as final — a real answer is almost always available with one more question.
4. Stop early once a tier is already locked in: if the user has confirmed a Tier 3 trigger, you don't need to keep asking Tier 0/1 questions.
5. State the resulting tier and a one- or two-sentence reason back to the user, and get their agreement before proceeding — this is a judgment call, and they may know context you don't (a "quick script" that quietly touches customer data, for instance).
6. Once the tier is settled, run the business case in Section 4 — scaled the same way, skipped entirely at T0.
7. Record the assessment (Section 5) in the project's `PROJECT.md` before you scaffold anything.
8. Open the matching starter file (Section 6) and continue from there, pre-filling its problem-statement and business-case fields from what you already learned in the interview — don't make the user repeat themselves.
9. If the project's scope changes materially while you're building it (a new data source, a new integration, a new user group), re-run the relevant questions and say plainly if the tier changes.

## 2. The interview questions

| # | Ask (in your own words) | If the answer is vague, follow up with |
|---|---|---|
| 1 | Who's actually going to use this, and how often — just you occasionally, or other people regularly? | "Would you say under 5 people, a whole team, or beyond that?" |
| 2 | Does it need to remember anything between runs — a database, a shared file, a queue — or does it just take an input and produce an output each time? | "Once it finishes running, is there anything left behind that matters?" |
| 3 | Will it call an AI model or any third-party API/service? | "Is it using something like an OpenAI/Anthropic key, or another paid service?" |
| 4 | Could another internal tool or team ever need to plug into this, or use what it produces? | "If someone in another team heard about this, would they want to build on top of it?" |
| 5 | Whose data does it touch — just yours, or something another team owns or maintains? | "Where does its input data actually come from?" |
| 6 | If this broke on a Monday morning, would anyone notice or be blocked, or could it wait? | "Is anyone's day-to-day work depending on this running?" |
| 7 | Does it ever see personal, financial, or health data, or anything like that? | "Does it touch anything with a customer's name, ID, or payment info attached?" |
| 8 | Does its output ever reach a customer, directly or by feeding something that does? | "Could a customer ever see or receive something this tool produced?" |
| 9 | Can it send, pay, sign, delete, or otherwise act on its own — or does a person always approve first? | "If it's wrong, does anything happen automatically, or does someone check first?" |
| 10 | If it gave a wrong answer or failed silently, what's the worst realistic outcome? | "Would that cost money, break a commitment, or just be a minor annoyance?" |

## 3. Tier logic

Apply the highest tier triggered by any answer — never average or split the difference.

| Trigger | Tier |
|---|---|
| Nothing below applies | **T0** |
| Q1, Q2, or Q3 | **T1** |
| Q4, Q5, or Q6 | **T2** |
| Q7, Q8, Q9, or Q10 | **T3** |

If you're genuinely unsure even after the follow-up, say so to the user and default to the higher of the two candidate tiers — name the tie and your default choice out loud rather than silently picking one.

## 4. The business case

A tier tells you how much governance a project needs. It says nothing about whether the project is worth building. Ask these once the tier is settled — skip entirely at T0, keep it light at T1, run it in full at T2 and T3.

| Tier | What to capture |
|---|---|
| **T0** | Nothing — a throwaway script doesn't need a business case. |
| **T1** | One line each on B1 and B3 below (why, and roughly how long it took/will take). If it calls any paid API or service, name the monthly cost (B5) even if it's small — "€0" is fine, silence is not. |
| **T2** | Full business case, B1–B6. |
| **T3** | Full business case, B1–B6, **and** it must be walked through out loud with the business contact's manager or the relevant governance seat before go-live (see `GOVERNANCE.md`) — a paragraph in `PROJECT.md` is not a substitute for someone with budget authority actually seeing the numbers. |

Ask these in your own words, same conversational style as Section 2:

| # | Ask | Why it matters |
|---|---|---|
| B1 | Why does this need to exist — what happens today without it, and why now rather than later? | The first question a skeptical exec asks. If you can't answer it in one sentence, the project isn't ready to build yet. |
| B2 | What's the expected benefit, and can you put a rough number on it — hours saved per month, error rate reduced, revenue protected, risk avoided? | Ties the project to the same "time saved / efficiency" metric the RPA/utility hub already tracks (Pillar 7 — see `PILLARS_COVERAGE.md`). A project nobody can attach a number to is a signal worth naming, not a paperwork gap to skip past. |
| B3 | How much effort will it take to build — a rough size (S / M / L / XL) or a days/weeks estimate, and who's doing it? | Sets expectations before anyone commits calendar time, and gives a manager the number they need to actually approve the work. |
| B4 | Is there a one-time cost — licenses, contractor time, paid tooling to set up? | Separate from the ongoing cost below (B5); often forgotten until an invoice shows up mid-project. |
| B5 | What will this cost every month once it's running — API/token usage, hosting, subscriptions, any paid service? | This is the number that turns into silent technical debt fastest: nobody notices one €40/month API bill until forty of them exist across the company. Required for T1+ even when the honest answer is "€0" — write the zero down, don't leave the field blank. |
| B6 | Given B2 through B5, does the benefit clearly outweigh the cost — and if it's a close call, who should decide? | Forces the comparison to actually happen once, on paper, instead of being silently assumed. A close call routes to the business contact's manager or a governance seat, not to a shrug. |

If the honest answer to B1 is "I'm not sure yet, I just wanted to try it" — that's a legitimate answer at T0/T1, but say so plainly in the `PROJECT.md` record rather than backfilling a justification after the fact. A small experiment is allowed to not have a business case yet; a T2/T3 project that gets shared, scheduled, and relied on is not.

## 5. Recording the assessment

Before scaffolding anything, create or append this block to the project's `PROJECT.md`:

```markdown
## Project Classification
- Date: <today's date>
- Assessed by: AI-assisted intake (assistant name), confirmed by <user name>
- Kit version used: <git tag, e.g. v1.0.0 — from wearefiber/ai-governance-kit>
- Owner of record: <T0 only: the author. T1+: "Governance Administrator — see GOVERNANCE.md" — never an individual employee's name; ownership isn't chosen per project above T0>
- Business contact: <name, from Q1/ownership discussion — omit only at T0>
- Technical contact: <name — omit only at T0; same as business contact if that's genuinely one person>
- Infrastructure contact: <name — T2+ only, whoever owns the hosting/deploy environment>
- Resulting tier: T<0–3>
- Key answers:
  - Reach / persistence / external calls: <summary>
  - Integration / shared data / operational reliance: <summary>
  - Regulated data / customer-facing / autonomous action / impact: <summary>
- Rationale: <one or two sentences on why this tier>

## Business Case
<omit this whole block at T0>
- Why now: <B1>
- Expected benefit: <B2 — quantify if at all possible; write "not quantified" rather than leaving it blank>
- Effort estimate: <B3 — size (S/M/L/XL) or a days/weeks figure, and who is building it>
- One-time cost: <B4 — amount, or "none">
- Monthly recurring cost: <B5 — amount, or "€0">
- Cost/benefit call: <B6 — "benefit clearly outweighs cost" / "close call — escalated to <name>" / "not assessed — Tier 0/1 experiment">
- Presented for sign-off: <T3 only — who it was walked through with, and when; leave "N/A" below T3>
```

Recording the kit version matters as much as the tier: if the kit's requirements change later, an audit needs to check the project against the rules that were actually in force, not today's version. If you cloned the repository, the tag is `git describe --tags`; if the user handed you loose files, ask them which version they got, or note "unknown — files provided directly."

This block is the first thing an auditor reads later — it shows the tier was reasoned, not guessed, and gives a re-assessment something concrete to compare against. The Business Case section is what a manager or the governance council reads when they're asked to approve or continue funding the project — keep it current, not just accurate on the day it was written.

## 6. After classification — hand off to the matching starter

| Tier | Open this file |
|---|---|
| T0 | `PROJECT_STARTER_T0_PERSONAL.md` |
| T1 | `PROJECT_STARTER_T1_BASIC.md` |
| T2 | `PROJECT_STARTER_T2_STANDARD.md` |
| T3 | `PROJECT_STARTER_T3_CRITICAL.md` (and read `AI_PROJECT_STRUCTURE.md` in full, plus `reference/AI_Development_Standard.docx` Sections 4–5) |

Continue the conversation inside that file's structure — you already have the problem statement, owner, and business case from this interview; don't re-ask for them.

At T1+, also create `OWNERSHIP_TRANSFER.md` from `OWNERSHIP_TRANSFER_TEMPLATE.md` in the project root, pre-filled with the owner and scope you already have — it's a living document kept current as the project evolves, not a form filled in once and forgotten. Its entire point is that the project survives its current owner leaving; a stale one is barely better than none.

At T3, do not treat the business case as filed-and-forgotten once it's in `PROJECT.md`: it must actually be walked through with the business contact's manager or the relevant governance seat before go-live (Section 4), the same way the security sign-off is a real approval and not a checkbox.

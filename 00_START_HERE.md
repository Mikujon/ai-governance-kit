# Start here — pick the right project starter

This is the index for the AI project-governance document set, maintained at `wearefiber/ai-governance-kit`.

## Why this exists

Every internally-built AI tool or automation is a small liability if nobody but its author understands it: when that person changes teams or leaves, the tool either breaks silently, keeps running with nobody watching its security or its cost, or gets rebuilt from scratch by someone else who didn't know it already existed. This kit exists so that never has to happen — every tool gets a named owner, a written scope, a security check sized to its actual risk, and a place other people can find it — scaled so a five-minute script isn't held to the same bar as something touching customer data. `PILLARS_COVERAGE.md` shows exactly how each of the seven founding requirements behind this kit is met, file by file.

## This covers more than "AI projects"

If it automates something, remembers something between runs, or was built with AI assistance at all, it's in scope — regardless of language or platform. Don't let the word "AI" in the kit's name narrow it in your head. Concretely, all of the following go through the same `00_START_HERE.md` → starter flow, just at whatever tier they actually land on:

| It looks like… | It's actually | Likely tier |
|---|---|---|
| A Google Apps Script or Sheets macro that reformats a report you run yourself now and then | A personal script | T0 |
| A Sheets/Apps Script automation that emails a report to your team on a schedule | Basic internal tool (persists nothing shared, but runs unattended and others rely on it) | T1–T2 |
| A Power Automate / Zapier / Make flow that moves data between two systems | Data architecture & integration in scope, even with zero code written | T2 |
| An RPA bot or scheduled Python script that reads from one team's system and writes to another's | Shared data + operational reliance | T2 |
| Any of the above if it touches customer, personal, financial or health data, or acts (sends, pays, deletes) without a human checking first | Regulated/autonomous | T3 |

If you're building something and thinking "this is just a script, it doesn't need any of this" — that's exactly the sentence this kit is designed to catch. Run the classification in `AI_INTAKE_ASSESSMENT.md` or the table below anyway; it takes a minute, and Tier 0 is a perfectly valid, low-friction answer if that's genuinely where it lands.

## Getting the kit — no local copy required

If your AI assistant has git or network access, don't attach these files by hand — have it read them directly from the repository at a pinned version:

```bash
git clone --branch v1.10.0 https://github.com/wearefiber/ai-governance-kit.git
```

Or fetch a single file without cloning (useful inside a prompt):

```
https://raw.githubusercontent.com/wearefiber/ai-governance-kit/v1.10.0/AI_INTAKE_ASSESSMENT.md
```

Always reference a version tag (e.g. `v1.1.0`), not `main` — record that tag in the project's `PROJECT.md` (Section 4 of `AI_INTAKE_ASSESSMENT.md`) so an audit later knows exactly which rules were in force when the project was built, even if the kit has changed since. See `CHANGELOG.md` in the repository for what changed between versions.

There are three ways to use the kit — pick one:

- **Option A — let the AI run the assessment (recommended).** Give your AI assistant **`AI_INTAKE_ASSESSMENT.md`** together with the four `PROJECT_STARTER_*.md` files and say you want to build something. The assistant interviews you, decides the tier itself, records why, and continues straight into the matching starter — you don't have to self-classify or hand it a second file.
- **Option B — classify it yourself.** Answer the questions below, pick the matching starter, and hand your assistant **only that one file**.
- **Option C — audit something that already exists.** Not building anything new — you want to check a tool that's already running. Use **`AI_PROJECT_AUDIT.md`** instead of the intake assessment; see "Later — the audit" below.

## Option B — self-classification

| # | Question |
|---|---|
| 1 | Will anyone other than the author ever run this, or does it run more than a handful of times? |
| 2 | Does it read or write data that persists beyond a single run (a database, a shared file, a queue)? |
| 3 | Does it call an external AI/LLM provider or any third-party API? |
| 4 | Will another internal tool or team need to consume its output or integrate with it? |
| 5 | Does it touch data shared with or owned by another team/system? |
| 6 | Is it expected to run unattended on a schedule, or be relied on operationally? |
| 7 | Does it process personal, financial, health, or other regulated data? |
| 8 | Is it customer-facing, or does its output reach customers directly or indirectly? |
| 9 | Can it take an action with financial, legal, or contractual effect without a human approving it first? |
| 10 | Would its failure or a wrong output cause material business, legal, or reputational harm? |

| If... | Use this file |
|---|---|
| No question answered "yes" | **`PROJECT_STARTER_T0_PERSONAL.md`** |
| Highest "yes" is question 1, 2 or 3 | **`PROJECT_STARTER_T1_BASIC.md`** |
| Highest "yes" is question 4, 5 or 6 | **`PROJECT_STARTER_T2_STANDARD.md`** |
| Any of question 7–10 is "yes" | **`PROJECT_STARTER_T3_CRITICAL.md`** |

If it's a close call, pick the heavier tier — it's cheaper to lightly over-document a small tool than to find a Tier 3 gap after something goes wrong.

## Give the files to your AI assistant

**Option A prompt:**

> "Here are our AI project governance documents. Run the intake assessment in `AI_INTAKE_ASSESSMENT.md` — ask me the questions, decide the tier, then build [describe what you need]."

**Option B prompt** (you already picked the starter):

> "Use this file as the project structure and requirements. Build [describe what you need]."

**Option C prompt** (auditing something that already exists):

> "Here are our AI project governance documents. Run the audit in `AI_PROJECT_AUDIT.md` against [name/describe the existing tool] — confirm its tier, walk the checklist, and produce the audit document."

Either way (A or B), the assistant scaffolds the repository, applies the listed requirements, and fills in the problem statement and sign-off blocks with you as it works. For C, it produces a written audit report instead — see below.

## Later — the audit

This is what `AI_PROJECT_AUDIT.md` (Option C, above) is for — don't just open a starter's checklist and eyeball it yourself. Give your AI assistant that file and ask it to run the audit: it confirms (or re-derives) the tier, walks the correct checklist for that tier — the starter's **Compliance checklist** for T0–T2, the full 42-point checklist in **`reference/AI_Development_Standard.docx`** for T3, never a shortened version — and produces a written audit document with evidence per item, not a verbal impression. It also checks the project's recorded kit version first (`AI_INTAKE_ASSESSMENT.md` Section 4, cross-referenced against `CHANGELOG.md`) so the audit is against the rules that were actually live when the project was built, unless you specifically ask it to check against today's kit instead.

## The full document set

| File | What it's for |
|---|---|
| `00_START_HERE.md` | This index. |
| `AI_INTAKE_ASSESSMENT.md` | Gives the AI assistant a script to interview the user and self-determine the tier (Option A). |
| `AI_PROJECT_AUDIT.md` | Gives the AI assistant a script to audit a tool that already exists (Option C) — confirm/re-derive the tier, walk the right checklist, produce a written audit document. |
| `PROJECT_STARTER_T0_PERSONAL.md` | Starter kit for personal / throwaway scripts and reports. |
| `PROJECT_STARTER_T1_BASIC.md` | Starter kit for small internal tools with a handful of users. |
| `PROJECT_STARTER_T2_STANDARD.md` | Starter kit for departmental tools that integrate with other systems/data. |
| `PROJECT_STARTER_T3_CRITICAL.md` | Starter kit for tools touching regulated data, customers, or autonomous actions. |
| `AI_PROJECT_GUIDELINES.md` | The full tiering model and requirements matrix behind these starters (reference only). |
| `AI_PROJECT_STRUCTURE.md` | The full technical standard behind these starters — stack, ReBAC, data, design, CI/CD (reference only). |
| `reference/AI_Development_Standard.docx` | The complete governance policy and 42-point audit checklist for Tier 3. |
| `reference/Guida_Uso_Kit_Governance_AI.docx` | Step-by-step user guide (Italian) for people using the kit day to day. |
| `PILLARS_COVERAGE.md` | Maps each of the 7 founding requirements to exactly where it's defined, enforced, and tracked, plus a crosswalk to NIST AI RMF / EU AI Act / ISO 42001. Read this first if you're being asked "does this kit actually cover X?" |
| `AI_ASSISTANT_USAGE_GUIDE.md` | How to actually run Claude Code / Codex CLI: install, which mode to use, how to cut token/cost usage, and — critically — what the assistant must never be given access to. Read this before your first real session, regardless of tier. |
| `GOVERNANCE.md` | Who decides: the four-seat governance council, what needs whose approval, and the continuity plan if the Governance Administrator is unavailable. Read this if you're escalating a Tier 3 sign-off or a disputed classification. |

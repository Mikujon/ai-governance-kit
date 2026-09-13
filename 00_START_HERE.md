# Start here — pick the right project starter

This is the index for the AI project-governance document set, maintained at `Mikujon/ai-governance-kit`.

## Getting the kit — no local copy required

If your AI assistant has git or network access, don't attach these files by hand — have it read them directly from the repository at a pinned version:

```bash
git clone --branch v1.0.0 https://github.com/Mikujon/ai-governance-kit.git
```

Or fetch a single file without cloning (useful inside a prompt):

```
https://raw.githubusercontent.com/Mikujon/ai-governance-kit/v1.0.0/AI_INTAKE_ASSESSMENT.md
```

Always reference a version tag (`v1.0.0`), not `main` — record that tag in the project's `PROJECT.md` (Section 4 of `AI_INTAKE_ASSESSMENT.md`) so an audit later knows exactly which rules were in force when the project was built, even if the kit has changed since. See `CHANGELOG.md` in the repository for what changed between versions.

There are two ways to use the kit — pick one:

- **Option A — let the AI run the assessment (recommended).** Give your AI assistant **`AI_INTAKE_ASSESSMENT.md`** together with the four `PROJECT_STARTER_*.md` files and say you want to build something. The assistant interviews you, decides the tier itself, records why, and continues straight into the matching starter — you don't have to self-classify or hand it a second file.
- **Option B — classify it yourself.** Answer the questions below, pick the matching starter, and hand your assistant **only that one file**.

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

Either way, the assistant scaffolds the repository, applies the listed requirements, and fills in the problem statement and sign-off blocks with you as it works.

## Later — the audit

When it's time to check a project is compliant, open its starter file (and, for Option A, the `## Project Classification` block it recorded in `PROJECT.md`): the starter's **Compliance checklist** section is the audit instrument. For Tier 3 projects, the full 42-point checklist lives in **`reference/AI_Development_Standard.docx`** — use that instead of the short version. Check the project's recorded kit version against `CHANGELOG.md` first — audit against the rules that were live when the project was built, not necessarily today's.

## The full document set

| File | What it's for |
|---|---|
| `00_START_HERE.md` | This index. |
| `AI_INTAKE_ASSESSMENT.md` | Gives the AI assistant a script to interview the user and self-determine the tier (Option A). |
| `PROJECT_STARTER_T0_PERSONAL.md` | Starter kit for personal / throwaway scripts and reports. |
| `PROJECT_STARTER_T1_BASIC.md` | Starter kit for small internal tools with a handful of users. |
| `PROJECT_STARTER_T2_STANDARD.md` | Starter kit for departmental tools that integrate with other systems/data. |
| `PROJECT_STARTER_T3_CRITICAL.md` | Starter kit for tools touching regulated data, customers, or autonomous actions. |
| `AI_PROJECT_GUIDELINES.md` | The full tiering model and requirements matrix behind these starters (reference only). |
| `AI_PROJECT_STRUCTURE.md` | The full technical standard behind these starters — stack, ReBAC, data, design, CI/CD (reference only). |
| `reference/AI_Development_Standard.docx` | The complete governance policy and 42-point audit checklist for Tier 3. |
| `reference/Guida_Uso_Kit_Governance_AI.docx` | Step-by-step user guide (Italian) for people using the kit day to day. |

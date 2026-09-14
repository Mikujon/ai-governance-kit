---
name: start-ai-project
description: Run the full AI Project Governance Kit flow end-to-end for a new tool or automation — classify its tier, run the business case, scaffold the matching starter, and set up its ownership-transfer file. Use when someone says they want to build a new AI tool, script, automation, or app, or asks "what tier is this" / "does this need governance."
---

# Start a new AI project under governance

You are running the wearefiber AI Project Governance Kit's full intake flow, **automatically, in one pass** — the person invoking this should not need to know which file does what, paste a prompt from a README, or juggle four starter files themselves. That manual path still exists (`00_START_HERE.md` Option A) as the fallback for anyone without this skill installed; this skill *is* that path, wired up so a single command runs it.

## What to do, in order

1. **Locate the kit.** Look for these files in the current repo (root or a `governance/`, `docs/`, or vendored subfolder): `AI_INTAKE_ASSESSMENT.md`, `AI_PROJECT_GUIDELINES.md`, `PROJECT_STARTER_T0_PERSONAL.md` through `_T3_CRITICAL.md`, `OWNERSHIP_TRANSFER_TEMPLATE.md`, `PILLARS_COVERAGE.md`, `GOVERNANCE.md`. If you can't find them locally and have network access, clone the pinned version: `git clone --branch v1.4.0 https://github.com/wearefiber/ai-governance-kit.git /tmp/ai-governance-kit` (check `CHANGELOG.md` in that clone for the actual latest tag — don't assume v1.4.0 is still current) and read from there. If neither is possible, say so and stop — do not improvise a classification from memory of what this kit "usually" asks; the files are the source of truth and they change.

2. **Run the interview.** Follow `AI_INTAKE_ASSESSMENT.md` exactly: Section 1's process, Section 2's ten tier questions, Section 3's tier logic. Ask conversationally, in your own words, grouped naturally — never dump the raw table at the user. Push back on "it's just a script" per the file's own instructions.

3. **State the tier and get agreement** before moving on, per Section 1 step 5.

4. **Run the business case (Section 4)**, scaled to the tier that was just settled: skip entirely at T0, one line + monthly cost at T1, the full B1–B6 at T2/T3. Do not skip B5 (monthly recurring cost) even when the honest answer is "€0" — record the zero.

5. **Record the assessment** in `PROJECT.md` using the exact block in Section 5, including the `## Business Case` section (omitted only at T0). Note the kit version/tag you're working from.

6. **Open the matching starter** (Section 6) and continue scaffolding inside it — pre-fill everything you already collected, never re-ask.

7. **Create `OWNERSHIP_TRANSFER.md`** from `OWNERSHIP_TRANSFER_TEMPLATE.md` at T1+, pre-filled with what you already know (owner, one-paragraph scope, business case cost figures). Tell the user explicitly that this is a living file they should update as the project evolves, not a one-time form.

8. **At T2, flag** that the business case should be presented for go-live sign-off (recommended); **at T3, flag it as a hard gate** — a real conversation with the business owner's manager or the relevant governance seat (`GOVERNANCE.md`), not just a filled-in paragraph — and remind the user this is one of the "Before go-live — gate items" in `PROJECT_STARTER_T3_CRITICAL.md` Section 4.

9. **Summarize before you scaffold any code**: tier, one-line rationale, business case in three bullets (why / cost / benefit), owner, and what happens next (which starter, what's required at this tier). Get an explicit go-ahead before writing files.

## What this skill deliberately does not do

- It does not lower or skip requirements to move faster — if the interview surfaces a Tier 3 trigger, say so even if the user was hoping for Tier 1.
- It does not fabricate numbers for the business case. If effort or cost is genuinely unknown, record "not yet estimated" and say so out loud — that's an honest gap the user can fill in, not something for you to guess at so the field isn't blank.
- It does not replace the human sign-off required at T2 (recommended) and T3 (mandatory) — it gets the business case *ready to present*, it doesn't substitute for the presentation itself.
- It does not touch production credentials, deploy anything, or run destructive commands — see `AI_ASSISTANT_USAGE_GUIDE.md` for what you must never be given access to, regardless of the tier this lands on.

## Installing this skill in your own project

Copy this `start-ai-project/` folder into your project's `.claude/skills/start-ai-project/` (or your global `~/.claude/skills/`), alongside a copy or clone of the kit itself so the files in step 1 are actually reachable. Then just invoke `/start-ai-project` — the whole flow above runs without anyone needing to open `00_START_HERE.md` first.

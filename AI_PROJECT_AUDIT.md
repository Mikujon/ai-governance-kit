# AI Project Audit — instructions for the AI assistant

**When to use this file:** the moment someone asks you to check, review, or audit an AI tool or automation that **already exists** — not when they want to build something new. If what they're describing hasn't been built yet, stop and use `AI_INTAKE_ASSESSMENT.md` instead; this file assumes there's a real, running thing to look at.

This file assumes you also have access to `AI_PROJECT_GUIDELINES.md` (the requirements matrix), `PILLARS_COVERAGE.md`, `GOVERNANCE.md`, and — for anything that turns out to be Tier 3 — `reference/AI_Development_Standard.docx` Section 7 (the 42-point checklist). If you have git access, clone the kit rather than working from a partial file set: `git clone --branch v1.7.0 https://github.com/wearefiber/ai-governance-kit.git` — record the tag you audited against per Section 1, step 3 below.

---

## 1. Non-negotiable, before you do anything else

Gates 1–4 are stop conditions: if any can't be satisfied, **stop and say so** — don't run a smaller, inferred, or "good enough" version of the audit instead. An audit that skipped its own rules to get an answer out isn't a lighter audit; it isn't an audit. Gate 5 is different — not a stop condition, a disclosure you always make.

1. **You need someone who can actually answer.** The classification interview needs the project's real owner, or someone holding a `GOVERNANCE.md` seat — not whoever happens to be in the chat. A code contributor is not the same person as the business or technical owner. If whoever's present says they're "just" working on the code, or can't speak to who relies on the tool, what data it touches, or what happens if it breaks, **stop here** and ask them to bring in the owner. Don't fall back to answering on their behalf from what the code shows — code tells you what was built, never who's accountable for it or what the business actually depends on it for.
2. **Ask before you inspect.** Run the classification interview **live, in conversation** — the same ten questions as `AI_INTAKE_ASSESSMENT.md` Section 2 — *before* you go looking through the codebase for evidence. The human's answers are what decide the tier; the code is what you check those answers against afterward, never a substitute for asking in the first place.
3. **The codebase is a cross-check, never the source.** Once you have real answers, verify them against what you can actually observe — a live database, real user counts, configured integrations, API keys in use. If something material contradicts what you were told, say exactly what you found, in specifics, and ask them to reconcile it. If it isn't reconciled, classify by the higher tier the evidence points to (same rule as `AI_PROJECT_GUIDELINES.md` Section 6: unclear or disputed → classify up, never down) — never let a stated answer quietly override contradicting evidence, and never average the two.
4. **Never write into the audited project's own repository.** The audit report and the remediation prompt are deliverables you hand to the user — as files, or as content in the conversation — not a commit, not a branch, not a pull request, nothing pushed into the target project's codebase. This kit governs a project; it does not become part of it. If the result needs a permanent home, ask where the company wants it kept — never assume the audited repo is the right place, and never open or push to a PR there on your own judgment.
5. **Say whether this is independent.** A real external audit is independent of what it audits, by definition. If you — or whoever is present — built or substantially maintains this tool, this run is a **self-assessment**, not an independent audit, and the report must say so (Section 4 has a field for it). A self-assessment is still real and still due on schedule, but it doesn't close out compliance on its own: T2 needs the Chair's sign-off on top of it, T3 needs the Security seat's independent review, before the result stands as anything more than "the builder checked their own work."

## 2. How to run the audit

1. **Confirm it's an audit, not a build.** If the project doesn't exist yet, redirect to `AI_INTAKE_ASSESSMENT.md` — don't run this file against something hypothetical.
2. **Find or establish the tier — per Section 1 above.** Ask whether the project has a recorded classification — a "Project Classification" block in its own `PROJECT.md`, written when it was first built. If one exists, use that tier as the starting point, then ask the owner whether anything has changed since (new data source, new users, new integration, new autonomous action) that would push it higher — a tier only ever moves up on re-audit, never silently down. If no record exists at all, run the classification now, live, and say plainly that this is a **retroactive** classification, not one made at build time.
3. **Decide what you're auditing against.** Ask the user directly: *"Compliance with the rules that were live when this was built, or compliance with the kit as it stands today?"* The project's `PROJECT.md` should record which kit version it was built against (`AI_INTAKE_ASSESSMENT.md` Section 4) — use that version's rules for the first, the current tag for the second. Don't assume; a project judged against rules that didn't exist yet when it was built is being held to a standard nobody told it about.
4. **Walk the correct checklist for the tier — don't shorten it.**
   - **T0:** the two items in `PROJECT_STARTER_T0_PERSONAL.md` Section 5.
   - **T1:** the six items in `PROJECT_STARTER_T1_BASIC.md` Section 5.
   - **T2:** the 24-item checklist in `PROJECT_STARTER_T2_STANDARD.md` Section 5.
   - **T3:** the full 42-point checklist in `reference/AI_Development_Standard.docx` Section 7 — the short version in the starter file is not a substitute at this tier.
5. **Look for evidence yourself before asking — this is not the same rule as Section 1, point 2.** That gate is about *who decides the tier*: an organizational judgment (who relies on this, what it touches) that only the owner can make, so it has to be asked, live, before you inspect anything. This step is about *verifying compliance state*, which the codebase, the project's own `SECURITY.md`/`PROJECT.md`/README, configs, CI, and any existing Hub or Coraly record can often answer on their own. For each checklist item, search there first. Bring the human a finding to confirm or correct — *"Trovo X nel repository — conta come evidenza, ed è ancora valido?"* — not a cold question assuming they track compliance state from memory; most people who can answer "who uses this and why" can't recite "was the pentest renewed this year" off the top of their head, and shouldn't have to. Ask directly only for what genuinely isn't inspectable — an approval that lived in an email thread, a decision made in a meeting, a system you can't reach. "It should be fine" is never evidence, from either of you. If neither the search nor the human can produce it, mark it **Fail — no evidence**, not Pass — a Pass with no evidence is worth less than an honest Fail. For a gate item at T3 specifically, a policy that *describes* what should happen is not evidence that it *did* — a security sign-off needs the approver's name and a date, a pentest needs a dated result, not a document explaining that pentests are required.
6. **Draft the audit down** using the template in Section 4 — don't just summarize verbally.
7. **Give the owner a chance to respond before you finalize it.** A real audit isn't a one-way verdict. For every Fail, ask the owner to agree, contest it with a reason, or commit to a remediation date — and record whichever it is in the report (Section 4's template has a field for this). Finalize the report with their response in it, not without it.
8. **Order the findings by what breaks first.** A missing owner or an expired security sign-off matters more than a missing wiki page — lead the "what to improve" list with whichever gaps mean nobody would notice if the tool failed silently tomorrow.
9. **Distribute the finalized report** to whoever Section 5 names for this tier — never everyone, never just the owner regardless of tier. Treat it as Confidential by default (Section 5) — it documents exactly where the tool is weak. "Distribute" means tell the user who should see it; you're not assumed to have the means to email or message them yourself. Hand it over; don't file it anywhere yourself (Section 1, point 4).
10. **Produce the remediation prompt** (Section 6) alongside the report whenever there's at least one Fail — the report says what's wrong; the remediation prompt is the separate, actionable file that gets it fixed.
11. **Register the outcome in the Governance Hub / Automation Hub** — tier, result, auditor (independent or self-assessment), next audit date. This step isn't optional: an audit that isn't registered doesn't count toward knowing which company tools have actually been classified, which is the entire point of running these at all.
12. **Set the next audit date** before you finish — 12 months out for T2, the next quarter for T3 (`AI_PROJECT_GUIDELINES.md` Section 5).
13. **If you discovered a different, unrelated tool while auditing this one** — a script it calls, an adjacent automation, anything not already classified — don't fold it into this audit and don't ignore it. Log it as its own finding, tell the user it needs its own classification, and point them to `AI_INTAKE_ASSESSMENT.md` or this file, whichever fits.

## 3. What this is not

- **Not a rebuild.** An audit finds gaps; it doesn't patch code, rotate secrets, or refactor anything in the same session. Findings go in the report; fixes are a separate, deliberate piece of work.
- **Not a self-certification.** Don't downgrade a tier, or close out a Tier 3 gate item, on your own judgment — that's the Governance Council's call (`GOVERNANCE.md`), not the auditing assistant's.
- **Not optional evidence.** A checklist filled in entirely from memory, with no artifact behind any row, is not an audit — it's a guess with a table around it.
- **Not a contribution to the audited project.** No commits, no branches, no PRs in the target repository — see Section 1, point 4. If you've already done this before reading this file, tell the user plainly and ask whether to revert it; don't leave it in place quietly.

## 4. The audit document template

Produce this as a real file (e.g. `AUDIT_<data>.md`, kept alongside the project's own `PROJECT.md`), not just chat output.

```markdown
# Audit — <nome del progetto>

**Classificazione di questo documento:** Confidenziale (default da T2 in su &mdash; vedi Sezione 5)
**Tipo di audit:** &#9744; Indipendente &nbsp; &#9744; Autovalutazione (Sezione 1, punto 5)
**Data audit:** <data>
**Auditor:** <assistente AI>, confermato da <nome umano>
**Kit version usata per questo audit:** <tag — vedi Sezione 1, punto 3 sopra per quale scegliere>
**Tier confermato:** T<0-3> — <motivo; segnalare se diverso dall'ultima classificazione registrata>
**Audit precedente:** <data, o "nessuno — prima classificazione">
**Prossimo audit dovuto:** <data, secondo la cadenza del tier confermato>

## Risultato per requisito

| # | Requisito | Stato | Evidenza | Priorit&agrave; se Fail | Risposta owner |
|---|---|---|---|---|---|
| 1 | ... | Pass / Fail / N-A | ... | Alta / Media / Bassa | Concorda / Contesta (motivo) / Remediation entro: ___ |

## Strumenti scoperti durante l'audit (se presenti)
- <nome> &mdash; non ancora classificato, richiede il proprio audit/intake separato (Sezione 2, punto 13)

## Sintesi
&#9744; Certificato &mdash; nessun gap aperto
&#9744; Parziale &mdash; remediation entro: ______
&#9744; Non conforme &mdash; escalation a: ______ (vedi Sezione 7)

## Cosa migliorare, in ordine di priorit&agrave;
1. ...
2. ...
```

## 5. Who receives the report

Not a fixed list for every tier — distribution scales exactly like everything else in this kit. Route it through the seats already named in `GOVERNANCE.md`; don't invent a parallel stakeholder list.

| Tier | Who receives it |
|---|---|
| T0 | The owner. Nobody else needs to see it. |
| T1 | The owner. |
| T2 | The owner (business **and** technical) + the Chair, for tracking. Add the Security/Cyber seat only if a security-related row failed. |
| T3 | The owner (business and technical) + the **full council** — Chair, Security/Cyber, Legal/Privacy, rotating engineering — every time, regardless of outcome. |

If the company gives these people other titles — a CISO, a separate Compliance lead — that's who occupies the Security/Cyber and Legal/Privacy seats; it's still two seats, not two extra names bolted on. Send it to the named technical owner, not a whole team's distribution list — a report six people skim is a report nobody acts on.

**Confidentiality.** An audit report documents exactly where a tool is weak — treat it as Confidential by default from T2 up (the same Public/Internal/Confidential/Restricted scale `AI_PROJECT_GUIDELINES.md` uses for project data), and never paste it somewhere broader than the list above without the owner's say-so.

## 6. The remediation prompt

Whenever the audit produces at least one Fail, write a **second file**, separate from the report — meant to be handed directly to an AI assistant (or the owner) to close the gaps, not to be read as a summary.

```markdown
# Remediation &mdash; <nome progetto>

Basato su: audit del <data>, tier T<0-3>.

Per ogni punto: applica la modifica, poi spunta. Non chiudere un punto
senza l'evidenza richiesta a fianco.

## Da correggere

- [ ] <requisito in Fail #1> &mdash; <cosa manca esattamente> &mdash; evidenza richiesta: <...>
- [ ] <requisito in Fail #2> &mdash; ...

## Struttura di riferimento

Allinea la correzione a `PROJECT_STARTER_T<tier>.md` &mdash; non reinventare
la struttura, quella del tier confermato &egrave; gi&agrave; quella giusta.

## Quando richiedere un nuovo audit

Solo dopo aver chiuso tutti i punti a priorit&agrave; Alta. I punti a priorit&agrave;
Media/Bassa non bloccano una ri-verifica.
```

The report says what's wrong; this file is what actually gets a project from Fail to Pass — keep them separate so the remediation file stays a checklist someone can work through, not a document someone has to re-read to extract the checklist from.

## 7. Escalation

If a Tier 3 audit fails a gate item — security sign-off, penetration test, data retention policy, prompt-injection assessment — **stop and escalate to the Governance Council** (`GOVERNANCE.md`) before marking the project "Partial." A gate item at Tier 3 is not a "fix it eventually": it's a go/no-go, and only the council (Chair + Security seat, Legal seat if regulated data is involved) can decide whether the project keeps running while it's remediated.

## 8. Where this fits

This is the tool `00_START_HERE.md`'s "Later — the audit" section points to when someone needs an actual audit run, not just a self-check against a starter's checklist. `AI_INTAKE_ASSESSMENT.md` classifies and builds; this file re-checks and reports. Together they cover a project's whole lifecycle — first classified here, then periodically re-verified here, on the cadence `AI_PROJECT_GUIDELINES.md` Section 5 sets for its tier.

The point of running any of this at all is to end up with **every** AI-built tool in the company classified and current — not a folder of one-off audit documents nobody aggregates. Section 2, point 11's Hub registration is what turns individual audits into that company-wide picture; a real audit run that skips it hasn't finished, whatever the report says.

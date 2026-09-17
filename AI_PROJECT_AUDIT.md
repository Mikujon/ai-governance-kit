# AI Project Audit — instructions for the AI assistant

**When to use this file:** the moment someone asks you to check, review, or audit an AI tool or automation that **already exists** — not when they want to build something new. If what they're describing hasn't been built yet, stop and use `AI_INTAKE_ASSESSMENT.md` instead; this file assumes there's a real, running thing to look at.

This file assumes you also have access to `AI_PROJECT_GUIDELINES.md` (the requirements matrix), `PILLARS_COVERAGE.md`, `GOVERNANCE.md`, and — for anything that turns out to be Tier 3 — `reference/AI_Development_Standard.docx` Section 7 (the 42-point checklist). If you have git access, clone the kit rather than working from a partial file set: `git clone --branch v1.4.0 https://github.com/wearefiber/ai-governance-kit.git` — record the tag you audited against per Section 1, step 3 below.

---

## 1. How to run the audit

1. **Confirm it's an audit, not a build.** If the project doesn't exist yet, redirect to `AI_INTAKE_ASSESSMENT.md` — don't run this file against something hypothetical.
2. **Find or establish the tier.** Ask whether the project has a recorded classification — a "Project Classification" block in its own `PROJECT.md`, written when it was first built. If one exists, use that tier as the starting point, then ask whether anything has changed since (new data source, new users, new integration, new autonomous action) that would push it higher — a tier only ever moves up on re-audit, never silently down. If no record exists at all, run the classification now using the ten questions in `AI_INTAKE_ASSESSMENT.md` Section 2, and say plainly that this is a **retroactive** classification, not one made at build time.
3. **Decide what you're auditing against.** Ask the user directly: *"Compliance with the rules that were live when this was built, or compliance with the kit as it stands today?"* The project's `PROJECT.md` should record which kit version it was built against (`AI_INTAKE_ASSESSMENT.md` Section 4) — use that version's rules for the first, the current tag for the second. Don't assume; a project judged against rules that didn't exist yet when it was built is being held to a standard nobody told it about.
4. **Walk the correct checklist for the tier — don't shorten it.**
   - **T0:** the two items in `PROJECT_STARTER_T0_PERSONAL.md` Section 5.
   - **T1:** the six items in `PROJECT_STARTER_T1_BASIC.md` Section 5.
   - **T2:** the 24-item checklist in `PROJECT_STARTER_T2_STANDARD.md` Section 5.
   - **T3:** the full 42-point checklist in `reference/AI_Development_Standard.docx` Section 7 — the short version in the starter file is not a substitute at this tier.
5. **Mark each item Pass / Fail / N/A — and ask for evidence.** "It should be fine" is not evidence. Ask for something you could point to: a link, a config file, a log, a named approver. If evidence can't be produced on the spot, mark it **Fail — no evidence**, not Pass. A Pass with no evidence is worth less than an honest Fail.
6. **Write the audit down** using the template in Section 3 — don't just summarize verbally. The document is what gets attached to the project's record in the Governance Hub.
7. **Order the findings by what breaks first.** A missing owner or an expired security sign-off matters more than a missing wiki page — lead the "what to improve" list with whichever gaps mean nobody would notice if the tool failed silently tomorrow.
8. **Distribute the report** to whoever Section 4 names for this tier — never everyone, never just the owner regardless of tier.
9. **Produce the remediation prompt** (Section 5) alongside the report whenever there's at least one Fail — the report says what's wrong; the remediation prompt is the separate, actionable file that gets it fixed.
10. **Set the next audit date** before you finish — 12 months out for T2, the next quarter for T3 (`AI_PROJECT_GUIDELINES.md` Section 5).

## 2. What this is not

- **Not a rebuild.** An audit finds gaps; it doesn't patch code, rotate secrets, or refactor anything in the same session. Findings go in the report; fixes are a separate, deliberate piece of work.
- **Not a self-certification.** Don't downgrade a tier, or close out a Tier 3 gate item, on your own judgment — that's the Governance Council's call (`GOVERNANCE.md`), not the auditing assistant's.
- **Not optional evidence.** A checklist filled in entirely from memory, with no artifact behind any row, is not an audit — it's a guess with a table around it.

## 3. The audit document template

Produce this as a real file (e.g. `AUDIT_<data>.md`, kept alongside the project's own `PROJECT.md`), not just chat output.

```markdown
# Audit — <nome del progetto>

**Data audit:** <data>
**Auditor:** <assistente AI>, confermato da <nome umano>
**Kit version usata per questo audit:** <tag — vedi Sezione 1, punto 3 sopra per quale scegliere>
**Tier confermato:** T<0-3> — <motivo; segnalare se diverso dall'ultima classificazione registrata>
**Audit precedente:** <data, o "nessuno — prima classificazione">
**Prossimo audit dovuto:** <data, secondo la cadenza del tier confermato>

## Risultato per requisito

| # | Requisito | Stato | Evidenza | Priorit&agrave; se Fail |
|---|---|---|---|---|
| 1 | ... | Pass / Fail / N-A | ... | Alta / Media / Bassa |

## Sintesi
&#9744; Certificato &mdash; nessun gap aperto
&#9744; Parziale &mdash; remediation entro: ______
&#9744; Non conforme &mdash; escalation a: ______ (vedi Sezione 6)

## Cosa migliorare, in ordine di priorit&agrave;
1. ...
2. ...
```

## 4. Who receives the report

Not a fixed list for every tier — distribution scales exactly like everything else in this kit. Route it through the seats already named in `GOVERNANCE.md`; don't invent a parallel stakeholder list.

| Tier | Who receives it |
|---|---|
| T0 | The owner. Nobody else needs to see it. |
| T1 | The owner. |
| T2 | The owner (business **and** technical) + the Chair, for tracking. Add the Security/Cyber seat only if a security-related row failed. |
| T3 | The owner (business and technical) + the **full council** — Chair, Security/Cyber, Legal/Privacy, rotating engineering — every time, regardless of outcome. |

If the company gives these people other titles — a CISO, a separate Compliance lead — that's who occupies the Security/Cyber and Legal/Privacy seats; it's still two seats, not two extra names bolted on. Send it to the named technical owner, not a whole team's distribution list — a report six people skim is a report nobody acts on.

## 5. The remediation prompt

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

## 6. Escalation

If a Tier 3 audit fails a gate item — security sign-off, penetration test, data retention policy, prompt-injection assessment — **stop and escalate to the Governance Council** (`GOVERNANCE.md`) before marking the project "Partial." A gate item at Tier 3 is not a "fix it eventually": it's a go/no-go, and only the council (Chair + Security seat, Legal seat if regulated data is involved) can decide whether the project keeps running while it's remediated.

## 7. Where this fits

This is the tool `00_START_HERE.md`'s "Later — the audit" section points to when someone needs an actual audit run, not just a self-check against a starter's checklist. `AI_INTAKE_ASSESSMENT.md` classifies and builds; this file re-checks and reports. Together they cover a project's whole lifecycle — first classified here, then periodically re-verified here, on the cadence `AI_PROJECT_GUIDELINES.md` Section 5 sets for its tier.

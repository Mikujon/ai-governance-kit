# The 7 founding requirements — where each one lives in this kit

This kit exists to answer one original brief in a way that scales to every project's actual size, instead of a one-size-fits-all policy nobody follows. This page is the traceability map: each requirement below is quoted as it was first asked for, followed by exactly where it is defined, where it is enforced, and where it is made visible to the rest of the company. Use this page to answer "does the kit actually cover X?" without reading every file — and use it as the first exhibit in an audit or certification, since it shows the requirement was designed for, not bolted on after the fact.

---

## 1. Guarantee of security / cyber approval, to mitigate possible risks

- **Defined:** `AI_PROJECT_GUIDELINES.md` Section 4 "Security & data handling" (tiered rules) and the Requirements matrix row "Formal security/cyber review & sign-off". `AI_PROJECT_STRUCTURE.md` Section 3 gives the technical controls (secrets, SSO, SAST, TLS, encryption). `reference/AI_Development_Standard.docx` Section 5.1 and the 42-point checklist (Section 7) are the full policy for Tier 3.
- **Enforced:** every starter (`PROJECT_STARTER_T*.md`) carries the security items due at its own tier as a checklist the assistant fills in while building — not as a document read once and forgotten. Tier 3 treats it as a **gate**: `PROJECT_STARTER_T3_CRITICAL.md` Section 4 lists "written security approval" as one of the items that must be *closed, not merely planned*, before go-live.
- **Visible:** the Governance Hub artifact's "Security & cyber" panel per tool — approval status, risk level, next re-assessment date, notes/approver. A tool with no approval or an expired one shows a warning badge on the Hub's overview.

## 2. Shared DB architecture, and made possible to integrate with other tools

- **Defined:** `AI_PROJECT_GUIDELINES.md` Section 4 "Data architecture & integration"; `AI_PROJECT_STRUCTURE.md` Section 5 (data structure standards) and Section 1 (the shared **model gateway**, so AI/model access is centralized rather than duplicated per project).
- **Enforced:** from Tier 2 up, a project must check whether an existing shared schema already covers the need *before* standing up a new store, publish a schema/data dictionary, and (Tier 3) expose data only through a documented, versioned API or event stream — never an undocumented direct DB link. `PROJECT_STARTER_T2_STANDARD.md` Section 4 "Data & integration" turns this into a build-time checklist.
- **Visible:** the Governance Hub's "Data architecture" field per tool records where the data lives and which other tools/systems already integrate with it, so the next team doesn't have to reverse-engineer it or build a duplicate store.

## 3. A wiki of the processes covered

- **Defined:** `AI_PROJECT_GUIDELINES.md` Section 4 "Documentation" (tiered: nothing at T0, a README at T1, a wiki page at T2, a full runbook at T3). Requirements matrix row "Process wiki page".
- **Enforced:** `PROJECT_STARTER_T2_STANDARD.md` and `_T3_CRITICAL.md` both scaffold `docs/architecture.md` and `docs/data-flow.md` as required deliverables, and T3 adds `docs/runbook.md` and `docs/exceptions.md` (the fallback procedure).
- **Visible:** the Governance Hub's "Process wiki" field holds the actual link and a last-updated date, so a stale or missing wiki page is a visible gap, not a silent one.

## 4. A scope of the tool and the problem it resolves

- **Defined:** every starter's Section 1 is a problem-statement template, scaled by tier — one line at T0/T1, full in-scope/out-of-scope/success-metric at T2/T3. `reference/AI_Development_Standard.docx` Appendix C is the full problem-statement template for Tier 3.
- **Enforced:** `AI_INTAKE_ASSESSMENT.md` collects this *during the classification interview itself*, so it exists before a line of code is written, and the assistant pre-fills the starter's Section 1 from the interview rather than asking twice. The same interview (Section 4) immediately follows scope with the business case — why the project needs to exist, expected benefit, effort, and cost — so scope and justification are captured in one sitting, not two.
- **Visible:** the Governance Hub's "Scope" field is the same text, one field, one source of truth — not a document that drifts from what the Hub says.

## 5. An owner, for future development, features, or switching the tool off

- **Defined:** Requirements matrix row "Named owner" (from T1) and "business **and** technical owner" (T2+). `AI_PROJECT_GUIDELINES.md` Section 4 "Ownership", including a 5-business-day transfer SLA on a role change or departure.
- **Enforced:** every starter from T1 up has an explicit owner field in its problem statement, and `AI_INTAKE_ASSESSMENT.md` Section 5's classification record includes an `Owner` line so it's recorded at classification time, not left to be filled in later and forgotten. From T1 up, every project also carries its own `OWNERSHIP_TRANSFER.md` (`OWNERSHIP_TRANSFER_TEMPLATE.md`) — access checklist, current status, pending work, and cost commitments — kept current as a living document, not written once and left to rot. This is what actually makes the 5-business-day transfer SLA achievable instead of aspirational: the SLA is a deadline, the transfer file is what lets someone hit it.
- **Visible:** the Governance Hub's "Ownership" panel shows business + technical owner per tool — this is exactly the field a manager checks when someone leaves, before a tool is left ownerless.

## 6. An automatic reminder for the tool to be checked for cyber and utility

- **Defined:** `AI_PROJECT_GUIDELINES.md` Section 4 "Review & reminders" and Section 5 "Audit cadence by tier" (12 months at T2, quarterly at T3). Requirements matrix row "Automatic review reminder".
- **Enforced today:** every T2/T3 starter requires a next-review date to be set at go-live (`PROJECT_STARTER_T2_STANDARD.md` Section 4 "Governance visibility"; `PROJECT_STARTER_T3_CRITICAL.md` Section 4).
- **Visible / the actual mechanism:** the Governance Hub records a `nextReview` date per tool and its overview surfaces an **"Overdue review"** count and a per-tool warning badge the moment that date passes — this is what makes the reminder "automatic" rather than a note in someone's calendar. Two ways to close the gap between "visible on a dashboard someone has to open" and "someone gets pinged": (a) the simple version — the owner adds the next-review date to their own calendar/task tool as part of go-live, which every starter already asks for; (b) the stronger version — wire a scheduled job (a Power Automate/Zapier flow, an RPA schedule, or a Claude/other scheduled task) to read the Hub weekly and message overdue owners directly in Slack/Teams/email. (b) is a natural next iteration for this kit, not yet built — call it out as a gap rather than claim it's solved if it isn't wired up yet in your environment.

## 7. Inclusion on the automatic utility/RPA hub, to understand utilization and savings

- **Defined:** Requirements matrix rows "Registered in the AI Tools Governance Hub" and "Registered in the RPA / utility hub" (both from T2). `AI_PROJECT_GUIDELINES.md` Section 4 "Central visibility".
- **Enforced:** T2/T3 starters both list Hub + RPA/utility-hub registration as a go-live requirement, and T3 keeps the savings/usage figures refreshed at least quarterly. The estimated-savings figure itself isn't invented at registration time — it's the same "expected benefit" (B2) and "monthly recurring cost" (B5) numbers already captured in the business case (`AI_INTAKE_ASSESSMENT.md` Section 4, `PROJECT.md`'s `## Business Case`), carried forward rather than re-estimated from scratch.
- **Visible:** the Governance Hub's "RPA / utility hub" panel is a single yes/no plus an estimated-savings field per tool — deliberately simple, so it rolls up next to actual RPA-platform tools even when the automation itself was never built on that platform (a Python script, a Sheets macro, a Power Automate flow — see "This also covers…" in `00_START_HERE.md`).

---

## 8. Framework crosswalk — what an outside auditor calls each tier

The seven pillars above are this kit's own vocabulary. The table below is the translation layer: it maps each tier to the frameworks a security questionnaire, an enterprise customer, or an external auditor will actually name — NIST's AI Risk Management Framework, the EU AI Act's risk classes, and ISO/IEC 42001 (the certifiable AI-management-system standard). Use this table to answer "how does this compare to a recognized standard?" without having to reverse-engineer the mapping under audit pressure.

| Kit tier | Typical project | NIST AI RMF | EU AI Act risk class* | ISO/IEC 42001 |
|---|---|---|---|---|
| **T0** — Personal | Throwaway script, run by its author only | Govern (baseline hygiene) | Minimal risk | Out of scope |
| **T1** — Basic | Small internal tool, a handful of users | Govern + light Map | Minimal, or Limited if it's chat/generative-facing | Awareness only |
| **T2** — Standard | Departmental tool, shared data, scheduled or relied on | Govern, Map, Measure | Limited — transparency duties (Article 50) typically apply | In scope: risk assessment + controls |
| **T3** — Critical | Regulated data, customer-facing, or autonomous action | Full Govern–Map–Measure–Manage loop | Often High-risk if it matches an Annex III use case | Full AI management system — audit/certification-relevant evidence |

\* Directional, not a legal classification. The EU AI Act's risk class is determined by the actual use case against Annex I/III, not by this internal tier — route anything landing in the T3 row past the Legal/Privacy seat (`GOVERNANCE.md`) before relying on this mapping. As of this writing, the Act's general transparency obligations are already in force (2 August 2026); the Digital Omnibus has deferred the High-risk obligations under Annex III to December 2027 and Annex I to August 2028 — recheck this footnote's dates against current guidance before citing it in an external audit, since regulatory timelines are the part of this table most likely to move.

---

## Where this stops being "written policy" and starts being "the actual system of record"

The `.md` files in this kit are the **rules**: what's required, at which tier, and why — and, since `GOVERNANCE.md`, *who* decides when a rule is unclear or a sign-off is needed. The Governance Hub artifact is the **register**: the live, shared place those rules get recorded per tool, so an audit is "open the Hub record" rather than "go find whoever built it and ask." Both are needed — a rule nobody records against is unauditable, and a register with no rule behind it is just a spreadsheet. If your organization doesn't yet have the Hub linked, treat every "Visible" line above as the target state and record the same fields in `PROJECT.md` in the meantime.

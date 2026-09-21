# AI Development Guidelines

**Purpose:** this file is written to be handed directly to an AI coding assistant (or read by a developer) at the start of any internally-built AI tool. It tells the assistant which governance requirements apply, scaled to the project's actual size and risk — a one-off report does not carry the same weight as a customer-facing automation.

**How to use this file (instructions to the assistant):**
1. Before writing code, work through [Section 1](#1-classify-the-project) with the user and state the resulting tier and why.
2. Apply only the requirements listed for that tier in [Section 3](#3-requirements-matrix) and [Section 4](#4-best-practices-by-area) — do not impose a higher tier's requirements on a lower-tier project.
3. Once the tier is set, check [Section 7](#7-adaptive-deepening--follow-ups-by-tool-type-and-business-area) for tool-type and business-area follow-ups that apply, and fold those into the same interview alongside the fixed ten questions — they deepen the conversation where this specific project actually needs it; they don't replace the fixed questions everyone gets, and they don't move the tier on their own.
4. If the project's scope grows mid-build (new data source, new users, new integration), re-run the classification — the tier can go up, never silently down.
5. At the end of the build, output the applicable rows of Section 3 as a checklist for the user, marked done/not done.

---

## 1. Classify the project

Answer these questions. **The project's tier is the highest tier triggered by any "yes."**

| # | Question | If yes → at least |
|---|---|---|
| 1 | Will anyone other than the author ever run this, or does it run more than a handful of times? | Tier 1 |
| 2 | Does it read or write data that persists beyond a single run (a database, a shared file, a queue)? | Tier 1 |
| 3 | Does it call an external AI/LLM provider or any third-party API? | Tier 1 |
| 4 | Will another internal tool or team need to consume its output or integrate with it? | Tier 2 |
| 5 | Does it touch data shared with or owned by another team/system (not just the author's own files)? | Tier 2 |
| 6 | Is it expected to run unattended on a schedule, or be relied on operationally (someone would notice if it broke)? | Tier 2 |
| 7 | Does it process personal, financial, health, or other regulated data? | Tier 3 |
| 8 | Is it customer-facing, or does its output reach customers directly or indirectly? | Tier 3 |
| 9 | Can it take an action with financial, legal, or contractual effect without a human approving it first (send, pay, sign, delete)? | Tier 3 |
| 10 | Would its failure or a wrong output cause material business, legal, or reputational harm? | Tier 3 |

No "yes" anywhere → **Tier 0**.

---

## 2. The four tiers

| Tier | Name | Typical example | Governance weight |
|---|---|---|---|
| **0** | Personal / throwaway | A one-off script to reformat a file, a quick chart for a meeting, a personal prompt template, a Sheets formula or Apps Script macro you run yourself now and then | Almost none — use good judgment, nothing to register |
| **1** | Basic internal tool | A small script a team runs weekly, a personal report generator others start using, a Sheets/Apps Script automation that emails a scheduled report, a personal Zapier/Make flow | Light — named owner, basic security hygiene, no formal Hub record required but recommended |
| **2** | Standard business tool | A departmental automation, a tool that reads/writes a shared database, an internal chatbot, an RPA bot or a Power Automate/Zapier/Make flow moving data between two systems | Full seven-pillar Hub record, without the heaviest security testing |
| **3** | Critical / regulated | Anything touching customer or personal data, anything acting autonomously, anything customer-facing — no-code and low-code automations included; the platform doesn't change the tier | Full seven-pillar Hub record **and** the complete technical requirements in the AI Development Standard, including penetration testing and quarterly review |

Tier 3 projects must also follow the full **AI Development Standard** document (the technical bible with the 42-point audit checklist) — this file gives the scaled-down version for Tiers 0–2, and points Tier 3 to that fuller standard rather than duplicating it.

---

## 3. Requirements matrix

`●` required · `○` recommended · `—` not required

| Requirement | T0 | T1 | T2 | T3 |
|---|---|---|---|---|
| Problem statement (one line) | ○ | ● | ● | ● |
| Full scope doc (in/out of scope, success metric) | — | — | ● | ● |
| Named owner | — | ● | ● | ● (business **and** technical) |
| RACI matrix | — | — | ○ | ● |
| Secrets kept out of code (vault/env, never hardcoded) | ● | ● | ● | ● |
| No personal/sensitive data without a check first | ● | ● | ● | ● |
| Formal security/cyber review & sign-off | — | — | ○ | ● |
| Penetration test | — | — | — | ● |
| Data architecture documented for reuse/integration | — | — | ● | ● |
| Exposed via documented API/contract (not ad-hoc) | — | — | ○ | ● |
| Process wiki page | — | ○ (short) | ● | ● (+ runbook) |
| Registered in the AI Tools Governance Hub | — | ○ | ● | ● |
| Registered in the RPA / utility hub | — | — | ● | ● |
| Automatic review reminder | — | — | ● (12 mo) | ● (quarterly) |
| Ownership-transfer SLA on attrition | — | — | ● | ● |

---

## 4. Best practices by area

Each item is tagged with the tier it starts applying at — apply everything tagged at or below the project's tier.

### Scope & problem statement
- **[T0+]** Write one sentence: what problem this solves and for whom.
- **[T2+]** Add target users, expected usage, and explicit in-scope / out-of-scope.
- **[T2+]** Define a success metric before building, not after.

### Ownership
- **[T1+]** Name a person accountable for the tool (even if it's the author, write it down somewhere findable).
- **[T2+]** Separate business owner (value/retire decisions) and technical owner (maintenance).
- **[T2+]** Ownership transfers within 5 business days of a role change or departure — never left pointing at someone who's gone.

### Security & data handling
- **[T0+]** Never hardcode API keys, passwords, or tokens — use environment variables or the company vault, even for a throwaway script.
- **[T0+]** Before touching any personal, customer, financial, or health data, pause and classify it (Public / Internal / Confidential / Restricted) — if in doubt, treat it as Confidential and ask.
- **[T1+]** Use the company SSO/OAuth2 for anything with a login; no personal accounts wired into shared tools.
- **[T2+]** Log access and usage; keep logs at least 12 months.
- **[T2+]** Run a dependency/vulnerability scan before go-live.
- **[T3]** Formal written security approval before go-live, recorded with approver, risk tier, and expiry.
- **[T3]** Penetration test before go-live and annually thereafter.
- **[T3]** Explicitly assess prompt-injection and data-leakage risk for any generative/LLM component.
- **[T3]** Any third-party AI/model provider reviewed for data residency and retention; signed data processing agreement if personal data leaves the company.

### Data architecture & integration
- **[T0-T1]** Keep it simple — a local file or a personal table is fine.
- **[T2+]** Before creating a new data store, check whether an existing shared schema/platform already covers the need.
- **[T2+]** Publish a schema or data dictionary if others might need it.
- **[T3]** Expose data to other tools only through a documented, versioned API or event stream — never an undocumented direct link into a database.
- **[T3]** Document the data contract (schema + availability expectation) and a retention/deletion policy.

### Documentation
- **[T0]** None required, but a short comment block at the top of the script (what it does, who wrote it) costs nothing.
- **[T1+]** A short README: what it does, how to run it, who to ask.
- **[T2+]** A wiki page: architecture overview, data flow, and what to do if it breaks.
- **[T3]** Add a full runbook (deploy, rollback, on-call escalation), an exception-handling procedure, and a model/prompt version log.

### Review & reminders
- **[T2+]** Set a recurring reminder to check the tool is still secure and still worth keeping — 12 months for Tier 2.
- **[T3]** Quarterly reminder, covering both a cyber re-check and a utility check (is it still used, still worth the cost).
- **[T2+]** A missed review flags the tool for follow-up after a 30-day grace period.

### Central visibility (Hub & RPA/utility hub)
- **[T1]** Registration is optional but encouraged if you expect others to eventually rely on it.
- **[T2+]** Register the tool in the AI Tools Governance Hub with owner, scope, and data notes.
- **[T2+]** Register in the RPA/utility hub so usage and estimated savings roll up into company-wide reporting, whether or not it's built on the RPA platform.
- **[T3]** Keep the savings/usage figures refreshed at least quarterly.

---

## 5. Audit cadence by tier

| Tier | Certification required? | Cadence |
|---|---|---|
| 0 | No | — |
| 1 | No (spot-checked only if it grows in use) | — |
| 2 | Yes | Every 12 months |
| 3 | Yes | Every quarter |

Use the same pass/fail structure as the AI Development Standard's checklist (Section 7 there), but only audit the rows that applied to the project's tier under Section 3 above — a Tier 1 tool is never marked non-compliant for skipping a Tier 3 requirement.

---

## 6. If the tier is unclear

When a project sits on the edge (for example, a "basic report" that quietly reads customer data, or a personal script three other people now depend on), classify up, not down, and say so to the user — it's cheaper to lightly over-document a Tier 1 tool than to discover a Tier 3 gap after something goes wrong.

This applies identically to anything built with no-code/low-code tools — a Sheets/Apps Script macro, a Power Automate/Zapier/Make flow, an RPA bot. The classification questions in Section 1 don't mention a programming language on purpose: what matters is who relies on it, what it touches, and what it can do on its own, never what it was built with.

---

## 7. Adaptive deepening — follow-ups by tool type and business area

The ten questions in Section 1 decide the tier and stay identical for every project — that's what makes tiers comparable across the company over time, and what stops the interview from bending to fit whoever's answering it. This section is the layer on top: once you know the tier, match the project against every row below that applies (a project can match several) and add those specific questions to the same live interview (`AI_INTAKE_ASSESSMENT.md`) or, during a later audit, the same evidence-gathering pass (`AI_PROJECT_AUDIT.md` Section 2). They add depth where a generic ten-question interview wouldn't reach on its own. They never substitute for the fixed ten, and a match here never moves the tier by itself — it's a prompt to ask more, not a reclassification trigger.

### By tool type

| Signal | What it usually means it touches | Ask about |
|---|---|---|
| Uses an LLM / generative AI component | Prompt injection surface, a third-party model provider | Is user or document input ever fed straight into the prompt? What happens if the model is manipulated into ignoring its instructions? What does the model provider do with the data it's sent — residency, retention, training use? |
| RPA bot / browser automation (Playwright, UiPath, Power Automate desktop, etc.) | Logs in as a real account, acts inside another system's own UI | What account/credentials does it log in as, and what can that account do beyond this one task? If a selector or step breaks silently, does it fail loud or keep going with wrong data? Does it complete send/pay/delete actions unattended, or stop for a human first? |
| No-code flow (Zapier/Make/Power Automate/Apps Script) | Anyone with edit access can change its behavior without review | Who can edit this flow, and is that access logged or reviewed? What triggers it — a schedule, a webhook, a form — and whose data does that trigger expose to whoever can see the flow's run history? |
| Data pipeline / ETL / scheduled report | Moves or reshapes data between systems | Trace it start to finish: where does data enter, what transforms happen, where does it land, who or what reads it from there? Is anything dropped or masked before regulated data would otherwise land somewhere broader than its original access scope? |
| Customer-facing app or chatbot | Its output reaches someone outside the company | What happens when it gives a wrong or harmful answer — is there a human escalation path? How long is a customer's session/conversation data kept, and who can access it? Can the customer tell they're talking to an AI? |
| Internal assistant with tool/function access (MCP, shell, file, API calls) | Its blast radius is whatever its tools can do, not just what it says | List every tool/function it can call — which of those write, delete, or send rather than just read? Is every call logged with what triggered it? |

### By business area / data domain

| Area | Ask about |
|---|---|
| Finance / Payments | Is this in PCI scope? Does moving money ever happen without a second human approving first? How are reconciliation and error-correction handled? |
| HR / People data | What's the classification of what it touches (salary, performance, health, immigration status)? Is access scoped so someone only sees their own team, not the whole company? Any works-council or legal notice obligation before deploying this? |
| Sales / CRM | Does it touch marketing-consent status? Is data retention here consistent with the CRM's own policy, or does it create a second copy that outlives the original? |
| Legal / Compliance | Does it touch privileged or litigation-hold material? Could its output ever be treated as the company's official position? |
| Customer Support | What can it see of a customer's history, and can it take actions on their account (refund, cancel, change details)? |
| Engineering / Infra | What production access does it have, and is that scoped to only what this specific task needs? |

If a project matches something not listed here — a new tool type or business area the company starts using — ask the equivalent question yourself (what does this touch, what can go wrong, who'd notice) instead of skipping the deepening step because no row fits exactly, then suggest adding a row here so the next interview or audit doesn't have to improvise the same thing again.

# Project Starter — Tier 3: Critical / Regulated

**Use this when:** the project processes personal, financial or health data; is customer-facing; can take a financial, legal or contractual action without a human approving it first; or its failure could cause material business, legal or reputational harm.

**This is the only tier with no shortcuts.** Nothing here is trimmed down — this starter scaffolds the project and points to the two documents that define the full requirement, so nothing drifts out of sync with a duplicated copy:

- **`AI_PROJECT_STRUCTURE.md`** — apply **every** section, not just the `[T3]`-tagged lines (the lower tags apply too, they're a floor, not an alternative).
- **`reference/AI_Development_Standard.docx`** — the complete policy and the 42-point audit checklist. This is the audit instrument for this tier; do not use a shortened checklist.

**Instructions to the AI assistant:** before writing any code, read `AI_PROJECT_STRUCTURE.md` in full and `reference/AI_Development_Standard.docx` Sections 4–5, then scaffold the structure below. Flag to the user any Section 5 requirement (of the Standard) that needs a person, not code — the security sign-off, the penetration test, the DPA — those are approvals to obtain, not steps to automate around.

---

## 1. Problem statement & RACI

> **Business problem:** ______________________________________________
>
> **Target users & expected usage:** ______________________________________________
>
> **In-scope:** ______________________________________________
> **Out-of-scope:** ______________________________________________
>
> **Success metric:** ______________________________________________
>
> **Business owner:** ________________  **Technical owner:** ________________
> **Risk tier confirmed as High** because: ______________________________________________

> **Business case**
> - Why now: ______________________________________________
> - Expected benefit (quantify if possible): ______________________________________________
> - Effort estimate (size or days/weeks, and who's building it): ______________________________________________
> - One-time cost: ______________________________________________
> - Monthly recurring cost: ______________________________________________
> - Cost/benefit call: ______________________________________________
> - **Presented for sign-off to:** ________________ **on:** ________________ (mandatory at this tier — see `AI_INTAKE_ASSESSMENT.md` Section 4 and `GOVERNANCE.md`)

| Activity | Responsible | Accountable | Consulted | Informed |
|---|---|---|---|---|
| Problem statement & scope | | | | |
| Business case & cost/benefit sign-off | | | | |
| Data architecture design | | | | |
| Security review & approval | | | | |
| Build & test | | | | |
| Documentation (wiki) | | | | |
| Go-live decision | | | | |
| Scheduled review | | | | |
| Retire / decommission | | | | |

## 2. Structure

```
project-root/
├── README.md
├── PROJECT.md
├── OWNERSHIP_TRANSFER.md     # from OWNERSHIP_TRANSFER_TEMPLATE.md — kept current, not filled once
├── SECURITY.md              # data classification + security sign-off reference
├── .env.example
├── docker-compose.yml
├── src/
│   ├── api/
│   ├── services/
│   ├── ai/                   # model gateway — provider clients, prompt templates, version log
│   ├── data/
│   ├── auth/                  # ReBAC — OpenFGA-backed (Section 4.3 of AI_PROJECT_STRUCTURE.md)
│   └── shared/
├── tests/
│   ├── unit/
│   └── integration/           # must cover the fallback/failure path, not only the happy path
├── docs/
│   ├── architecture.md
│   ├── data-flow.md
│   ├── runbook.md              # deploy / rollback / on-call escalation
│   └── exceptions.md            # manual fallback procedure
├── infra/
│   ├── docker/
│   └── ci/
└── CHANGELOG.md
```

## 3. Stack

Same as Tier 2 (TypeScript/Python, NestJS/FastAPI, PostgreSQL, Redis, Docker), with two additions:

- **Access control:** ReBAC via a shared **OpenFGA** instance, not a per-project relations table — permissions on regulated data must not be duplicated and drift between tools.
- **Model gateway:** the `src/ai/` module must log prompt version, token usage and latency for every call, and never log raw Confidential+ content without explicit approval.

## 4. Before go-live — gate items

These must all be closed, not merely planned, before release:

- [ ] Business case walked through out loud with the business owner's manager or the relevant governance seat — recorded in `PROJECT.md` (who, when), not just written down unread.
- [ ] `OWNERSHIP_TRANSFER.md` created and current — access checklist, status, pending work, cost commitments.
- [ ] Written security approval recorded in the Hub (approver, risk tier, date, expiry).
- [ ] Penetration test completed.
- [ ] Prompt-injection / data-leakage risk assessed and documented.
- [ ] Third-party AI/model provider reviewed for data residency; DPA signed if personal data leaves the company.
- [ ] Data retention & deletion policy implemented, not just written.
- [ ] Runbook and exception/fallback procedure written and understood by the technical owner's team (not only the original author).
- [ ] Rollback procedure tested at least once.
- [ ] Two approvers on any change touching `src/auth/`, `src/ai/`, or Confidential+ data.
- [ ] Registered in the AI Tools Governance Hub and the RPA/utility hub.
- [ ] Quarterly review reminder configured (cyber + utility check).

## 5. Audit

Use **`reference/AI_Development_Standard.docx`** Section 7 (the 42-point checklist) as-is — every item applies at this tier, none are skipped or substituted. Certification is valid 12 months from the audit date; the review cadence above (quarterly) re-checks utility and risk in between full audits.

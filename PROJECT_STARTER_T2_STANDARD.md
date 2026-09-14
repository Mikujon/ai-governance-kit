# Project Starter — Tier 2: Standard Business Tool

**Use this when:** a departmental automation, a tool that reads/writes a shared database, an internal chatbot, or anything another team depends on, integrates with, or that runs unattended on a schedule — but doesn't touch regulated/personal data, isn't customer-facing, and can't take an autonomous action with real-world consequences. If any of that becomes true, move up to `PROJECT_STARTER_T3_CRITICAL.md`.

**Instructions to the AI assistant:** scaffold the full structure below, implement every requirement in Section 4, and register the project in the AI Tools Governance Hub before go-live. This tier is the default for anything meant to last and be shared.

---

## 1. Problem statement

> **Business problem:** ______________________________________________
>
> **Target users & expected usage:** ______________________________________________
>
> **In-scope:** ______________________________________________
> **Out-of-scope:** ______________________________________________
>
> **Success metric:** ______________________________________________
>
> **Owner of record:** Governance Administrator — see `GOVERNANCE.md` (automatic from Tier 1 up, not chosen per project)
> **Business contact:** ________________  **Technical contact:** ________________  **Infrastructure contact:** ________________

> **Business case**
> - Why now: ______________________________________________
> - Expected benefit (quantify if possible): ______________________________________________
> - Effort estimate (size or days/weeks, and who's building it): ______________________________________________
> - One-time cost: ______________________________________________
> - Monthly recurring cost: ______________________________________________
> - Cost/benefit call: ______________________________________________

## 2. Structure

```
project-root/
├── README.md
├── PROJECT.md              # Section 1 above
├── OWNERSHIP_TRANSFER.md    # from OWNERSHIP_TRANSFER_TEMPLATE.md — kept current, not filled once
├── .env.example
├── docker-compose.yml
├── src/
│   ├── api/                 # routes / controllers — thin, no business logic
│   ├── services/            # business logic
│   ├── ai/                  # model gateway: provider clients, prompt templates, prompt version log
│   ├── data/                 # repositories, ORM models, migrations
│   ├── auth/                 # ReBAC permission checks (Section 4)
│   └── shared/                # types, utils, config
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
│   ├── architecture.md
│   └── data-flow.md
├── infra/
│   ├── docker/
│   └── ci/
└── CHANGELOG.md
```

## 3. Stack

| Layer | Standard choice |
|---|---|
| Application language | TypeScript (Node.js) |
| AI / data language | Python |
| Backend framework | NestJS or FastAPI |
| Frontend (if any) | React + Next.js, Tailwind + company component library |
| Database | PostgreSQL (`pgvector` for embeddings) |
| Caching / queues | Redis |
| ORM / migrations | Prisma or SQLAlchemy + Alembic |
| AI/LLM access | Only through `src/ai/` — never call a provider SDK from feature code |
| Containers | Docker + docker-compose |

## 4. Requirements

**Ownership**
- [ ] Owner of record shows the Governance Administrator (Section 1) — never an individual employee's name.
- [ ] Business, technical, and infrastructure contacts all named (Section 1).
- [ ] A RACI matrix is completed for the project (recommended at this tier, required at Tier 3) — the "Accountable" column is normally the Governance Administrator by design; contacts are typically "Responsible" or "Consulted".
- [ ] Contacts hand off within 5 business days of a role change or departure — the Owner of record doesn't change, so this is a handoff to log, not an ownership gap to fill.
- [ ] `OWNERSHIP_TRANSFER.md` created from `OWNERSHIP_TRANSFER_TEMPLATE.md` and kept current — reviewed at the same cadence as the 12-month check below, not only when someone actually leaves.

**Business case**
- [ ] Full business case completed in Section 1 (why / benefit / effort / cost).
- [ ] Business case presented for go-live sign-off (recommended at this tier — see `AI_INTAKE_ASSESSMENT.md` Section 4).
- [ ] Monthly recurring cost re-checked if usage or pricing changes materially after go-live.

**Security**
- [ ] No secret hardcoded — vault-backed in every shared/deployed environment.
- [ ] All external input validated at the boundary (`zod` / `pydantic`).
- [ ] SSO/OAuth2 for authentication; dependency scanning on every pull request.
- [ ] Access and admin actions logged, retained ≥ 12 months.
- [ ] SAST runs in CI; High/Critical findings block merge.
- [ ] TLS 1.2+ everywhere; encryption at rest for Confidential+ data.

**Data & integration**
- [ ] Before creating a new data store, check whether an existing shared schema covers it.
- [ ] Schema/data dictionary published and linked from the Hub record.
- [ ] Schema changes only through versioned migrations, never a manual `ALTER TABLE`.
- [ ] Columns holding Confidential+ data are tagged in the data dictionary.

**Access control (ReBAC)**
- [ ] Every permission check goes through one `check(object, relation, subject)` function in `src/auth/` — no scattered role checks in feature code.
- [ ] Implemented as a `relations` table in Postgres: `(object_type, object_id, relation, subject_type, subject_id)`.
- [ ] Standard relations used: `owner`, `editor`, `viewer`, `operator`, `member`.
- [ ] Every relation granted is logged (who, to whom, when).

**Documentation**
- [ ] `docs/architecture.md` and `docs/data-flow.md` published.
- [ ] Prompt/model version log maintained if the project has a generative component.

**Testing & CI/CD**
- [ ] Every pull request runs lint → unit tests → dependency/security scan → build; a failing stage blocks merge.
- [ ] At least one other person reviews and approves before merge.
- [ ] Local / staging / production environments use different credentials.

**Observability**
- [ ] Structured (JSON) logs with a request/correlation id.
- [ ] A health/status endpoint exists.
- [ ] Errors captured centrally (company error-tracking tool).
- [ ] Manual fallback procedure documented in `docs/architecture.md`.

**Governance visibility**
- [ ] Registered in the AI Tools Governance Hub with Owner of record, contacts, scope, and data notes.
- [ ] Registered in the RPA / utility hub with usage/savings tracking.
- [ ] Automatic review reminder set for 12 months (cyber + utility check).

## 5. Compliance checklist (for later reference)

| # | Requirement | Status (Pass/Fail/N-A) | Evidence |
|---|---|---|---|
| 1 | Owner of record = Governance Administrator; business/technical/infra contacts named | | |
| 2 | RACI completed | | |
| 3 | Contact-handoff SLA understood | | |
| 4 | `OWNERSHIP_TRANSFER.md` created and current | | |
| 5 | Business case completed (why / benefit / effort / cost) | | |
| 6 | Business case presented for go-live sign-off | | |
| 7 | No hardcoded secrets | | |
| 8 | Input validation at the boundary | | |
| 9 | SSO + dependency scanning | | |
| 10 | Access logging (≥12 months) | | |
| 11 | SAST in CI | | |
| 12 | TLS + encryption at rest | | |
| 13 | New-store check done; schema dictionary published | | |
| 14 | Migrations versioned, no manual schema edits | | |
| 15 | Sensitive columns tagged | | |
| 16 | ReBAC `check()` used everywhere; no scattered role checks | | |
| 17 | Relation grants logged | | |
| 18 | Architecture & data-flow docs published | | |
| 19 | Prompt/model version log (if applicable) | | |
| 20 | CI pipeline complete; ≥1 reviewer per merge | | |
| 21 | Environment separation with distinct credentials | | |
| 22 | Structured logs + correlation id | | |
| 23 | Health endpoint + central error tracking | | |
| 24 | Fallback procedure documented | | |
| 25 | Registered in Governance Hub | | |
| 26 | Registered in RPA/utility hub | | |
| 27 | 12-month review reminder set | | |

**Risk tier confirmed:** Medium   **Overall result:** ☐ Certified ☐ Partial — remediation by: ______ ☐ Non-compliant — escalated to: ______
**Audited by:** ________________  **Date:** ________________  **Next audit due:** ________________ (12 months)

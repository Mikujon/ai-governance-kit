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
> **Business owner:** ________________  **Technical owner:** ________________

## 2. Structure

```
project-root/
├── README.md
├── PROJECT.md              # Section 1 above
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
- [ ] Business owner and technical owner both named (Section 1).
- [ ] A RACI matrix is completed for the project (recommended at this tier, required at Tier 3).
- [ ] Ownership transfers within 5 business days of a role change or departure.

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
- [ ] Registered in the AI Tools Governance Hub with owner, scope, and data notes.
- [ ] Registered in the RPA / utility hub with usage/savings tracking.
- [ ] Automatic review reminder set for 12 months (cyber + utility check).

## 5. Compliance checklist (for later reference)

| # | Requirement | Status (Pass/Fail/N-A) | Evidence |
|---|---|---|---|
| 1 | Business & technical owner named | | |
| 2 | RACI completed | | |
| 3 | Ownership-transfer SLA understood | | |
| 4 | No hardcoded secrets | | |
| 5 | Input validation at the boundary | | |
| 6 | SSO + dependency scanning | | |
| 7 | Access logging (≥12 months) | | |
| 8 | SAST in CI | | |
| 9 | TLS + encryption at rest | | |
| 10 | New-store check done; schema dictionary published | | |
| 11 | Migrations versioned, no manual schema edits | | |
| 12 | Sensitive columns tagged | | |
| 13 | ReBAC `check()` used everywhere; no scattered role checks | | |
| 14 | Relation grants logged | | |
| 15 | Architecture & data-flow docs published | | |
| 16 | Prompt/model version log (if applicable) | | |
| 17 | CI pipeline complete; ≥1 reviewer per merge | | |
| 18 | Environment separation with distinct credentials | | |
| 19 | Structured logs + correlation id | | |
| 20 | Health endpoint + central error tracking | | |
| 21 | Fallback procedure documented | | |
| 22 | Registered in Governance Hub | | |
| 23 | Registered in RPA/utility hub | | |
| 24 | 12-month review reminder set | | |

**Risk tier confirmed:** Medium   **Overall result:** ☐ Certified ☐ Partial — remediation by: ______ ☐ Non-compliant — escalated to: ______
**Audited by:** ________________  **Date:** ________________  **Next audit due:** ________________ (12 months)

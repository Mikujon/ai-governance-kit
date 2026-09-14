# AI Project Technical Structure & Best Practices

**Purpose:** this file is the technical scaffold every AI project starts from — stack, folder structure, security, data, access control, design, testing and operations — so that any project built inside the company is structured the same way and can be audited against a fixed standard. It is a companion to two other files:

1. **`AI_PROJECT_GUIDELINES.md`** — classifies the project into a tier (T0 Personal → T3 Critical) based on its scope and risk.
2. **`AI_Development_Standard`** (docx) — the full governance requirements and the 42-point audit checklist for Tier 3 projects.

This file gives the concrete "how": the stack to use, the repo layout to generate, and the security/data/access-control/design/testing rules to apply — each tagged with the tier it starts applying at, exactly like `AI_PROJECT_GUIDELINES.md`.

**Instructions to an AI assistant reading this file:** classify the project's tier first (using `AI_PROJECT_GUIDELINES.md`), then scaffold and build using only the sections and tags at or below that tier. State the tier and which parts of this file you applied before writing code. When the project's tier is T3, also load the full `AI_Development_Standard` and satisfy every item in its Section 5 and 7.

---

## 1. Recommended technology stack

Use this stack unless the project already lives in an existing codebase with its own conventions — in that case, follow the existing codebase's stack and note the exception in the project's Hub record.

| Layer | Standard choice | Notes |
|---|---|---|
| Application language | **TypeScript** (Node.js runtime) | Default for services, APIs, and frontends. Strong typing catches errors before an audit ever has to. |
| AI / data / ML language | **Python** | Default for model pipelines, data processing, notebooks, and anything using an ML framework. |
| Backend framework | **NestJS** (TypeScript) or **FastAPI** (Python) | Both give a structured, modular shape (routes/controllers, services, DI) that keeps projects consistent and easy to review. |
| Frontend framework | **React** with **Next.js**, TypeScript | Server-side rendering where useful; static/SPA otherwise. |
| Styling / design system | **Tailwind CSS** + the company component library (see Section 6) | No ad-hoc CSS frameworks per project. |
| Primary database | **PostgreSQL** | Add the `pgvector` extension for embeddings/RAG use cases instead of standing up a separate vector database, unless scale requires one. |
| Caching / queues | **Redis** | Background jobs, rate limiting, short-lived state. |
| ORM / migrations | **Prisma** (TypeScript) or **SQLAlchemy + Alembic** (Python) | Schema is always defined in code and migrated, never edited by hand in production. |
| AI/LLM provider access | Through the shared **model gateway** module only (Section 3.4) — never call a provider SDK directly from feature code. | Centralizes keys, logging, retries, and prompt versioning. |
| Containers | **Docker**, `docker-compose` for local dev | One `Dockerfile` per service; no "works on my machine" deploys. |
| CI/CD | The company's existing CI (GitHub Actions unless told otherwise) | Pipeline stages defined in Section 7. |

---

## 2. Standard repository structure

**[T0–T1]** — minimal shape:

```
project-root/
├── README.md              # what it is, how to run it, who owns it
├── PROJECT.md             # problem statement (from AI_PROJECT_GUIDELINES.md §4)
├── .env.example
├── src/                   # all code
└── tests/
```

**[T2+]** — full shape:

```
project-root/
├── README.md
├── PROJECT.md              # scope, in/out of scope, success metric
├── .env.example            # never commit a real .env
├── docker-compose.yml
├── src/
│   ├── api/                # routes / controllers — thin, no business logic
│   ├── services/           # business logic
│   ├── ai/                 # model gateway: provider clients, prompt templates, prompt version log
│   ├── data/                # repositories, ORM models, migrations
│   ├── auth/                # ReBAC integration and permission checks (Section 4)
│   └── shared/              # types, utils, config
├── tests/
│   ├── unit/
│   └── integration/
├── docs/
│   ├── architecture.md      # diagram + narrative
│   ├── data-flow.md
│   └── runbook.md           # [T3] deploy / rollback / on-call
├── infra/
│   ├── docker/
│   └── ci/                  # pipeline definitions
└── CHANGELOG.md
```

**[T3]** additionally requires: a `SECURITY.md` documenting the approved data classification and the security sign-off reference, and a `docs/exceptions.md` for manual fallback procedures.

Every repository's `README.md` links to its record in the **AI Tools Governance Hub** — the Hub record is the source of truth for Owner of record, contacts, scope and review date; the README is the entry point for a developer or auditor to find it.

---

## 3. Security requirements

**[T0+]**
- No secret (API key, password, connection string, model key) is ever hardcoded or committed. Use `.env` locally (git-ignored) and the company secrets vault in any shared or deployed environment.
- Before touching personal, customer, financial or health data, classify it (Public / Internal / Confidential / Restricted per `AI_PROJECT_GUIDELINES.md`) — default to Confidential if unsure.
- Validate all external input at the boundary — use **zod** (TypeScript) or **pydantic** (Python); never trust a payload's shape.

**[T1+]**
- Authentication goes through the company SSO/OAuth2 provider — no personal accounts or ad-hoc logins wired into a shared tool.
- Dependencies are pinned (lockfile committed) and checked by automated dependency scanning (`npm audit` / `pip-audit`, or the company's Dependabot-equivalent) on every pull request.

**[T2+]**
- All access and administrative actions are logged (who, what, when), retained at least 12 months.
- Static analysis (SAST) runs in CI; a High/Critical finding blocks merge.
- Encryption in transit (TLS 1.2+) is enforced everywhere; anything stored at rest that is Confidential or above is encrypted at rest.

**[T3]**
- A formal security/cyber review is completed and signed off before go-live, recorded in the Hub with approver, risk tier and expiry — see `AI_Development_Standard` §5.1.
- A penetration test is completed before go-live and annually thereafter.
- Prompt-injection and data-leakage risk is explicitly assessed and documented for any generative/LLM component.
- Any third-party AI/model provider is reviewed for data residency and retention, with a signed data-processing agreement if personal data leaves the company.

### 3.4 The model gateway

Every project that calls an LLM or external AI API does so through one internal module (`src/ai/`), never by calling the provider SDK directly from feature code. The gateway is responsible for:
- Holding the provider credentials (from the vault), so no feature code ever sees a raw key.
- Logging every call's prompt version, token usage and latency (never the raw content of Confidential+ data, unless explicitly approved).
- Central retry, timeout and rate-limit handling.
- A versioned prompt store, so a prompt change is a reviewable diff, not a silent edit.

---

## 4. Access control model (ReBAC)

Role-only access control ("admin" / "user") breaks down quickly once tools need to share data with specific people, teams, or other tools. Every **[T2+]** project uses **relationship-based access control (ReBAC)** instead: permissions are derived from relationships between subjects and objects, not from a fixed role list.

### 4.1 Core model

Every permission is a **tuple**: `(object, relation, subject)`.

```
document:invoice-123   #owner    user:jetmir
document:invoice-123   #viewer   group:finance#member
report:q3-summary      #editor   user:arben
tool:invoice-bot        #owner    user:jetmir      # this is the technical contact with admin rights on the object,
                                                     # not the Hub's Owner of record — see GOVERNANCE.md Section 2
tool:invoice-bot        #operator user:driton       # day-to-day operator, ties back to the Hub's technical contact
```

A subject can itself be a set — `group:finance#member` means "anyone who is a member of the finance group" — which is how group and inherited permissions are expressed without duplicating tuples per person.

### 4.2 Standard relations

Define these relations consistently across projects unless a project genuinely needs more:

| Relation | Meaning |
|---|---|
| `owner` | Full control, including granting access to others and deleting the object. |
| `editor` | Can modify the object's content. |
| `viewer` | Can read the object. |
| `operator` | Can run/operate a tool without owning its data (maps to the Hub's technical contact). |
| `member` | Membership in a group, used as a subject set for the relations above. |

### 4.3 Implementation guidance by tier

- **[T2]** A `relations` table in the project's own Postgres database — `(object_type, object_id, relation, subject_type, subject_id)` — with a single `check(object, relation, subject)` function used everywhere access is decided. Simple, auditable, no new infrastructure.
- **[T3]**, or any project whose permissions must be shared across multiple tools, use a dedicated authorization service (**OpenFGA**, the open-source Zanzibar-model implementation) so permission data isn't duplicated and drifting between tools. Point every project's `check()` call at the same OpenFGA instance.

### 4.4 Rules

- **[T2+]** Access is never decided by scattering `if (user.role === 'admin')` checks through feature code — every check goes through the single `check(object, relation, subject)` function in `src/auth/`.
- **[T2+]** Every relation granted is logged (who granted it, to whom, when) — this log is part of the Section 3 access logging requirement.
- **[T3]** Access reviews (who has `owner`/`editor` on Confidential+ objects) happen on the same cadence as the tool's security review.

---

## 5. Data structure standards

**[T0+]**
- Data lives in a real, named store (a file, a table) — not scattered across ad-hoc variables or spreadsheets nobody else can find.

**[T1+]**
- Column and field names use `snake_case` in the database, `camelCase` at the API/JSON boundary; the ORM layer handles the conversion.
- No destructive migration (drop column, drop table) runs without a backup taken first and a rollback plan written down.

**[T2+]**
- Schema is defined in code (ORM models) and changed only through versioned migrations — never a manual `ALTER TABLE` against production.
- A schema/data dictionary is published and linked from the Hub record before any other tool is allowed to integrate.
- Before creating a new table or store, check whether an existing shared schema already covers the need — new stores are a documented decision, not a default.
- Columns holding Confidential or Restricted data are tagged in the data dictionary so they're easy to find in an audit.

**[T3]**
- Data exposed to other tools goes through a documented, versioned API or event contract (request/response schema, availability expectation) — never a direct database connection handed to another team.
- A retention and deletion policy is defined and enforced for every table holding personal data.

---

## 6. Design & UI standards

Applies to any project with a user interface.

**[T1+]**
- Use the company's shared component library and Tailwind design tokens rather than one-off styling — consistency across tools matters more than a bespoke look for an internal utility.
- Every interactive control has a visible focus state and a readable label — don't rely on color alone to convey status.

**[T2+]**
- New UI patterns (not covered by the existing component library) are reviewed before being built, so the library grows instead of forking into inconsistent one-offs.
- Minimum accessibility bar: WCAG 2.1 AA (color contrast, keyboard navigation, semantic HTML).

**[T3]**
- Any UI presenting Confidential+ data masks or redacts it by default where feasible (e.g., last 4 digits only), revealed only on explicit action.
- Customer-facing UI copy is reviewed for clarity on what the AI is doing and how to reach a human — no unexplained automated decisions.

---

## 7. Testing, CI/CD & code quality

**[T0–T1]**
- Code is linted and formatted before commit (ESLint + Prettier, or Ruff + Black) — a pre-commit hook is enough; no CI pipeline required.

**[T2+]**
- Every pull request runs: lint → unit tests → dependency/security scan → build. A failing stage blocks merge.
- At least one other person reviews and approves before merge.
- Unit test coverage on business logic (`src/services/`, `src/ai/`) is tracked; a meaningful drop is flagged in review, not just a hard percentage gate.
- Environments are separated: local → staging → production, with different credentials in each — production secrets never appear in a lower environment.

**[T3]**
- Two approvers required for changes touching `src/auth/`, `src/ai/`, or anything handling Confidential+ data.
- Integration tests cover the tool's failure/fallback path (Section 8), not only the happy path.
- A rollback procedure is tested at least once before go-live, not left as a theoretical plan in the runbook.

---

## 8. Observability & operations

**[T1+]**
- Logs are structured (JSON), not free-text `print`/`console.log` — at minimum: timestamp, level, message, and a request/correlation id.

**[T2+]**
- A health/status endpoint exists so uptime can be monitored.
- Errors are captured centrally (the company's error-tracking tool), not only visible in local logs.
- The manual fallback procedure (what a human does if the tool is down) is written down in `docs/architecture.md` or `docs/exceptions.md`.

**[T3]**
- On-call escalation path is documented in `docs/runbook.md` and known to the technical contact's team, not only the original author.
- Key metrics (usage volume, error rate, latency) feed into the RPA/utility hub reporting required by `AI_Development_Standard` §5.7.

---

## 9. Documentation requirements

| Document | Tier | Lives in |
|---|---|---|
| `PROJECT.md` (problem statement) | T0+ | Repo |
| `README.md` (what/how/who) | T1+ | Repo |
| Hub record (owner, scope, security status, review date) | T1+ (recommended), T2+ (required) | AI Tools Governance Hub |
| `docs/architecture.md`, `docs/data-flow.md` | T2+ | Repo, linked from Hub |
| `docs/runbook.md`, `docs/exceptions.md`, `SECURITY.md` | T3 | Repo, linked from Hub |
| Prompt/model version log | T2+ (any generative component) | `src/ai/`, linked from Hub |

---

## 10. Audit mapping

Every item above traces back to a numbered requirement in the `AI_Development_Standard`'s Section 7 checklist, so a project built to this structure is already positioned to pass its audit rather than scrambling to retrofit evidence.

| This document | Maps to `AI_Development_Standard` |
|---|---|
| §3 Security requirements | §5.1 Security & cyber approval (items 1–10) |
| §5 Data structure standards | §5.2 Shared data architecture & integration (items 11–17) |
| §9 Documentation requirements | §5.3 Process wiki & technical documentation (items 18–24) |
| `PROJECT.md` (§2, §9) | §5.4 Scope & problem statement (items 25–29) |
| §4 Access control (ReBAC ties to Hub owner tuples) | §5.5 Ownership (items 30–33) |
| §8 Observability (monitoring feeding review cadence) | §5.6 Automatic review reminder (items 34–38) |
| §8 Observability (usage metrics) | §5.7 RPA / utility hub registration (items 39–42) |

When auditing a project, walk this table left to right: check the structural requirement was actually implemented, then mark the corresponding `AI_Development_Standard` checklist item Pass/Fail with that implementation as the evidence.

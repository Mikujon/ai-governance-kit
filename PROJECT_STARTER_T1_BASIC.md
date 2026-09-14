# Project Starter — Tier 1: Basic Internal Tool

**Use this when:** a small script or tool a team runs repeatedly, a personal report generator others have started using, or anything that persists data or calls a third-party/AI API — but has no sensitive data, no integration with other systems, and low usage. If it starts being relied on operationally, shared with other teams, or touches shared/sensitive data, move up to `PROJECT_STARTER_T2_STANDARD.md`.

**Instructions to the AI assistant:** scaffold exactly this structure and apply exactly this requirement list — don't add Tier 2 items (formal security review, ReBAC, CI pipelines) unless the user asks for them explicitly.

---

## 1. Problem statement

> **What problem does this solve, and for whom?**
> ______________________________________________
>
> **Owner:** ______________________ (one person, accountable even if they're also the builder)
>
> **Why now (business case, light):** ______________________________________________
> **Monthly recurring cost** (API/hosting/subscription — write "€0" if none): ______________________________________________

## 2. Structure

```
project-root/
├── README.md            # what it is, how to run it, who owns it
├── PROJECT.md            # the problem statement above
├── OWNERSHIP_TRANSFER.md # recommended — from OWNERSHIP_TRANSFER_TEMPLATE.md
├── .env.example
├── src/
└── tests/
```

## 3. Stack

- **TypeScript** (Node.js) for services/scripts, or **Python** for data/AI-adjacent work.
- No database required unless the tool needs one — if it does, **PostgreSQL** or a local SQLite file is fine at this scale.
- If it calls an LLM/AI provider, isolate that call in its own module/function rather than scattering provider calls through the code — makes it trivial to move to Tier 2 later.

## 4. Requirements

**Ownership**
- [ ] A named owner is written down in `PROJECT.md` (Section 1) — even if it's the author.
- [ ] `OWNERSHIP_TRANSFER.md` created from `OWNERSHIP_TRANSFER_TEMPLATE.md` (recommended at this tier — cheaper to do now than to reconstruct after someone's left).

**Security**
- [ ] No secret is hardcoded — use `.env` (git-ignored) locally; use the company vault if this ever runs in a shared environment.
- [ ] Before using personal, customer, financial or health data, classify it (Public / Internal / Confidential / Restricted) — default to Confidential if unsure, and check with the security/cyber function before proceeding.
- [ ] Any login uses the company SSO/OAuth2 — no personal accounts wired into a shared tool.
- [ ] Dependencies are pinned (lockfile committed).

**Documentation**
- [ ] `README.md` explains what it does, how to run it, and who to ask.

**Visibility (optional but recommended)**
- [ ] Consider registering the tool in the AI Tools Governance Hub if you expect others to start relying on it — cheaper to do now than to retrofit later.

## 5. Compliance checklist (for later reference)

| # | Requirement | Status (Pass/Fail/N-A) | Evidence |
|---|---|---|---|
| 1 | Problem statement & owner written | | |
| 2 | Business case (light): why + monthly cost named | | |
| 3 | `OWNERSHIP_TRANSFER.md` created | | |
| 4 | No hardcoded secrets | | |
| 5 | Sensitive-data classification checked | | |
| 6 | SSO used for any login | | |
| 7 | Dependencies pinned | | |
| 8 | README complete | | |

**Audited by:** ________________  **Date:** ________________

No formal review cadence is required at this tier, but re-run the classification in `00_START_HERE.md` if the tool's use grows — most Tier 1 tools that get adopted by other teams become Tier 2.

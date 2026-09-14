# Ownership Transfer — `OWNERSHIP_TRANSFER.md` template

**What this is:** people are not eternal — contacts change roles, change teams, and leave the company. This file is what makes that take five business days (the SLA in `AI_PROJECT_GUIDELINES.md` Section 4) instead of five weeks of someone reverse-engineering the project from scratch. Required from **Tier 1 upward** (recommended at T1, required at T2/T3 — see the Requirements matrix in `AI_PROJECT_GUIDELINES.md` Section 3).

**Ownership vs. contacts — read this before filling anything in:** from Tier 1 up, the accountable **Owner of record is the Chair / Governance Administrator seat** (`GOVERNANCE.md` Section 2) — that never changes hands as part of this file, because the seat has its own continuity plan. What *this* file tracks are the **working contacts**: the people actually doing the business, technical, and infrastructure work day to day. They rotate; the file's job is to make sure that rotation is never a mystery to whoever picks it up next, and never has to become a governance emergency, because the governance answer never depended on them in the first place.

**Instructions to the AI assistant or the contact filling this in:**
- Copy this file into the project root as `OWNERSHIP_TRANSFER.md` and fill in every section — an empty section is worse than an honest "none" or "not yet."
- This is a **living document**, not a form filled in once at go-live. Update it whenever a contact changes, whenever access/credentials change, and at the same cadence as the project's own review reminder (`AI_PROJECT_GUIDELINES.md` Section 5) — not only when someone is actually about to leave.
- Never put an actual secret, password, or key value in this file. Point to *where* it lives (the vault path, the credential name) — this file is about findability, not custody.
- If you're the AI assistant maintaining a project, treat a stale `OWNERSHIP_TRANSFER.md` (referencing a person who already left, or a status paragraph months out of date) as a finding to raise with the Governance Administrator, the same way you'd flag an overdue security review.

---

## 1. Owner of record & current contacts

| Role | Name | Since | Backup / secondary contact |
|---|---|---|---|
| **Owner of record** (T1+, does not change here) | Chair / Governance Administrator — see `GOVERNANCE.md` | — | Security seat (interim, per `GOVERNANCE.md` Section 4) |
| Business contact | | | |
| Technical contact | | | |
| Infrastructure contact *(from T2, where there's an environment to run)* | | | |

> If business and technical contact are the same person (common at T1), say so explicitly — don't leave the row blank and ambiguous. The Owner of record row is filled in once and only changes if `GOVERNANCE.md` Section 1 changes who holds the Chair seat — it is never edited because a contact below it left.

## 2. What this project is, in one paragraph

Write this as if explaining it to the person picking it up cold — assume they've read the project's name and nothing else.

> ______________________________________________

Link back to the full problem statement in `PROJECT.md` rather than duplicating it here; if the two drift apart, `PROJECT.md` is the source of truth.

## 3. Access & credentials checklist

List *what* needs to be re-provisioned to a new owner and *where* it lives — never the secret value itself.

| What | Where it lives (vault path / system / admin to ask) | Re-provisioned to incoming contact? |
|---|---|---|
| Repository access | | ☐ |
| Production/staging credentials | | ☐ |
| Third-party AI/API provider account | | ☐ |
| Database / data-store access | | ☐ |
| Hosting / infrastructure console (cloud account, deploy pipeline) | | ☐ |
| Monitoring / error-tracking dashboard | | ☐ |
| AI Tools Governance Hub record (edit rights) | | ☐ |
| RPA / utility hub record (edit rights) | | ☐ |
| Other (scheduler, admin panel, domain, etc.) | | ☐ |

## 4. Current status & known issues

The "if I disappeared tomorrow" summary — what's working, what's fragile, what you'd want someone to know before they touch anything.

- **Health right now:** ______________________________________________
- **Known issues / workarounds in place:** ______________________________________________
- **Anything fragile or about to break (expiring cert, deprecated API, dependency going EOL):** ______________________________________________

## 5. Pending / planned work

What the incoming owner should expect to pick up, roughly in priority order.

| Item | Priority | Notes |
|---|---|---|
| | | |

## 6. Cost commitments to be aware of

Pull these from `PROJECT.md`'s `## Business Case` section rather than re-deriving them — if they've drifted, fix the source, not just this copy.

- **Monthly recurring cost:** ______________________________________________ (as of: <date>)
- **Any contract, subscription, or commitment with a renewal/cancellation date:** ______________________________________________
- **Who approves a cost increase if usage grows:** ______________________________________________

## 7. Key stakeholders & who to ask what

| Question type | Ask |
|---|---|
| "Who owns this, formally?" | Chair / Governance Administrator — see `GOVERNANCE.md` Section 2 |
| "Is this still worth keeping?" | Business contact (above), or the Governance Administrator if that seat is vacant |
| "Is this still secure?" | Security/Cyber governance seat — see `GOVERNANCE.md` |
| "How does the data model work?" | Technical contact, or whoever is named in `docs/architecture.md` |
| "Who's responsible for where this runs?" | Infrastructure contact (above), or the Technical contact if that row is empty |
| "Who uses this and how much?" | AI Tools Governance Hub / RPA-utility hub record |

## 8. Transfer log

Append a row every time a **contact** (business, technical, or infrastructure) changes hands — this is the audit trail an auditor or a disputed-tier escalation (`GOVERNANCE.md` Section 3) will ask for. The Owner of record row in Section 1 does not get a row here: it stays the Governance Administrator seat regardless of which contact just changed — that's the entire point of separating the two.

| Date | Role (business / technical / infra) | Outgoing contact | Incoming contact | Confirmed by | Notes |
|---|---|---|---|---|---|
| | | | | | |

---

**Reminder to whoever is reading this because a contact just left:** start with Section 1 and Section 3 — get access secured first, understand the project second. Section 4 tells you where the bodies are buried. Loop in the Governance Administrator (Section 1, top row) if it's not already obvious who that is — they're the Owner of record and the fallback if nothing else in this file answers your question. If anything here is unclear or missing, that's a gap in *this* file to fix once you've taken over, not a reason to work around it silently — the next person deserves a better copy than you got.

# Ownership Transfer — `OWNERSHIP_TRANSFER.md` template

**What this is:** people are not eternal — owners change roles, change teams, and leave the company. This file is what makes a transfer take five business days (the SLA in `AI_PROJECT_GUIDELINES.md` Section 4) instead of five weeks of someone reverse-engineering the project from scratch. Required from **Tier 1 upward** (recommended at T1, required at T2/T3 — see the Requirements matrix in `AI_PROJECT_GUIDELINES.md` Section 3).

**Instructions to the AI assistant or the owner filling this in:**
- Copy this file into the project root as `OWNERSHIP_TRANSFER.md` and fill in every section — an empty section is worse than an honest "none" or "not yet."
- This is a **living document**, not a form filled in once at go-live. Update it whenever ownership changes, whenever access/credentials change, and at the same cadence as the project's own review reminder (`AI_PROJECT_GUIDELINES.md` Section 5) — not only when someone is actually about to leave.
- Never put an actual secret, password, or key value in this file. Point to *where* it lives (the vault path, the credential name) — this file is about findability, not custody.
- If you're the AI assistant maintaining a project, treat a stale `OWNERSHIP_TRANSFER.md` (referencing a person who already left, or a status paragraph months out of date) as a finding to raise with the current owner, the same way you'd flag an overdue security review.

---

## 1. Current ownership

| Role | Name | Since | Backup / secondary contact |
|---|---|---|---|
| Business owner | | | |
| Technical owner | | | |

> If business and technical owner are the same person (common below Tier 2), say so explicitly — don't leave the second row blank and ambiguous.

## 2. What this project is, in one paragraph

Write this as if explaining it to the person picking it up cold — assume they've read the project's name and nothing else.

> ______________________________________________

Link back to the full problem statement in `PROJECT.md` rather than duplicating it here; if the two drift apart, `PROJECT.md` is the source of truth.

## 3. Access & credentials checklist

List *what* needs to be re-provisioned to a new owner and *where* it lives — never the secret value itself.

| What | Where it lives (vault path / system / admin to ask) | Re-provisioned to incoming owner? |
|---|---|---|
| Repository access | | ☐ |
| Production/staging credentials | | ☐ |
| Third-party AI/API provider account | | ☐ |
| Database / data-store access | | ☐ |
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
| "Is this still worth keeping?" | Business owner (above), or their manager if the seat is vacant |
| "Is this still secure?" | Security/Cyber governance seat — see `GOVERNANCE.md` |
| "How does the data model work?" | Technical owner, or whoever is named in `docs/architecture.md` |
| "Who uses this and how much?" | AI Tools Governance Hub / RPA-utility hub record |

## 8. Transfer log

Append a row every time ownership actually changes hands — this is the audit trail an auditor or a disputed-tier escalation (`GOVERNANCE.md` Section 2) will ask for.

| Date | Outgoing owner | Incoming owner | Confirmed by | Notes |
|---|---|---|---|---|
| | | | | |

---

**Reminder to whoever is reading this because an owner just left:** start with Section 1 and Section 3 — get access secured first, understand the project second. Section 4 tells you where the bodies are buried. If anything here is unclear or missing, that's a gap in *this* file to fix once you've taken over, not a reason to work around it silently — the next person deserves a better copy than you got.

# Governance — who decides, and what happens if one person is out

**Purpose:** the rest of this kit is the *rules*. This file is the *body* that runs them — who's accountable for the kit itself, who signs off on a Tier 3 go-live, and who a builder escalates to when a classification is disputed or the one person in `CODEOWNERS` is unavailable. Until this file existed, all of that was implicitly one person. It still can be, in practice, for a while — but it has to be written down as a role with a backup, not a name.

---

## 1. The council

Four seats. Small on purpose — this is sized to prevent a single point of failure, not to create a committee.

| Seat | Holds | Decision rights |
|---|---|---|
| **Chair — Governance Administrator** | *[name — currently the sole `CODEOWNERS` entry]* | Owns the kit's content end to end. Required reviewer on every PR to this repo (Section 6). Casts the deciding vote on a tie. Approves or delegates every Tier 3 go-live sign-off. |
| **Security / Cyber seat** | *[TBD — assign]* | Co-approves any Tier 3 security sign-off (`AI_PROJECT_GUIDELINES.md` Section 4, `AI_Development_Standard` Section 5.1). Required co-reviewer on any kit change that loosens a security requirement (`CONTRIBUTING.md` already requires this — this seat is who that rule refers to). Named backup approver for the Chair. |
| **Legal / Privacy seat** | *[TBD — assign]* | Owns the calls the framework crosswalk in `PILLARS_COVERAGE.md` flags for review — EU AI Act risk classification, data-processing agreements for third-party AI providers, regulated-data handling. Required for any Tier 3 project touching personal, financial, or health data. |
| **Rotating engineering seat** | *[TBD — 6-month rotation]* | Filled by whoever is actively shipping the most Tier 2+ work that cycle. Keeps the council's decisions grounded in what builders actually hit, not just policy on paper. No standing veto — a voice and a vote, not a gate. |

A seat with no name assigned is not a decision-blocker for day-to-day Tier 0–2 work — it only matters the moment that seat's specific sign-off is needed (a Tier 3 go-live, a legal question, a security-requirement change). Assign the two `TBD` seats before the first Tier 3 project needs to close, not before.

---

## 2. What needs whom

| Decision | Required approval |
|---|---|
| Tier 0–1 classification, day-to-day building | No council involvement — self-classify or ask a department champion (`AI_ASSISTANT_USAGE_GUIDE.md`). |
| Tier 2 classification or go-live | Any one council seat can confirm; no full-council review needed. |
| Tier 3 go-live sign-off | Chair (or backup) **and** Security seat. Legal seat added if the project touches regulated data or a third-party AI provider. |
| Tier 3 remediation plan closure | All four seats certify their own share — Chair the plan as a whole, Engineering the technical items, Security the security items, Legal the data/regulatory items (`AI_PROJECT_AUDIT.md` Section 6) — not delegable to the auditing assistant alone. |
| A kit change to this repository | Chair approval always (`CODEOWNERS`). Security seat co-approves if the change loosens a security requirement (`CONTRIBUTING.md`, "Cosa NON fare"). |
| A disputed or edge-case tier | Whichever seat is asked first defaults to the *higher* candidate tier and loops in the Chair — same rule the kit already gives individual builders in `AI_PROJECT_GUIDELINES.md` Section 6, applied one level up. |
| Tie among the council | Chair casts the deciding vote. |

---

## 3. Continuity — the point of this file

If the Chair is unavailable for more than 5 business days (the same SLA the kit already sets for ownership transfer on a role change, `AI_PROJECT_GUIDELINES.md` Section 4):

1. The Security seat becomes the interim required reviewer on `CODEOWNERS`-gated PRs and interim Tier 3 sign-off authority.
2. Nothing needing only the Chair's ordinary review (a documentation fix, a typo patch) waits for this — the Security seat can merge it directly under the same standard.
3. On the Chair's return, a joint review of anything merged during the gap is a five-minute agenda item at the next council meeting (Section 4), not a re-approval requirement.

This is the concrete answer to the P0 gap this file exists to close: governance with one name in it is unauditable the moment that person is on leave, changes role, or leaves the company.

---

## 4. Cadence

- **Quarterly council review** (aligned with the Tier 3 audit cadence in `AI_PROJECT_GUIDELINES.md` Section 5): walk the Governance Hub's overdue-review list, confirm Tier 3 audits due that quarter happened, and clear any kit-change PR that's been open more than 30 days without a decision.
- **Ad hoc**: Tier 3 go-live sign-offs, security-requirement exceptions, and disputed classifications happen as they arise — they don't wait for the quarterly meeting.

---

## 5. Escalation path

```
Builder has a question
  → Tier 0–1: self-classify (00_START_HERE.md) or ask the department champion
  → Tier 2: any one council seat
  → Tier 3, disputed tier, or a security/legal exception: full council
```

The champions layer referenced above is the enablement track proposed alongside this file — see the kit's roadmap notes. Until it exists, Tier 0–1 questions go to whichever council seat is easiest to reach; the point of a champion is to keep that load off the council, not to gate it.

---

## 6. Relationship to `CODEOWNERS`

`.github/CODEOWNERS` is the mechanical gate GitHub enforces — it currently lists only the Chair. This file is the policy behind that gate: who else is on the council, what each seat actually decides, and who has authority when the Chair doesn't. When a seat changes hands, update **both** in the same PR — `CODEOWNERS` so the gate reflects reality, this file so the reasoning does.

---

## 7. Filling the open seats

Only the Chair is currently assigned. Assigning the Security and Legal seats is the first action item this file creates — see `CHANGELOG.md`. Until they're named, treat every row in Section 2 that references them as routed to the Chair by default, and say so out loud when it happens, the same way the kit asks builders to name an unclear tier call rather than resolve it silently.

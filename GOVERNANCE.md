# Governance — who decides, and what happens if one person is out

**Purpose:** the rest of this kit is the *rules*. This file is the *body* that runs them — who's accountable for the kit itself, who signs off on a Tier 3 go-live, and who a builder escalates to when a classification is disputed or the one person in `CODEOWNERS` is unavailable. Until this file existed, all of that was implicitly one person. It still can be, in practice, for a while — but it has to be written down as a role with a backup, not a name.

---

## 1. The council

Four seats. Small on purpose — this is sized to prevent a single point of failure, not to create a committee.

| Seat | Holds | Decision rights |
|---|---|---|
| **Chair — Governance Administrator** | *[name — currently the sole `CODEOWNERS` entry]* | Owns the kit's content end to end. Required reviewer on every PR to this repo (Section 7). Casts the deciding vote on a tie. Approves or delegates every Tier 3 go-live sign-off. Is the formal **Owner of record for every Tier 1+ project** (Section 2). |
| **Security / Cyber seat** | *[TBD — assign]* | Co-approves any Tier 3 security sign-off (`AI_PROJECT_GUIDELINES.md` Section 4, `AI_Development_Standard` Section 5.1). Required co-reviewer on any kit change that loosens a security requirement (`CONTRIBUTING.md` already requires this — this seat is who that rule refers to). Named backup approver for the Chair. |
| **Legal / Privacy seat** | *[TBD — assign]* | Owns the calls the framework crosswalk in `PILLARS_COVERAGE.md` flags for review — EU AI Act risk classification, data-processing agreements for third-party AI providers, regulated-data handling. Required for any Tier 3 project touching personal, financial, or health data. |
| **Rotating engineering seat** | *[TBD — 6-month rotation]* | Filled by whoever is actively shipping the most Tier 2+ work that cycle. Keeps the council's decisions grounded in what builders actually hit, not just policy on paper. No standing veto — a voice and a vote, not a gate. |

A seat with no name assigned is not a decision-blocker for day-to-day Tier 0–2 work — it only matters the moment that seat's specific sign-off is needed (a Tier 3 go-live, a legal question, a security-requirement change). Assign the two `TBD` seats before the first Tier 3 project needs to close, not before.

---

## 2. Project ownership above Tier 0

Two different things get called "ownership" in this kit, and conflating them is exactly what creates a bus-factor problem — so this section keeps them apart.

- **At T0**, there's nothing to separate: the author is the owner, full stop. It's their throwaway script; they carry whatever business case exists (usually none) themselves, and the rest of this section doesn't apply.
- **From T1 up**, the name the Hub records as **Owner** — the name an audit asks for, the name accountable if a project goes stale, unreviewed, or unexplained — is the **Chair / Governance Administrator seat**, not the department that asked for the project and not the engineer who built it.

**Why centralize it here instead of distributing it:** an individual business sponsor or engineer changes teams or leaves the company on their own schedule, with no obligation to this kit. A seat has a written continuity plan (Section 4) precisely so it never goes dark. Naming a person as "owner" and hoping the 5-business-day transfer SLA (`AI_PROJECT_GUIDELINES.md` Section 4) catches every departure recreates, in the one field meant to prevent it, the exact single point of failure this kit exists to remove everywhere else.

**What this does *not* mean:** the Governance Administrator does not personally build, fund, or run the project day to day. Three working roles still exist, still get named in `PROJECT.md` and kept current in `OWNERSHIP_TRANSFER.md`, and still do the actual work:

| Role | Does | Named in |
|---|---|---|
| **Business contact** | Whoever requested it, or whoever it serves — consulted on value, priority, and retire decisions | `PROJECT.md` Section 1, `OWNERSHIP_TRANSFER.md` Section 1 |
| **Technical contact** | Whoever builds and maintains it day to day | same |
| **Infrastructure contact** (from T2, wherever there's an actual environment to run) | Whoever owns the hosting/deploy environment it runs on | same |

None of these three is the accountable Owner of record. That's the point: a business contact can move teams, a technical contact can leave, an infrastructure contact can rotate — without ever reopening the question of who owns the project, because that answer was never a person to begin with.

**What the Governance Administrator actually does with this ownership:** the job is connective, not custodial. It's the one standing point of contact between the business contact, the technical contact, the infrastructure contact, and the stakeholder functions a project touches — Security and Legal/Compliance chief among them (Section 1). "Who do I even ask about this tool" gets one durable answer regardless of who's currently sitting in any of the other seats.

**Delegation, not devolution:** at scale, the Chair delegates day-to-day coordination — most often to the rotating engineering seat for technical questions, within its existing remit (Section 1) — the same way Tier 3 sign-off is already "approves *or delegates*." The delegate can change every six months by design; the name of record on the project does not change with them.

---

## 3. What needs whom

| Decision | Required approval |
|---|---|
| Tier 0–1 classification, day-to-day building | No council involvement — self-classify or ask a department champion (`AI_ASSISTANT_USAGE_GUIDE.md`). |
| Tier 2 classification or go-live | Any one council seat can confirm; no full-council review needed. |
| Tier 3 go-live sign-off | Chair (or backup) **and** Security seat. Legal seat added if the project touches regulated data or a third-party AI provider. |
| A kit change to this repository | Chair approval always (`CODEOWNERS`). Security seat co-approves if the change loosens a security requirement (`CONTRIBUTING.md`, "Cosa NON fare"). |
| A disputed or edge-case tier | Whichever seat is asked first defaults to the *higher* candidate tier and loops in the Chair — same rule the kit already gives individual builders in `AI_PROJECT_GUIDELINES.md` Section 6, applied one level up. |
| Tie among the council | Chair casts the deciding vote. |

---

## 4. Continuity — the point of this file

If the Chair is unavailable for more than 5 business days (the same SLA the kit already sets for a contact handoff on a role change, `AI_PROJECT_GUIDELINES.md` Section 4):

1. The Security seat becomes the interim required reviewer on `CODEOWNERS`-gated PRs, interim Tier 3 sign-off authority, and interim Owner of record for every Tier 1+ project (Section 2).
2. Nothing needing only the Chair's ordinary review (a documentation fix, a typo patch) waits for this — the Security seat can merge it directly under the same standard.
3. On the Chair's return, a joint review of anything merged during the gap is a five-minute agenda item at the next council meeting (Section 5), not a re-approval requirement.

This is the concrete answer to the P0 gap this file exists to close: governance with one name in it is unauditable the moment that person is on leave, changes role, or leaves the company. It's also why Section 2 puts project ownership on this seat rather than on individual people in the first place — the seat already has this continuity plan; an individual business or technical contact never did.

---

## 5. Cadence

- **Quarterly council review** (aligned with the Tier 3 audit cadence in `AI_PROJECT_GUIDELINES.md` Section 5): walk the Governance Hub's overdue-review list, confirm Tier 3 audits due that quarter happened, and clear any kit-change PR that's been open more than 30 days without a decision.
- **Ad hoc**: Tier 3 go-live sign-offs, security-requirement exceptions, and disputed classifications happen as they arise — they don't wait for the quarterly meeting.

---

## 6. Escalation path

```
Builder has a question
  → Tier 0–1: self-classify (00_START_HERE.md) or ask the department champion
  → Tier 2: any one council seat
  → Tier 3, disputed tier, or a security/legal exception: full council
```

The champions layer referenced above is the enablement track proposed alongside this file — see the kit's roadmap notes. Until it exists, Tier 0–1 questions go to whichever council seat is easiest to reach; the point of a champion is to keep that load off the council, not to gate it.

---

## 7. Relationship to `CODEOWNERS`

`.github/CODEOWNERS` is the mechanical gate GitHub enforces — it currently lists only the Chair. This file is the policy behind that gate: who else is on the council, what each seat actually decides, and who has authority when the Chair doesn't. When a seat changes hands, update **both** in the same PR — `CODEOWNERS` so the gate reflects reality, this file so the reasoning does.

---

## 8. Filling the open seats

Only the Chair is currently assigned. Assigning the Security and Legal seats is the first action item this file creates — see `CHANGELOG.md`. Until they're named, treat every row in Section 3 that references them as routed to the Chair by default, and say so out loud when it happens, the same way the kit asks builders to name an unclear tier call rather than resolve it silently.

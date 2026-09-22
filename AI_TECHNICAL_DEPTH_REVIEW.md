# AI Technical Depth Review — instructions for the AI assistant

**When to use this file:** as a companion to `AI_PROJECT_AUDIT.md`, on every audit from **Tier 1 up** — never skipped, but never the same depth twice. The governance audit checks conformity: owner, scope, security sign-off, documentation. This file is the one that actually reads code, and how far it goes scales with the tier exactly the way every other requirement in this kit does — a short spot-check at T1, a full enterprise-grade line-by-line pass at T3. T0 is the only tier this file doesn't apply to at all, for the same reason nothing else in the kit applies to it: a personal, throwaway script doesn't carry the risk that justifies the effort.

This file assumes `AI_PROJECT_AUDIT.md` has already run (or is running) on the same tool and confirmed its tier — that tier is what Section 2 below uses to decide how deep to go. If nobody has confirmed the tier yet, stop and run that file first.

---

## 1. What this is, and what it isn't

`AI_PROJECT_AUDIT.md` Section 3 says plainly: the governance audit is not a rebuild, doesn't read code line by line, and stays a conformity check. That's deliberate — it has to run the same way for every tool, from a T0 script to a T3 platform.

This file is the opposite kind of work:
- **It reads real code** — not a checklist, not a policy document, not "should be fine." How much of it, and how closely, is what Section 2 sets.
- **It looks for how the tool would behave under real load**, not just whether it currently works for the people using it today.
- **It proposes concrete improvements** — a shorter query path, a cache that should exist and doesn't, an API pattern that won't survive ten times the current traffic — not just "this is a gap."

What it still isn't:
- **Not a rebuild either.** Same rule as the governance audit: findings and a proposed direction go in the report; applying the fix is separate, deliberate work — see `AI_PROJECT_AUDIT.md` Section 6 remediation, which this file feeds into.
- **Not a replacement for the governance audit.** A tool can pass this review with excellent architecture and still fail the governance audit on ownership or security sign-off, and vice versa. Run both; report both.
- **Not the same depth at every tier.** Reading an entire T3 codebase line by line the same way you'd spot-check a T1 tool is neither realistic nor the point — Section 2 is what makes this proportional instead of a blunt yes/no.

## 2. How deep — by tier

This is the section that makes the review scale the way the rest of the kit does (`AI_PROJECT_STRUCTURE.md`'s own tier-tagged requirements are the same pattern). Never skip a tier's minimum, and never assume T3's full depth is owed to a T1 tool just because "more thorough" sounds safer — that's not proportionate, it's just expensive.

| Tier | Depth | What you actually do |
|---|---|---|
| **T0** | Not applicable | No technical review — same reasoning as every other requirement this kit skips at T0. |
| **T1** | Spot-check | Read only the one or two paths that matter most (Section 3, scaled down to a quick pick, not a full scoping exercise). Look only for what would actually hurt at this tier's real scale — an obviously unbounded query, a write endpoint with no validation, a secret leaking into a log — not the full five-area sweep in Section 4. A short paragraph in the governance audit report is enough; no separate document. |
| **T2** | Scoped review | Full method: scope with Section 3, then walk every area in Section 4 — but only on the modules that actually carry load or consequence, not the whole codebase. Produces the separate report (Section 6), on the same 12-month cadence as the T2 governance audit. |
| **T3** | Full enterprise-grade review | Same method, no shortcuts and no sampling: every area in Section 4, on every module that carries real load or real consequence, with the file:line evidence discipline in full (Section 5). Mandatory, on the same quarterly cadence as the T3 governance audit — this is the tier the kit itself calls critical, so it's the one tier where a light pass isn't good enough. |

The depth only ever moves up when the tier itself moves up on re-audit (same rule as the tier, `AI_PROJECT_AUDIT.md` Section 2 point 2) — never quietly run a lighter pass than the confirmed tier calls for because the code looked fine at a glance, and never skip straight past T1/T2's real (if lighter) obligation just because it isn't T3.

## 3. Where to start — scope before you read code

For T2 and T3, don't open the repository and start at the top. Ask the owner (or check the Hub record, if it's populated) which parts of the tool actually carry the load:
- Which screen, endpoint, or job runs the most often, or is hit by the most users at once?
- Which part of the tool is business-critical if it's slow or wrong — not just technically complex?
- What's the realistic scale target — how many concurrent users, requests per minute, or records does this need to handle in the next year, not just today?

Start reading there. A T3 tool that's mostly idle outside one dashboard doesn't need every controller read line by line — it needs that one dashboard read properly. Say out loud which modules you picked and why, so the owner can correct you before the session goes into the wrong ones. For T1, skip the exercise — just ask which one path matters most and read that.

## 4. What to look for

Not a fixed checklist — this is engineering judgment, and every tool is different. At T1, use this only to recognize the most damaging pattern in the one path you're reading; at T2/T3, walk every area on every module Section 3 selected.

- **Query patterns.** Is there an N+1 query hiding behind a loop? Is a query missing an index it clearly needs? Does a list endpoint fetch every row before filtering in application code instead of in the query itself?
- **Database access patterns.** Is the same data fetched more than once in one request? Is a write path doing more round-trips than it needs? Are transactions used where they should be, and avoided where they shouldn't?
- **Caching.** Is there a cache where one is obviously missing — a value recomputed every request that barely changes? Where a cache exists, is invalidation correct, or does it risk serving stale data past the point that matters?
- **API maturity.** Versioning, pagination on anything that can grow unbounded, rate limiting, idempotency on anything that writes, consistent error shapes — the difference between an endpoint that was fine for ten users and one that's fine for a thousand.
- **Scalability under the real target.** Given the scale target from Section 3, walk the hot path and say plainly where it would actually break first — not hypothetically, at what number.
- **Whatever else is specific to this tool.** These five are the most common; if the tool's real risk is somewhere else (a background job with no retry, a websocket connection that never closes), follow that instead of forcing it into the list above.

## 5. Evidence and findings format

Every finding cites the exact file and line — `src/services/matrix.ts:142`, not "the Matrix comparison module." A finding with no line reference is an impression, not a finding, and doesn't belong in the report — at any tier.

For each finding, give three things:
1. **What's there now** — quoted or described precisely enough that the owner can find it without searching.
2. **Why it matters at this tool's actual scale** — tie it to the scale target from Section 3, not a generic best-practice statement.
3. **A concrete direction for the fix** — not "optimize this query," but what the shorter path actually looks like: which index, which cache key, which pagination pattern. You're not applying it (Section 1), but a finding without a proposed direction is half the work.

## 6. Producing the report

For T2 and T3, write it as a real file (`TECHNICAL_REVIEW_<data>.md`), alongside the governance audit's own report — same discipline, different content. For T1, fold the short paragraph from Section 2 into the governance audit report instead of producing a separate file.

```markdown
# Revisione tecnica di profondità — <nome progetto>

**Tier:** T<2-3>
**Data:** <data>
**Reviewer:** <assistente AI>, confermato da <nome umano — il seat Engineering, non solo l'owner>
**Moduli scelti e perché:** <elenco, con la giustificazione di Sezione 3>
**Target di scala assunto:** <es. 1.500 utenti concorrenti, N richieste/minuto>

## Findings

| # | Area | File:riga | Cosa c'è ora | Perché conta a questa scala | Direzione proposta | Priorità |
|---|---|---|---|---|---|---|
| 1 | Query / Cache / API / Scalabilità / Altro | ... | ... | ... | ... | Alta / Media / Bassa |

## Cosa non è stato letto, e perché
- <modulo/area esclusa> — <motivo: basso traffico, non critico, fuori scope concordato>
```

## 7. Feeding into remediation

Findings here that need fixing go into the **same** remediation plan as the governance audit (`AI_PROJECT_AUDIT.md` Section 6) — tag each one `[Tecnico]` so it's clear which review found it, but track everything in one place. Don't let a second, parallel remediation file exist that nobody's actually working from. The T3-only "Garanzia dei chair" in that section applies to technical findings the same as governance ones — Engineering signs off on the technical items specifically.

## 8. Where this fits

`AI_PROJECT_AUDIT.md` Section 2 triggers this file on every audit from T1 up — see that file's step on this, and Section 2 above for how deep to go. Skipping it below T3 isn't "not required yet"; it's a gap in the audit's own proportionality, the same as skipping a T1 tool's security hygiene check because it "felt like just a script."

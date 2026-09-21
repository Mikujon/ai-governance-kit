# AI Technical Depth Review — instructions for the AI assistant

**When to use this file:** as a companion to `AI_PROJECT_AUDIT.md`, once that audit confirms a tool is **Tier 3**. Mandatory at T3, never optional — the governance audit tells you the tool is critical; this file is what actually checks whether the code underneath can carry that weight. Available on request at T2 for a tool the owner already suspects has technical debt, but not required there.

This file assumes `AI_PROJECT_AUDIT.md` has already run (or is running) on the same tool — this is not a substitute for it, and it never runs alone. If nobody has confirmed the tier yet, stop and run that file first.

---

## 1. What this is, and what it isn't

`AI_PROJECT_AUDIT.md` Section 3 says plainly: the governance audit is not a rebuild, doesn't read code line by line, and stays a conformity check — owner, scope, security sign-off, documentation. That's deliberate: it has to run fast and the same way for every tool, from a T0 script to a T3 platform.

This file is the opposite kind of work, and needs its own separate run:
- **It reads real code, line by line** — not a checklist, not a policy document, not "should be fine."
- **It looks for how the tool would behave under real load**, not just whether it currently works for the people using it today.
- **It proposes concrete improvements** — a shorter query path, a cache that should exist and doesn't, an API pattern that won't survive ten times the current traffic — not just "this is a gap."

What it still isn't:
- **Not a rebuild either.** Same rule as the governance audit: findings and a proposed direction go in the report; applying the fix is separate, deliberate work — see `AI_PROJECT_AUDIT.md` Section 6 remediation, which this file feeds into.
- **Not a replacement for the governance audit.** A tool can pass this review with excellent architecture and still fail the governance audit on ownership or security sign-off, and vice versa. Run both; report both; never let a good technical review stand in for a missing security approval, or a clean governance audit stand in for code that won't survive real load.
- **Not exhaustive coverage of every file.** Reading an entire T3 codebase line by line is neither realistic nor useful — see Section 2 for how to scope it instead.

## 2. Where to start — scope before you read code

Don't open the repository and start at the top. Ask the owner (or check the Hub record, if it's populated) which parts of the tool actually carry the load:
- Which screen, endpoint, or job runs the most often, or is hit by the most users at once?
- Which part of the tool is business-critical if it's slow or wrong — not just technically complex?
- What's the realistic scale target — how many concurrent users, requests per minute, or records does this need to handle in the next year, not just today?

Start reading there. A T3 tool that's mostly idle outside one dashboard doesn't need every controller read line by line — it needs that one dashboard read properly. Say out loud which modules you picked and why, so the owner can correct you before the session goes into the wrong ones.

## 3. What to look for

Not a fixed checklist — this is engineering judgment, and every tool is different. These are the areas that matter most, with the kind of question to ask in each:

- **Query patterns.** Is there an N+1 query hiding behind a loop? Is a query missing an index it clearly needs? Does a list endpoint fetch every row before filtering in application code instead of in the query itself?
- **Database access patterns.** Is the same data fetched more than once in one request? Is a write path doing more round-trips than it needs? Are transactions used where they should be, and avoided where they shouldn't?
- **Caching.** Is there a cache where one is obviously missing — a value recomputed every request that barely changes? Where a cache exists, is invalidation correct, or does it risk serving stale data past the point that matters?
- **API maturity.** Versioning, pagination on anything that can grow unbounded, rate limiting, idempotency on anything that writes, consistent error shapes — the difference between an endpoint that was fine for ten users and one that's fine for a thousand.
- **Scalability under the real target.** Given the scale target from Section 2, walk the hot path and say plainly where it would actually break first — not hypothetically, at what number.
- **Whatever else is specific to this tool.** These five are the most common; if the tool's real risk is somewhere else (a background job with no retry, a websocket connection that never closes), follow that instead of forcing it into the list above.

## 4. Evidence and findings format

Every finding cites the exact file and line — `src/services/matrix.ts:142`, not "the Matrix comparison module." A finding with no line reference is an impression, not a finding, and doesn't belong in the report.

For each finding, give three things:
1. **What's there now** — quoted or described precisely enough that the owner can find it without searching.
2. **Why it matters at this tool's actual scale** — tie it to the scale target from Section 2, not a generic best-practice statement.
3. **A concrete direction for the fix** — not "optimize this query," but what the shorter path actually looks like: which index, which cache key, which pagination pattern. You're not applying it (Section 1), but a finding without a proposed direction is half the work.

## 5. Producing the report

Write it as a real file (`TECHNICAL_REVIEW_<data>.md`), alongside the governance audit's own report — same discipline, different content:

```markdown
# Revisione tecnica di profondità — <nome progetto>

**Data:** <data>
**Reviewer:** <assistente AI>, confermato da <nome umano — il seat Engineering, non solo l'owner>
**Moduli scelti e perché:** <elenco, con la giustificazione di Sezione 2>
**Target di scala assunto:** <es. 1.500 utenti concorrenti, N richieste/minuto>

## Findings

| # | Area | File:riga | Cosa c'è ora | Perché conta a questa scala | Direzione proposta | Priorità |
|---|---|---|---|---|---|---|
| 1 | Query / Cache / API / Scalabilità / Altro | ... | ... | ... | ... | Alta / Media / Bassa |

## Cosa non è stato letto, e perché
- <modulo/area esclusa> — <motivo: basso traffico, non critico, fuori scope concordato>
```

## 6. Feeding into remediation

Findings here that need fixing go into the **same** remediation plan as the governance audit (`AI_PROJECT_AUDIT.md` Section 6) — tag each one `[Tecnico]` so it's clear which review found it, but track everything in one place. Don't let a second, parallel remediation file exist that nobody's actually working from.

## 7. Where this fits

`AI_PROJECT_AUDIT.md` Section 2 triggers this file automatically the moment a tool is confirmed T3 — see that file's step on this. Skipping it isn't a lighter T3 audit; it means the tool most likely to hurt at scale is the one tier where nobody actually checked whether it would.

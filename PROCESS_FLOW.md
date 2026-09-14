# Process flow — the kit end to end

Every other file in this kit describes one piece of the process in prose: how to classify (`AI_INTAKE_ASSESSMENT.md`), what's required at each tier (`AI_PROJECT_GUIDELINES.md`), who decides what (`GOVERNANCE.md`). None of them show the whole shape at once. This file is that map — four diagrams, one per phase of a project's life, each pointing back to the file that actually defines the step. If a diagram and a file disagree, the file is right and this page has drifted — open an issue/PR (`CONTRIBUTING.md`).

---

## 1. Intake — from idea to a starter in hand

```mermaid
flowchart LR
    A(["Idea or need"]) --> B["Run the interview<br/>AI_INTAKE_ASSESSMENT.md §2<br/>(10 questions)"]
    B --> C["Apply tier logic<br/>§3 — highest tier wins,<br/>never averaged"]
    C --> D["Run the business case<br/>§4 — B1-B6, scaled by tier<br/>(skipped at T0)"]
    D --> E["Record PROJECT.md<br/>§5 — Classification + Business Case"]
    E --> F{{"Resulting tier"}}
    F -->|T0| G0["PROJECT_STARTER_T0_PERSONAL.md"]
    F -->|T1| G1["PROJECT_STARTER_T1_BASIC.md"]
    F -->|T2| G2["PROJECT_STARTER_T2_STANDARD.md"]
    F -->|T3| G3["PROJECT_STARTER_T3_CRITICAL.md<br/>+ read AI_PROJECT_STRUCTURE.md in full"]
```

**Where this runs:** the whole lane above is one conversation — `AI_INTAKE_ASSESSMENT.md` Section 1's process, or the same thing invoked in one shot via `.claude/skills/start-ai-project/` on Claude Code. Nobody should be classifying a project and separately hunting for the business-case questions; they're the same interview.

**The one hard rule this diagram hides:** if the project's scope changes materially mid-build (new data source, new users, new integration), you re-enter at box **C** — re-run the tier logic, and say plainly if the tier goes up. It never goes down silently.

---

## 2. Build to go-live — what gates what, by tier

The requirements differ by tier, but the *shape* of the gate is the same everywhere: nothing here is a step you do and forget, it's a condition that must be true before the arrow to "Go-live" is allowed to fire.

```mermaid
flowchart TD
    subgraph T0["T0 — Personal"]
        direction TB
        T0a["Build"] --> T0b["No secret hardcoded +<br/>sensitive-data check"] --> T0c(["Go-live<br/>(no gate — just build it)"])
    end

    subgraph T1["T1 — Basic"]
        direction TB
        T1a["Build"] --> T1b["Owner named +<br/>business case (light) +<br/>OWNERSHIP_TRANSFER.md (recommended)"] --> T1c(["Go-live<br/>(no formal sign-off required)"])
    end

    subgraph T2["T2 — Standard"]
        direction TB
        T2a["Build"] --> T2b["Full business case +<br/>OWNERSHIP_TRANSFER.md +<br/>RACI (recommended)"]
        T2b --> T2c{"Business case<br/>presented?<br/>(recommended)"}
        T2c --> T2d["Hub + RPA/utility hub<br/>registration"] --> T2e(["Go-live"])
    end

    subgraph T3["T3 — Critical"]
        direction TB
        T3a["Build"] --> T3b["Full business case +<br/>OWNERSHIP_TRANSFER.md +<br/>RACI (required)"]
        T3b --> T3c["Business case presented to<br/>business owner's manager or<br/>governance seat — MANDATORY GATE"]
        T3c --> T3d["Security sign-off + pen test +<br/>prompt-injection review +<br/>DPA if data leaves the company"]
        T3d --> T3e["Hub + RPA/utility hub<br/>registration"] --> T3f(["Go-live"])
    end
```

**Read this as:** every box that says "MANDATORY GATE" or names an approver is a place where the assistant building the project should stop and get a human answer, not assume one. `PROJECT_STARTER_T3_CRITICAL.md` Section 4 is the literal checklist behind the T3 lane; `AI_PROJECT_GUIDELINES.md` Section 3 is the matrix behind all four.

---

## 3. After go-live — the loop nothing above shows

A project doesn't end at go-live. This is the part most governance write-ups skip, and it's the part `PILLARS_COVERAGE.md` Pillars 5–7 exist to force.

```mermaid
flowchart TD
    A(["Go-live"]) --> B["Registered in Governance Hub<br/>+ RPA/utility hub (T2+)"]
    B --> C["Review reminder set<br/>12 months (T2) / quarterly (T3)"]
    C --> D{"Reminder fires"}
    D --> E["Cyber check: still secure?"]
    D --> F["Utility check: still worth the cost?<br/>(re-check the Business Case numbers)"]
    E --> G{"Still fine?"}
    F --> G
    G -->|Yes| C
    G -->|No — cost changed,<br/>risk changed, unused| H["Re-open the Business Case<br/>cost/benefit call"]
    H --> I{"Fix, retire, or<br/>escalate?"}
    I -->|Fix| C
    I -->|Retire| Z(["Decommission"])
    I -->|Escalate| J["Governance council<br/>GOVERNANCE.md §2"]

    K["Owner changes role<br/>or leaves — any time"] -.-> L["Open OWNERSHIP_TRANSFER.md<br/>Sections 1 & 3 first:<br/>secure access, then understand"]
    L --> M["5-business-day SLA<br/>AI_PROJECT_GUIDELINES.md §4"]
    M --> N["Log the transfer<br/>OWNERSHIP_TRANSFER.md §8"]
    N -.-> C
```

**Two loops, one file each:** the top loop (review cadence) lives in `AI_PROJECT_GUIDELINES.md` Section 5; the ownership loop (dashed lines — it can trigger at any point, not on a schedule) lives entirely in `OWNERSHIP_TRANSFER.md` itself. They're drawn separately because they fire on different triggers, but they read the same underlying record: a stale `OWNERSHIP_TRANSFER.md` is exactly as much of a finding as a missed review.

---

## 4. Who decides — the escalation flow

This is `GOVERNANCE.md` redrawn as a flow instead of a table, for the moments a builder needs "who do I actually ask" faster than reading Section 2 of that file.

```mermaid
flowchart TD
    A(["Question or sign-off needed"]) --> B{{"What tier?"}}
    B -->|T0-T1| C["Self-classify, or ask the<br/>department champion"]
    B -->|T2| D["Any one council seat confirms —<br/>no full-council review needed"]
    B -->|T3 go-live| E["Chair (or backup) +<br/>Security seat —<br/>+ Legal if regulated data"]
    B -->|Disputed tier<br/>or edge case| F["Whichever seat is asked first<br/>defaults to the HIGHER candidate<br/>tier and loops in the Chair"]
    B -->|Kit change<br/>(a PR to this repo)| G["Chair approval always +<br/>Security co-approves if the<br/>change loosens a security rule"]

    C --> H(["Resolved"])
    D --> H
    E --> H
    F --> H
    G --> H

    I["Chair unavailable<br/>> 5 business days"] -.-> J["Security seat becomes<br/>interim Chair for<br/>CODEOWNERS + T3 sign-off"]
    J -.-> K["Joint review of anything<br/>merged during the gap,<br/>at next council meeting"]
```

The four council seats (Chair, Security/Cyber, Legal/Privacy, rotating engineering) and exactly what each one decides are defined in `GOVERNANCE.md` Section 1 — this diagram is a routing table to that file, not a replacement for it.

---

## Keeping this file honest

This page is descriptive, not authoritative — every box names the file that actually governs it, and that file wins on any conflict. Whoever proposes a kit change that alters a gate, a tier, or a council decision (`CONTRIBUTING.md`) should check whether it also moves a box here, the same way a matrix change requires a matching starter update. A flow diagram nobody updates becomes the exact kind of stale documentation this kit exists to prevent (`00_START_HERE.md`, "Why this exists").

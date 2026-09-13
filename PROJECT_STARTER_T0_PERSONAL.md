# Project Starter — Tier 0: Personal / Throwaway

**Use this when:** a one-off script, a quick chart, a personal report generator, or a prompt template — run by the author only, occasionally, with no persistent shared data. If any of that stops being true (someone else starts running it, it starts storing data, it starts calling a paid API on a schedule), move up to `PROJECT_STARTER_T1_BASIC.md`.

**Instructions to the AI assistant:** build exactly what's asked, applying only the requirements below. Don't add a database, a framework, tests, or a folder structure the user didn't ask for — that's the point of this tier.

---

## 1. Problem statement (one line)

> _What is this for?_ ______________________________________________

## 2. Structure

```
project/
├── README.md          # optional, but write one line about what this does
└── <your script / notebook>
```

Nothing more is required. If it grows past a single file, that's a signal to move to Tier 1.

## 3. Stack

Whatever is fastest for the author — a Python or TypeScript script, a notebook, a spreadsheet formula. No framework, database, or container required.

## 4. Requirements

- [ ] No secret (API key, password, token) is hardcoded in the script — use an environment variable even for a throwaway script.
- [ ] Before touching any personal, customer, financial or health data, pause and ask whether it should really be here — if in doubt, treat it as sensitive.

That's the complete list for this tier.

## 5. Compliance checklist (for later reference)

| # | Requirement | Done? |
|---|---|---|
| 1 | Problem statement written (Section 1) | ☐ |
| 2 | No hardcoded secrets | ☐ |
| 3 | Sensitive-data check done before use | ☐ |

No Hub registration and no formal audit apply at this tier. If this tool is still around in a few months and others have started relying on it, reclassify it using `00_START_HERE.md` — most Tier 0 tools that survive become Tier 1.

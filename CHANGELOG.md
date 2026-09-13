# Changelog

Tutte le modifiche rilevanti a questo kit sono elencate qui. Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.0.0/); il versioning segue [Semantic Versioning](https://semver.org/lang/it/).

## [1.1.0] — 2026-09-13

### Aggiunto
- `PILLARS_COVERAGE.md` — mappa esplicita dei 7 requisiti fondativi (approvazione security/cyber, architettura dati condivisa, wiki di processo, scope/problem statement, owner, reminder automatico, iscrizione all'hub RPA/utility) verso il file/sezione che li implementa e verso il campo del Governance Hub che li rende visibili.

### Modificato
- `00_START_HERE.md` — aggiunta una sezione "Why this exists" e una sezione "This covers more than 'AI projects'" con esempi concreti (Sheets/Apps Script, Power Automate, Zapier/Make, bot RPA) tier per tier, per evitare che automazioni "senza codice" saltino la classificazione.
- `AI_INTAKE_ASSESSMENT.md` — aggiunta un'istruzione "Before you start" che impedisce all'assistente AI di accettare "è solo uno script/uno sheet" come classificazione automatica a Tier 0 senza le domande di verifica; aggiunta la riga `Owner` al blocco di registrazione della classificazione (Sezione 4).
- `AI_PROJECT_GUIDELINES.md` — la colonna "Typical example" della tabella dei tier ora include esempi no-code/low-code; aggiunta una riga alla Sezione 6 che chiarisce che il tier non dipende dal linguaggio/piattaforma con cui lo strumento è costruito.
- `README.md` — aggiunta una riga "Perché esiste" e il collegamento a `PILLARS_COVERAGE.md`.
- `.github/workflows/validate-kit.yml` — `PILLARS_COVERAGE.md` aggiunto ai file richiesti.

## [1.0.0] — 2026-09-13

### Aggiunto
- `00_START_HERE.md` — indice e le due modalità d'uso (intervista AI / autoclassificazione).
- `AI_INTAKE_ASSESSMENT.md` — script di intervista e classificazione per l'assistente AI.
- `PROJECT_STARTER_T0_PERSONAL.md`, `PROJECT_STARTER_T1_BASIC.md`, `PROJECT_STARTER_T2_STANDARD.md`, `PROJECT_STARTER_T3_CRITICAL.md` — starter per ciascun tier.
- `AI_PROJECT_GUIDELINES.md` — modello di classificazione e matrice dei requisiti.
- `AI_PROJECT_STRUCTURE.md` — standard tecnico (stack, ReBAC, dati, design, CI/CD).
- `reference/AI_Development_Standard.docx` — policy completa e checklist di audit a 42 punti per il Tier 3.
- `reference/Guida_Uso_Kit_Governance_AI.docx` — guida utente passo-passo.

### Da fare in una prossima versione
- Confermare piattaforma git definitiva e sostituire i placeholder `wearefiber/ai-governance-kit` con l'URL reale, se diverso.
- Sostituire il team placeholder in `.github/CODEOWNERS` con il team/utente reale del Governance Administrator.
- Impostare questo repository come *template repository* nelle impostazioni della piattaforma, per lo scaffolding diretto di nuovi progetti.

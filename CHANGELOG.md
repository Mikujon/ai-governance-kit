# Changelog

Tutte le modifiche rilevanti a questo kit sono elencate qui. Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.0.0/); il versioning segue [Semantic Versioning](https://semver.org/lang/it/).

## [Unreleased]

### Corretto
- `README.md`, `00_START_HERE.md` — i comandi di clone e l'URL raw puntavano ancora al tag `v1.1.0`; ora puntano a `v1.3.0`, l'ultima versione disponibile.
- `AI_INTAKE_ASSESSMENT.md` — il comando di clone suggerito all'assistente AI puntava ancora al tag `v1.0.0`; ora punta a `v1.3.0`.

## [1.3.0] — 2026-09-14

### Aggiunto
- `GOVERNANCE.md` — il consiglio di governance a 4 posti (Chair/Governance Administrator, Security/Cyber, Legal/Privacy, seat rotante di engineering), cosa richiede l'approvazione di chi, il piano di continuità se il Chair non è disponibile (SLA 5 giorni lavorativi, allineato a quello già usato per il trasferimento di ownership), la cadenza trimestrale, e il percorso di escalation. Risolve il single-point-of-failure della governance (un solo indirizzo in `CODEOWNERS`).
- `PILLARS_COVERAGE.md` — nuova Sezione 8, "Framework crosswalk": tabella che mappa T0–T3 verso le funzioni di NIST AI RMF (Govern/Map/Measure/Manage), le classi di rischio dell'EU AI Act, e la pertinenza rispetto a ISO/IEC 42001. Mapping indicativo, non una classificazione legale — vedi la nota a piè di tabella.

### Modificato
- `PILLARS_COVERAGE.md` — sostituito il riferimento abbreviato "§N" con "Section N" in tutto il file, per leggibilità da parte di chi non è tecnico.
- `README.md`, `00_START_HERE.md` — aggiunta `GOVERNANCE.md` alla tabella dei file del kit.
- `.github/workflows/validate-kit.yml` — `GOVERNANCE.md` aggiunto ai file richiesti.
- `.github/CODEOWNERS` — aggiunto un commento che rimanda a `GOVERNANCE.md` per il resto del consiglio.

### Da fare
- Assegnare i due seat ancora `TBD` in `GOVERNANCE.md` (Security/Cyber, Legal/Privacy) — non bloccante per il lavoro Tier 0–2, ma necessario prima che il primo progetto Tier 3 debba chiudere.

## [1.2.0] — 2026-09-13

### Aggiunto
- `AI_ASSISTANT_USAGE_GUIDE.md` — guida pratica per l'uso quotidiano di Claude Code / Codex CLI: installazione (entrambe le piattaforme), scelta della modalità di autonomia in base al tier del progetto, tecniche verificate per ridurre il consumo di token/costi, configurazione consigliata (`settings.json` / `config.toml`), e una sezione non negoziabile su cosa l'assistente non deve mai poter toccare (credenziali di produzione, server/dati sensibili, modalità unattended fuori da un container, azioni distruttive automatiche). Contenuto verificato contro la documentazione ufficiale di entrambi i fornitori al momento della stesura.
- Collegato da `00_START_HERE.md`, `README.md` e richiamato in `AI_INTAKE_ASSESSMENT.md` come lettura preliminare a qualunque tier.
- Aggiunto ai file richiesti in `.github/workflows/validate-kit.yml`.

## [1.1.1] — 2026-09-13

### Corretto
- `README.md`, `00_START_HERE.md` — i comandi di clone e l'URL raw puntavano ancora al tag `v1.0.0`; ora puntano a `v1.1.0`, l'ultima versione disponibile al momento del rilascio di questa patch.

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

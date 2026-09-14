# Changelog

Tutte le modifiche rilevanti a questo kit sono elencate qui. Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.0.0/); il versioning segue [Semantic Versioning](https://semver.org/lang/it/).

## [1.4.0] — 2026-09-14

### Aggiunto
- `GOVERNANCE.md` — nuova Sezione 2 "Project ownership above Tier 0": a T0 l'autore resta owner; **da T1 in su, l'Owner of record diventa il seat Chair/Governance Administrator**, non un dipendente. Tre ruoli operativi separati (business contact, technical contact, infrastructure contact da T2) continuano a fare il lavoro e vengono tracciati in `PROJECT.md`/`OWNERSHIP_TRANSFER.md`, ma non sono loro l'owner accountable. Il seat è il punto di collegamento tra business, tecnico e stakeholder (Security, Legal/Compliance) — non fa il lavoro, risponde a "chi devo chiedere" in modo stabile anche quando le persone sotto cambiano. Sezioni successive rinumerate (3–8).
- `AI_INTAKE_ASSESSMENT.md` — nuova Sezione 4 "The business case": sei domande (B1–B6) su perché il progetto serve, beneficio atteso, effort di sviluppo, costo una tantum, **costo ricorrente mensile** e il confronto costo/beneficio — scalate per tier (assente a T0, leggera a T1, completa a T2/T3). A T3 il business case deve essere presentato a voce al manager del business contact o al seat di governance competente prima del go-live, non solo scritto in `PROJECT.md`. Il blocco di classificazione (ora Sezione 5) registra Owner of record + i tre contact, non più un singolo `Owner`.
- `OWNERSHIP_TRANSFER_TEMPLATE.md` — nuovo file: documento di passaggio vivo per i **contact** (checklist accessi, stato corrente, lavoro in sospeso, contatti chiave, impegni di costo, log dei passaggi) — l'Owner of record non cambia mai in questo file. Obbligatorio da T2 (raccomandato a T1). Risponde direttamente al punto debole "le persone non sono eterne": centralizzando l'ownership formale sul seat di governance (che ha già un piano di continuità), un contact che cambia diventa un passaggio da loggare, non un'emergenza di governance.
- `.claude/skills/start-ai-project/SKILL.md` — skill Claude Code che esegue l'intero flusso di intake (intervista → tier → business case → starter → ownership transfer) con un solo comando (`/start-ai-project`), invece di dover allegare i file e incollare il prompt "Option A" a mano. Documentato come "Option A, fully automatic" in `00_START_HERE.md`.
- `PROCESS_FLOW.md` — l'intero kit ridisegnato come cinque diagrammi Mermaid: (1) intake → tier → starter, (2) gate di build per tier (T0–T3), (3) **il modello di ownership sopra il Tier 0** — un hub (l'Owner of record) e spoke rotanti (i contact + i seat stakeholder), (4) il ciclo dopo il go-live — revisione periodica e passaggio di contact, disegnati come due loop separati che condividono lo stesso record, (5) il percorso di escalation di `GOVERNANCE.md` come flow invece che come tabella. Ogni riquadro rimanda al file che governa davvero quel passo. Linkato da `00_START_HERE.md` come punto di ingresso alternativo per chi preferisce vedere la forma intera prima di leggere file per file.

### Modificato
- `AI_PROJECT_GUIDELINES.md` — Sezione 4 "Ownership" riscritta attorno al nuovo modello (Owner of record = Governance Administrator da T1, contact business/tecnico/infrastruttura come ruoli operativi separati); nuove righe nella matrice dei requisiti (Sezione 3) per Owner of record, i tre contact, business case, business case presentato per il sign-off, `OWNERSHIP_TRANSFER.md` mantenuto aggiornato; riga "Ownership-transfer SLA" rinominata "Contact-handoff SLA". Nuova sotto-sezione "Business case & cost/benefit" in Sezione 4.
- `PROJECT_STARTER_T1_BASIC.md`, `PROJECT_STARTER_T2_STANDARD.md`, `PROJECT_STARTER_T3_CRITICAL.md` — i campi "Business owner / Technical owner" sostituiti da "Owner of record" (fisso: Governance Administrator) + business/technical/infrastructure contact; aggiunti i campi di business case alla Sezione 1 (leggeri a T1, completi a T2/T3), un requisito su `OWNERSHIP_TRANSFER.md`, e le relative righe nella checklist di conformità. A T3, business case e ownership transfer aggiunti ai gate item "Before go-live"; nota sulla colonna "Accountable" della RACI (normalmente il Governance Administrator, non un contact).
- `AI_PROJECT_STRUCTURE.md` — corretto un possibile fraintendimento: la relazione ReBAC `#owner` su un oggetto tecnico (controllo di accesso) non è l'Owner of record di governance — è il technical contact con diritti di amministrazione sull'oggetto. Commenti e riferimenti aggiornati di conseguenza.
- `PILLARS_COVERAGE.md` — Pilastro 4 (scope), Pilastro 5 (owner) e Pilastro 7 (hub/risparmi) riscritti attorno al nuovo modello owner-of-record/contact; `OWNERSHIP_TRANSFER.md` descritto come ciò che rende la SLA di 5 giorni un passaggio loggato invece che un vuoto di governance da colmare.
- `00_START_HERE.md`, `README.md` — documentata la nuova modalità "Option A, fully automatic" (skill Claude Code), aggiunte le righe per `OWNERSHIP_TRANSFER_TEMPLATE.md`, per lo skill e per `PROCESS_FLOW.md` (ora cinque diagrammi) nella tabella dei file; comandi di clone e URL raw aggiornati a `v1.4.0`.
- `.github/workflows/validate-kit.yml` — `OWNERSHIP_TRANSFER_TEMPLATE.md`, `.claude/skills/start-ai-project/SKILL.md` e `PROCESS_FLOW.md` aggiunti ai file richiesti.
- `CONTRIBUTING.md`, `.github/PULL_REQUEST_TEMPLATE.md` — aggiunta la regola di coerenza: un cambio a un gate, un tier o una decisione di `GOVERNANCE.md` deve aggiornare anche il diagramma corrispondente in `PROCESS_FLOW.md`.

### Perché ora
Il kit copriva già "come" classificare un progetto e "chi" lo approva, ma non "perché costruirlo", "quanto costa ogni mese", "chi possiede davvero il progetto quando l'owner cambia lavoro" e "chi collega business, tecnico e stakeholder" — lacune che, lasciate aperte, producono esattamente il punto cieco che questo kit esiste per prevenire (vedi `00_START_HERE.md`, "Why this exists"). Centralizzare l'Owner of record sul seat di Governance Administrator invece che su singoli dipendenti risolve "le persone non sono eterne" in modo strutturale, non solo procedurale: il seat ha già un piano di continuità (`GOVERNANCE.md` Sezione 4), un dipendente no. Lo skill Claude Code chiude anche il divario tra "il processo è documentato" e "il processo parte da solo quando qualcuno lo invoca".

## [1.3.1] — 2026-09-14

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

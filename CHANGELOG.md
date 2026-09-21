# Changelog

Tutte le modifiche rilevanti a questo kit sono elencate qui. Il formato segue [Keep a Changelog](https://keepachangelog.com/it/1.0.0/); il versioning segue [Semantic Versioning](https://semver.org/lang/it/).

## [1.9.0] — 2026-09-21

Aggiunge un livello di domande adattive sopra le dieci fisse, e chiude lo spazio per eludere l'intervista — entrambe richieste dopo aver osservato come l'AI viene effettivamente usata sul campo.

### Aggiunto
- **`AI_PROJECT_GUIDELINES.md` Sezione 7 — approfondimento adattivo.** Tabelle "per tipo di strumento" (LLM, bot RPA, flusso no-code, pipeline dati, app/chatbot cliente, assistente con accesso a tool/MCP) e "per area business" (Finance/Payments, HR, Sales/CRM, Legal, Support, Engineering/Infra) con le domande specifiche da aggiungere una volta noto il tier. Le dieci domande fisse restano identiche per tutti — questa sezione aggiunge profondità dove serve, non sostituisce né sposta il tier da sola.
- `AI_INTAKE_ASSESSMENT.md` e `AI_PROJECT_AUDIT.md` ora rimandano entrambi a quella sezione, così le domande di approfondimento sono le stesse sia in fase di costruzione sia in fase di audit, invece di due elenchi paralleli.

### Modificato
- **`AI_INTAKE_ASSESSMENT.md` Sezione 1 — niente più modo di girarci intorno.** Una risposta vaga fa scattare il follow-up già previsto; se anche dopo il follow-up l'utente non risponde davvero (rifiuta, cambia argomento, dice "metti quello che vuoi"), l'assistente non indovina e non lascia cadere la domanda in silenzio — la segnala esplicitamente e applica la regola di classificazione per eccesso già prevista in Sezione 3 (ora estesa esplicitamente al rifiuto di rispondere, non solo all'incertezza). Aggiunto un punto finale esplicito: l'intervista, una volta iniziata, va finita prima di passare ad altro — un "iniziamo pure a costruire" non la sostituisce silenziosamente.
- `AI_PROJECT_AUDIT.md` Sezione 2, punto 4 — ogni audit ricontrolla anche gli approfondimenti adattivi della Sezione 7 di `AI_PROJECT_GUIDELINES.md`, non solo alla prima classificazione: uno strumento può aver aggiunto un componente LLM o iniziato a toccare pagamenti dopo il primo audit.
- Riferimenti di versione (`README.md`, `00_START_HERE.md`, `AI_INTAKE_ASSESSMENT.md`, `AI_PROJECT_AUDIT.md`) aggiornati a `v1.9.0`; corretto anche un riferimento rimasto fermo a `v1.3.0` in `AI_INTAKE_ASSESSMENT.md`.

## [1.8.0] — 2026-09-21

Estende `AI_PROJECT_AUDIT.md` con un modo di entrare e uscire da un audit senza dover sempre percorrere l'intero flusso, e con un modo di tracciare i gap che non bloccano l'audit ma non vanno persi — richieste emerse riascoltando come viene effettivamente usato oggi.

### Aggiunto
- **Sezione 9 — comandi diretti.** `audit avvia`, `audit mappa-dati`, `audit stato`, `audit remediation`, `audit registra`: cinque punti d'ingresso con un solo obiettivo dichiarato ciascuno, per non dover sempre ripartire dall'inizio. Quando un comando è attivo, l'assistente resta vincolato al suo obiettivo — niente deviazioni su altri argomenti, niente cambio di comando a metà, niente correzione dei gap trovati nella stessa conversazione (quella resta compito del remediation prompt, Sezione 6).
- **Sezione 2, punto 14 — mappa dei dati.** Prima di chiudere un audit, l'assistente deve documentare esplicitamente da dove entrano i dati dello strumento, se sono dati cliente/personali/finanziari, dove sono salvati, e quali altri strumenti ne consumano l'output — una riga sui dati senza questa tracciatura è una supposizione, non evidenza.
- **Stato "Aperto" (Sezione 4 e 10).** Un quarto stato oltre a Pass/Fail/N-A: un requisito realmente non soddisfatto ma che non blocca l'esito di questo audit — tipicamente perché il gap dipende da un altro strumento o un'integrazione mancante, non dallo strumento auditato. Ha una colonna dedicata ("Dipende da") per registrare quella dipendenza, e non può essere usato per ammorbidire un Fail che è davvero colpa dello strumento in esame.
- **Sezione 10 — punti aperti e promemoria.** Cadenza dei promemoria basata sulla priorità del punto (Alta = ogni mese, Media = ogni due mesi, Bassa = ogni sei mesi — mai annuale), calcolata e registrata nell'Hub alla stessa registrazione dell'audit (Sezione 2, punto 11). Finché l'Hub non invia promemoria da solo, l'assistente deve far riemergere i punti Aperto scaduti a ogni interazione successiva con quello strumento.
- **Sezione 6 — remediation esteso ai punti Aperto** e meccanismo di destinazione dei task: se Coraly è collegato, i task vanno aperti lì (non solo descritti nel file); il file `.md` resta il fallback finché quella connessione non esiste, o per i task che Coraly non può rappresentare (cambi strutturali/organizzativi, non di codice). Prevista la stessa logica per una futura integrazione diretta (es. un server MCP di Coraly raggiungibile dalla chat).

## [1.7.0] — 2026-09-17

Revisione complessiva di `AI_PROJECT_AUDIT.md` per allinearlo a come lavora davvero una società di audit esterna, non solo alle patch emerse dai test — l'obiettivo resta classificare e tenere aggiornati tutti gli strumenti AI dell'azienda, non produrre singoli documenti isolati.

### Aggiunto
- **Sezione 1, punto 5 — indipendenza.** Se chi audita ha costruito o mantiene lo strumento, il risultato è un'autovalutazione, non un audit indipendente, e il report deve dirlo esplicitamente. Non chiude da solo la conformità a T2/T3 — serve comunque il sign-off del Chair (T2) o della revisione indipendente del seat Security (T3).
- **Sezione 2, punto 7 — diritto di replica dell'owner.** Prima di finalizzare il report, ogni Fail va proposto all'owner perché lo confermi, lo contesti con una motivazione, o si impegni su una data di remediation — la sua risposta entra nel report finale, non è un verdetto a senso unico.
- **Sezione 2, punto 11 — registrazione obbligatoria nell'Hub.** Un audit che non viene registrato non conta ai fini della visibilità aziendale su cosa è stato classificato — non è un passaggio opzionale.
- **Sezione 2, punto 13 — strumenti scoperti per caso.** Se durante un audit emerge un altro strumento non registrato, va segnalato come finding a sé, mai ignorato o mescolato nell'audit in corso.
- **Sezione 4 (template) —** nuovi campi: tipo di audit (indipendente/autovalutazione), classificazione di riservatezza del documento, colonna "Risposta owner" per requisito, sezione per strumenti scoperti.
- **Sezione 5 —** nota esplicita di riservatezza: un report di audit è Confidenziale di default da T2 in su.
- **Sezione 2, punto 5 —** per i gate item T3, una policy che descrive cosa dovrebbe succedere non è evidenza che sia successo davvero — serve un'approvazione datata e firmata, non un documento generico.

## [1.6.0] — 2026-09-17

### Modificato
- `AI_PROJECT_AUDIT.md` Sezione 2, punto 5 — la verifica della checklist ora cerca l'evidenza da sola per prima (nel repository, in `SECURITY.md`/`PROJECT.md`, nelle config, nell'Hub) e porta all'utente un riscontro da confermare, invece di aprire con una domanda a freddo che presume l'utente tenga a mente lo stato di conformità. Si chiede direttamente solo ciò che non è ispezionabile (un'approvazione via email, una decisione presa a voce). Distingue esplicitamente questa regola dal gate della Sezione 1 punto 2 (la classificazione del tier resta un giudizio organizzativo che va sempre chiesto dal vivo, mai dedotto).

## [1.5.0] — 2026-09-17

### Aggiunto
- `AI_PROJECT_AUDIT.md` Sezione 1 — tre condizioni non negoziabili prima di iniziare un audit, emerse da un test reale dove l'assistente ha dedotto le risposte dal codice invece di chiederle, e ha finito per fare commit/PR dentro il repository controllato:
  1. serve qualcuno che può davvero rispondere (l'owner o un seat `GOVERNANCE.md`, non un contributor di codice) — altrimenti ci si ferma;
  2. si intervista prima, si ispeziona il codice dopo — mai il contrario;
  3. il codice è un controllo incrociato sulle risposte, non la fonte primaria — una contraddizione va segnalata e risolta, non media, e in caso di dubbio si classifica per eccesso;
  4. non si scrive mai nel repository del progetto auditato — nessun commit, branch, o PR lì. Il kit governa un progetto, non ne diventa parte.
- Se una di queste condizioni non è rispettabile, l'assistente si ferma e lo dice — non esegue una versione ridotta dell'audit.

### Modificato
- `00_START_HERE.md` — il prompt dell'Opzione C ora chiede a chi lo usa di dichiararsi come owner fin dall'inizio, e richiama esplicitamente le nuove regole.
- `AI_PROJECT_AUDIT.md` — rinumerate le sezioni successive (2&ndash;8) per fare spazio alla nuova Sezione 1; riferimenti incrociati corretti di conseguenza.

## [1.4.0] — 2026-09-17

### Aggiunto
- `AI_PROJECT_AUDIT.md` — copre il vuoto lasciato da `AI_INTAKE_ASSESSMENT.md`, che serve solo per progetti **nuovi**: dà all'assistente AI lo script per auditare uno strumento **già esistente** — conferma o ri-deriva il tier, applica la checklist corretta (matrice T0–T2, checklist a 42 punti per T3, mai una versione abbreviata), chiede evidenza per ogni voce invece di accettare "dovrebbe andare bene", e produce un documento di audit scritto con un template dedicato (Sezione 3). Include un percorso di escalation esplicito al consiglio di governance se un audit T3 fallisce una gate item (Sezione 6).
- `AI_PROJECT_AUDIT.md` Sezione 4 — chi riceve il report di audit, scalato per tier e ancorato ai seat già definiti in `GOVERNANCE.md` (solo l'owner a T0/T1, owner + Chair a T2, l'intero consiglio a T3) invece di un elenco fisso di destinatari.
- `AI_PROJECT_AUDIT.md` Sezione 5 — il prompt di remediation: un secondo file, separato dal report, con solo i punti in Fail come checklist spuntabile, pensato per essere dato direttamente a un assistente AI per chiudere i gap.
- `00_START_HERE.md` — nuova "Opzione C — audit di uno strumento esistente", con il prompt pronto da usare.

### Modificato
- `README.md`, `00_START_HERE.md` — comandi di clone e URL raw aggiornati a `v1.4.0`.
- `.github/workflows/validate-kit.yml` — `AI_PROJECT_AUDIT.md` aggiunto ai file richiesti.

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

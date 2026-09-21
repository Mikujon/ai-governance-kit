# AI Project Governance Kit

Il modello unico per costruire, classificare e certificare ogni strumento AI sviluppato internamente — dimensionato in base al rischio reale del progetto, non uguale per tutti. Copre anche automazioni "senza codice" (Sheets/Apps Script, Power Automate, Zapier/Make, bot RPA): il tier dipende da chi lo usa e cosa tocca, non dal linguaggio con cui è stato costruito — vedi `00_START_HERE.md`.

Perché esiste: senza un modello comune, ogni script o automazione interna rischia di diventare un punto cieco quando chi l'ha creato cambia ruolo o lascia l'azienda — nessun owner, nessuna verifica di sicurezza, nessuna visibilità sui costi/risparmi. Questo kit risolve i 7 requisiti fondativi elencati in `PILLARS_COVERAGE.md`, in modo proporzionato alla dimensione reale del progetto.

## Come si usa

1. Clona questo repository (vedi [Versioning](#versioning) per come puntare a una versione precisa).
2. Apri **[`00_START_HERE.md`](./00_START_HERE.md)** — spiega le tre modalità disponibili (intervista guidata dall'AI, autoclassificazione, o audit di uno strumento già esistente) e quale file usare.
3. Segui la guida passo-passo completa in **[`reference/Guida_Uso_Kit_Governance_AI.docx`](./reference/Guida_Uso_Kit_Governance_AI.docx)**.

## Contenuto

| File | Cosa contiene |
|---|---|
| `00_START_HERE.md` | Indice e punto di partenza. |
| `AI_INTAKE_ASSESSMENT.md` | Script per l'AI: come intervistare l'utente e assegnare il tier, per un progetto **nuovo**. |
| `AI_PROJECT_AUDIT.md` | Script per l'AI: come auditare uno strumento **già esistente** — conferma il tier, applica la checklist giusta, produce un documento di audit scritto. |
| `PROJECT_STARTER_T0_PERSONAL.md` | Starter per script/report personali, usa-e-getta. |
| `PROJECT_STARTER_T1_BASIC.md` | Starter per piccoli strumenti interni. |
| `PROJECT_STARTER_T2_STANDARD.md` | Starter per strumenti dipartimentali/integrati. |
| `PROJECT_STARTER_T3_CRITICAL.md` | Starter per strumenti su dati regolati, cliente, o azione autonoma. |
| `AI_PROJECT_GUIDELINES.md` | Modello di classificazione e matrice completa dei requisiti. |
| `AI_PROJECT_STRUCTURE.md` | Standard tecnico: stack, ReBAC, dati, design, CI/CD. |
| `reference/AI_Development_Standard.docx` | Policy completa e checklist di audit a 42 punti (Tier 3). |
| `reference/Guida_Uso_Kit_Governance_AI.docx` | Guida utente passo-passo. |
| `PILLARS_COVERAGE.md` | Mappa dei 7 requisiti fondativi → dove ciascuno è definito, applicato e tracciato. Include anche il crosswalk verso NIST AI RMF / EU AI Act / ISO 42001 (Sezione 8). |
| `AI_ASSISTANT_USAGE_GUIDE.md` | Come usare Claude Code / Codex CLI: installazione, modalità, come contenere i costi in token, e cosa non deve mai poter toccare. |
| `GOVERNANCE.md` | Chi decide: il consiglio di governance (4 posti), cosa richiede l'approvazione di chi, e cosa succede se il Governance Administrator non è disponibile. |

## Clonare da un'AI o da CI (nessuna copia locale necessaria)

Se il tuo assistente AI ha accesso a git, non serve allegargli i file: fagli clonare direttamente questo repository prima di iniziare l'intervista.

```bash
git clone --branch v1.9.0 https://github.com/wearefiber/ai-governance-kit.git
```

Oppure, per leggere un solo file senza clonare (es. dentro un prompt):

```
https://raw.githubusercontent.com/wearefiber/ai-governance-kit/v1.9.0/AI_INTAKE_ASSESSMENT.md
```

## Versioning

Questo repository segue [Semantic Versioning](https://semver.org/lang/it/) tramite tag Git (`v1.0.0`, `v1.1.0`, ...). Ogni progetto che usa il kit **deve** registrare, nel proprio `PROJECT.md`, quale tag era in vigore al momento della classificazione — è quel numero che un audit successivo confronta, anche se nel frattempo il kit è cambiato. Vedi `CHANGELOG.md` per la cronologia delle versioni.

## Proporre una modifica

Vedi [`CONTRIBUTING.md`](./CONTRIBUTING.md). Ogni modifica ai file del kit richiede la revisione del Governance Administrator (vedi `.github/CODEOWNERS`) — il ruolo e il resto del consiglio di governance sono definiti in [`GOVERNANCE.md`](./GOVERNANCE.md).

## Proprietà

Repository interno — organizzazione `wearefiber`. Non distribuire al di fuori dell'organizzazione senza autorizzazione del Governance Administrator.

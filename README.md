# AI Project Governance Kit

Il modello unico per costruire, classificare e certificare ogni strumento AI sviluppato internamente — dimensionato in base al rischio reale del progetto, non uguale per tutti.

## Come si usa

1. Clona questo repository (vedi [Versioning](#versioning) per come puntare a una versione precisa).
2. Apri **[`00_START_HERE.md`](./00_START_HERE.md)** — spiega le due modalità disponibili (intervista guidata dall'AI, o autoclassificazione) e quale file usare.
3. Segui la guida passo-passo completa in **[`reference/Guida_Uso_Kit_Governance_AI.docx`](./reference/Guida_Uso_Kit_Governance_AI.docx)**.

## Contenuto

| File | Cosa contiene |
|---|---|
| `00_START_HERE.md` | Indice e punto di partenza. |
| `AI_INTAKE_ASSESSMENT.md` | Script per l'AI: come intervistare l'utente e assegnare il tier. |
| `PROJECT_STARTER_T0_PERSONAL.md` | Starter per script/report personali, usa-e-getta. |
| `PROJECT_STARTER_T1_BASIC.md` | Starter per piccoli strumenti interni. |
| `PROJECT_STARTER_T2_STANDARD.md` | Starter per strumenti dipartimentali/integrati. |
| `PROJECT_STARTER_T3_CRITICAL.md` | Starter per strumenti su dati regolati, cliente, o azione autonoma. |
| `AI_PROJECT_GUIDELINES.md` | Modello di classificazione e matrice completa dei requisiti. |
| `AI_PROJECT_STRUCTURE.md` | Standard tecnico: stack, ReBAC, dati, design, CI/CD. |
| `reference/AI_Development_Standard.docx` | Policy completa e checklist di audit a 42 punti (Tier 3). |
| `reference/Guida_Uso_Kit_Governance_AI.docx` | Guida utente passo-passo. |

## Clonare da un'AI o da CI (nessuna copia locale necessaria)

Se il tuo assistente AI ha accesso a git, non serve allegargli i file: fagli clonare direttamente questo repository prima di iniziare l'intervista.

```bash
git clone --branch v1.0.0 https://github.com/Mikujon/ai-governance-kit.git
```

Oppure, per leggere un solo file senza clonare (es. dentro un prompt):

```
https://raw.githubusercontent.com/Mikujon/ai-governance-kit/v1.0.0/AI_INTAKE_ASSESSMENT.md
```

## Versioning

Questo repository segue [Semantic Versioning](https://semver.org/lang/it/) tramite tag Git (`v1.0.0`, `v1.1.0`, ...). Ogni progetto che usa il kit **deve** registrare, nel proprio `PROJECT.md`, quale tag era in vigore al momento della classificazione — è quel numero che un audit successivo confronta, anche se nel frattempo il kit è cambiato. Vedi `CHANGELOG.md` per la cronologia delle versioni.

## Proporre una modifica

Vedi [`CONTRIBUTING.md`](./CONTRIBUTING.md). Ogni modifica ai file del kit richiede la revisione del Governance Administrator (vedi `.github/CODEOWNERS`).

## Proprietà

Repository interno — organizzazione `wearefiber`. Non distribuire al di fuori dell'organizzazione senza autorizzazione del Governance Administrator.

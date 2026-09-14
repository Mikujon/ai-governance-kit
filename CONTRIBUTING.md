# Come proporre una modifica al kit

Questo repository è la fonte unica di verità per la governance dei progetti AI in azienda. Una modifica qui si propaga a ogni progetto che lo referenzia — trattalo con lo stesso rigore di un cambio di policy, non di un semplice refactor.

## Processo

1. Crea un branch (`fix/...`, `feat/...`, `docs/...`).
2. Modifica i file necessari. Se cambi un requisito nella matrice (`AI_PROJECT_GUIDELINES.md` o `AI_PROJECT_STRUCTURE.md`), aggiorna anche lo starter (`PROJECT_STARTER_T*.md`) corrispondente — devono restare coerenti tra loro. Se il cambio sposta un gate, un tier o una decisione di `GOVERNANCE.md`, aggiorna anche il diagramma interessato in `PROCESS_FLOW.md` — un diagramma non aggiornato è esattamente il tipo di documentazione stantia che questo kit esiste per evitare.
3. Aggiorna `CHANGELOG.md` con la modifica, sotto una nuova sezione `[Unreleased]` se non stai già rilasciando una versione.
4. Apri una Pull Request usando il template — la checklist ti ricorda i punti di coerenza da verificare.
5. La PR richiede l'approvazione del Governance Administrator (vedi `.github/CODEOWNERS`).
6. Al merge, se la modifica è sostanziale (nuovo requisito, nuovo tier, cambio di struttura), taggare una nuova versione:

```bash
git checkout main && git pull
git tag -a v1.1.0 -m "Descrizione sintetica della modifica"
git push origin v1.1.0
```

Usa **major** per cambi che rompono la compatibilità con progetti già classificati (es. un tier viene rinominato o unito), **minor** per nuovi requisiti o starter, **patch** per correzioni di testo/refusi.

## Cosa NON fare

- Non modificare un tag già pubblicato — un progetto potrebbe averlo già usato per il proprio audit. Se serve una correzione, pubblica una nuova patch version.
- Non eliminare un file richiesto da uno starter senza aggiornare lo starter stesso nella stessa PR.
- Non alzare la soglia di un requisito (renderlo più permissivo) senza il consenso della funzione Security/Cyber, quando il requisito riguarda la sicurezza.

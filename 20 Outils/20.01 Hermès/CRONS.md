# Hermès — Crons (état & désactivations)

> Registre des tâches planifiées (crons) d'Hermès et de leurs désactivations.
> **Règle :** à chaque désactivation d'un cron, noter ici le **pourquoi** (basique).
> Mis à jour le 2026-09-05.

## Crons actifs

| Cron | Horaire | Rôle | Dernier run |
|---|---|---|---|
| `vault-backup` | 23h00 | Backup du Vault sur GitHub | 02/09 23h01 OK |
| `rapport-vault-quotidien` | 02h00 | Résumé du Vault dans #rapports | 03/09 02h00 OK |
| `Veille IA quotidienne` | 09h00 | Veille IA | 03/09 19h14 OK |
| `Vider corbeille Gmail` | 02/10 09h00 | Purge corbeille email-triage | — |
| `qa-agent-ulysse-poll` | toutes les 30 min | Poll issues `bug` Ulysse → agent QA (cabinet, skill `qa-loop`, clapet Hermès, PR obligatoire). Monitor : ne réveille l'agent que si l'état des issues change. cmd_test : `test_tactile.py` ajouté en tête de chaîne (issue #122, 05/09) | 05/09 15h14 OK (RIEN) · 05/09 15h53 OK (#122 → PR #124, clapet vert) |

## Désactivations (avec pourquoi)

| Cron | Désactivé le | Pourquoi | Réactivé le |
|---|---|---|---|
| `qa-agent-ulysse-poll` | 05/09 16h12 | Pause demandée par Raf (« Stop la loop, pour le moment, je te dirai quand relancer ») pendant le test de bout en bout — PR #124 ouverte (clean), clapet non clôturé, rien de mergé | — |
| `vault-backup` | (avant 04/09) | Plus d'utilité (raison à confirmer) | 04/09 |
| `rapport-vault-quotidien` | (avant 04/09) | Plus d'utilité (raison à confirmer) | 04/09 |

> **Note :** la raison exacte de la désactivation initiale de `vault-backup` et
> `rapport-vault-quotidien` n'est pas tracée (probablement une pause via le
> dashboard). Réactivés le 04/09. À l'avenir, noter le pourquoi ici dès qu'un
> cron est désactivé.

# Hermès — Crons (état & désactivations)

> Registre des tâches planifiées (crons) d'Hermès et de leurs désactivations.
> **Règle :** à chaque désactivation d'un cron, noter ici le **pourquoi** (basique).
> Mis à jour le 2026-09-07 (ménage).

## Crons actifs

| Cron | Horaire | Rôle | Dernier run |
|---|---|---|---|
| `qa-agent-ulysse-poll` | toutes les 30 min | Poll issues `bug` Ulysse → agent QA (cabinet, skill `qa-loop`, clapet Hermès, PR obligatoire). Monitor : ne réveille l'agent que si l'état des issues change. cmd_test : `test_tactile.py` ajouté en tête de chaîne (issue #122, 05/09) | 05/09 15h14 OK (RIEN) · 05/09 15h53 OK (#122 → PR #124, clapet vert) |
| `Veille IA quotidienne` | 09h00 | Veille IA | 07/09 09h00 OK |
| `Boucle de rétroaction quotidienne` | 21h00 | Rétrospective quotidienne : analyse journaux de séance + sessions → leçons appliquées (skills/mémoire/vault), git/config exclus | — (créé 07/09, premier run 21h00) |
| `Vider corbeille Gmail` | 02/10 09h00 | Purge corbeille email-triage | — |

## Désactivés / supprimés

| Cron | Statut | Dernier état | Pourquoi |
|---|---|---|---|
| `qa-agent-ulysse-poll` | **réactivé 07/09** | Pause du 05/09 16h12 | Était en pause depuis le test e2e (PR #124 ouverte). Relancé par Raf le 07/09. |
| `vault-backup` | supprimé 07/09 | — | Plus d'utilité (raison initiale à confirmer). |
| `rapport-vault-quotidien` | supprimé 07/09 | — | Plus d'utilité (raison initiale à confirmer). |
| `boucle-ulysse` | supprimé 07/09 | — | Zombie (enabled, n'a pas tourné depuis 02/09) ; remplacé par `qa-agent-ulysse-poll`. |
| `Veille IA quotidienne` (doublon) | supprimé 07/09 | — | Doublon de la Veille active (auto-paused, prompt vide / désactivé). |
| `test-freebuff-autonome` | supprimé 07/09 | — | One-shot consumé (désactivé). |
| `bilan-rtk-soir` / `bilan-rtk-soir-v2` | supprimés 07/09 | — | Crones de test RTK, terminés. |
| `Tri email automatique` | **désactivé 07/09 (conservé)** | — | Conservé (rôle email) mais actuellement désactivé ; le réactiver si besoin. |

> **Note :** `vault-backup` est supprimé — la sauvegarde du Vault passe désormais par
> Obsidian Git (Ctrl+Alt+S, voir `00 Index.md`), pas par un cron.

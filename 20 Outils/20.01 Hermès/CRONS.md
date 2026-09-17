# Hermès — Crons (état & désactivations)

> Registre des tâches planifiées (crons) d'Hermès et de leurs désactivations.
> **Règle :** à chaque désactivation d'un cron, noter ici le **pourquoi** (basique).
> Mis à jour le 2026-09-17 (incident jobs.json + réparation).

## Incident du 09→17/09/2026 — crons muets

- **Symptôme** : plus aucune livraison de la veille IA à partir du 09/09 (Raf le
  signale les 15 et 17/09). Aucune erreur, aucune alerte, aucun échec visible.
- **Cause** : le dashboard Rust `~/projets/hermes-dashboard` (port 8091) réécrit
  `~/.hermes/cron/jobs.json` avec un schéma réduit de 11 champs. Ses endpoints
  **Pause / Relancer / Exécuter** effacent `prompt`, `schedule`, `deliver`,
  `origin`, `repeat`, `script`, `skills`. Le job reste `enabled=true` mais sans
  occurrence calculable → il ne se déclenche plus jamais, silencieusement.
- **Repro** : un seul `POST /api/crons/:id/pause` détruit 7 champs (test
  `~/projets/scratch-cron/test_dashboard_bug2.sh`). Vérifié à l'inverse : la CLI
  `hermes cron`, les fonctions `cron.jobs` (pause/resume/mark_run/update) et le
  dashboard Python (9119) **préservent** tous les champs.
- **Réparation** :
  - Dashboard Rust corrigé (`serde(flatten)` conserve et réémet tout champ
    inconnu, au niveau job et au niveau racine). Commit `aefddd8`, 19 tests
    verts, dont `pause_preserves_unknown_fields`.
  - `Veille IA quotidienne` : prompt + livraison restaurés depuis les backups
    curator, horaire **07h30** (demande de Raf du 06/09), prochaine livraison
    le 17/09 à 07h30. Run de contrôle en manuel OK le 17/09 à 05h54.
  - `Tri email automatique` : prompt + `deliver` + intervalle restaurés.
  - `qa-agent-ulysse-poll` : prompt + skills + script restaurés.
  - `Vider corbeille Gmail` : **prompt introuvable** dans les backups (job
    créé après le dernier snapshot) → laissé tel quel, à recréer sur accord.
  - `Boucle de rétroaction quotidienne` : **prompt introuvable** idem → laissé
    désactivé, à recréer sur accord.
  - Aucun `model`/`model_snapshot` restauré (règle : un cron ne dépend jamais
    d'un modèle).

## Crons actifs

| Cron | Horaire | Rôle | Dernier run |
|---|---|---|---|
| `Veille IA quotidienne` (`f2f69465b4da`) | 07h30 | Veille IA (11 sources, 5-6 sujets) → thread #revue-quotidien-ia | 17/09 05h54 OK (run de contrôle) |
| `qa-agent-ulysse-poll` (`f73e969eb6f0`) | toutes les 30 min | Poll issues `bug` Ulysse → agent QA (cabinet, skill `qa-loop`, clapet Hermès, PR obligatoire, script `qa_loop_poll.sh`) | 05/09 16h03 OK |
| `Tri email automatique` (`dffce1216c72`) | tous les 3 jours | Tri boîte kuchubb@gmail.com (Himalaya, skill email-inbox-triage) → #gestion-emails | 07/09 12h32 OK |
| `Vider corbeille Gmail` (`ca3f08d2cccc`) | 02/10 09h00 | Purge corbeille email-triage (one-shot) — **prompt perdu** | — |

## Désactivés / supprimés

| Cron | Statut | Dernier état | Pourquoi |
|---|---|---|---|
| `Boucle de rétroaction quotidienne` (`d5b1eeba86c2`) | désactivé 17/09 | — | Prompt + schedule détruits par le dashboard (schéma réduit) ; prompt absent des backups → désactivé plutôt que recréé de mémoire. |
| `qa-agent-ulysse-poll` | réactivé 07/09 | Pause du 05/09 16h12 | Était en pause depuis le test e2e (PR #124 ouverte). Relancé par Raf le 07/09. |
| `vault-backup` | supprimé 07/09 | — | Plus d'utilité (raison initiale à confirmer). |
| `rapport-vault-quotidien` | supprimé 07/09 | — | Plus d'utilité (raison initiale à confirmer). |
| `boucle-ulysse` | supprimé 07/09 | — | Zombie (enabled, n'avait pas tourné depuis 02/09) ; remplacé par `qa-agent-ulysse-poll`. |
| `Veille IA quotidienne` (doublon) | supprimé 07/09 | — | Doublon de la Veille active (auto-paused, prompt vide / désactivé). |
| `test-freebuff-autonome` | supprimé 07/09 | — | One-shot consumé (désactivé). |
| `bilan-rtk-soir` / `bilan-rtk-soir-v2` | supprimés 07/09 | — | Crones de test RTK, terminés. |
| `Tri email automatique` | réactivé 17/09 | désactivé 07/09 | Conservé puis restauré après l'incident. |

> **Note :** `vault-backup` est supprimé — la sauvegarde du Vault passe désormais par
> Obsidian Git (Ctrl+Alt+S, voir `00 Index.md`), pas par un cron.

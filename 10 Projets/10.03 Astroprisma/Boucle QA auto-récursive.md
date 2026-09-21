# Boucle QA auto-récursive — Astroprisma

> Posée le 2026-09-21 sur demande de Raf : « lance un agent QA loop sur
> astroprisma et qu'il travaille en boucle auto-récursive pour améliorer le jeu ».

## Nature du dispositif

Ce n'est **pas** un agent qui dialogue : c'est un **tick borné** qui se réarme
tout seul. Un passage = une itération complète en 6 étapes, puis la boucle se
replanifie. Rien n'est laissé au hasard ni à l'improvisation.

| Étape | Ce qui se passe | Garde-fou |
|---|---|---|
| 1. AUDITE | un auditeur (Codex) cherche UN défaut réel et le **prouve** (mesure/capture) | pas de preuve chiffrée ⇒ pas d'issue |
| 2. FILE | le défaut devient une issue GitHub (`bug` ou `amelioration`) | issue auto-portante (symptôme, preuve, critères, commande) |
| 3. CORRIGE | Codex corrige sur une branche dédiée, dans un **worktree jetable** | jamais le checkout partagé ; worktree depuis `origin/master` |
| 4. VÉRIFIE | test rouge **avant**, suite complète verte **après** | interdiction d'affaiblir les tests (contrôle du diff) |
| 5. PR | la branche part en pull request | **jamais de merge** — Raf/Hermès tranche |
| 6. RÉARME | se replanifie au tick suivant | une seule PR ouverte à la fois (pas d'empilement) |

## Où c'est

- **Cible** : `~/projets/astroprisma-dsh` → dépôt
  [`u2987920406-rgb/astroprisma-maquette-tactique`](https://github.com/u2987920406-rgb/astroprisma-maquette-tactique)
  (privé, branche `master`), servi sur `:5211` → tailnet `:8451`.
- **Tick** : `~/.hermes/scripts/astroprisma_qa_autoloop.sh`
- **Poll** (détection seule) : `~/.hermes/scripts/astroprisma_qa_poll.sh`
- **Cron** : `qa-astroprisma-autoloop` (job `8514c223c460`), `*/10 * * * *`,
  `deliver: origin`, `continuity: true`, `model_snapshot: null` (suit le modèle
  global — règle des crons respectée).
- **Journal** : `~/.hermes/cron/output/astroprisma-autoloop.log`
- **Rapports d'audit** : `audit/*.md` (hors git, régénérés)

## Cerveau QA

**Codex CLI** (`codex exec --model gpt-5.6-terra`) — c'est la dérogation établie
par Raf (skill `qa-loop`) : meilleur raisonnement autonome, compte ChatGPT, zéro
coût API additionnel. L'orchestrateur (le cron) tourne, lui, sur le **modèle
global courant**, jamais épinglé.

## Le clapet (ce qui empêche la boucle de dériver)

Codex ne merge **jamais**. Trois barrières avant qu'un travail compte :

1. **Commit réel exigé** : pas de commit sur la branche ⇒ pas de PR, l'issue
   reste ouverte pour le tick suivant.
2. **Contrôle du diff** : si le diff supprime trop de lignes de tests, la PR
   n'est pas créée (perte de garde probable) — à relire à la main.
3. **Clapet humain** : chaque PR est relue **sur le code réel** (jamais sur le
   texte de réponse de l'agent) — le test échouait-il avant ? la suite est-elle
   verte ? le fix est-il minimal ? la couverture n'a-t-elle pas baissé ?

## Issues de départ (produites par les audits du 21/09)

- **#1** (bug, élevé) — vue duel : ECHO **5,46×** plus grand que le Looter à
  l'init, rapport instable (5,46 → 1,27 → 1,25).
- **#2** (bug) — ECHO flotte **77 px au-dessus** du bas de son hexagone.
- **#3** (amelioration) — barre de vie ennemie quasi-invisible + FUIN lit comme
  un bouton désactivé.

## Pièges rencontrés à la pose (à savoir)

- **`model_snapshot` épinglé à la création** : le tool `cronjob` a collé un
  snapshot au job neuf. Corrigé par `hermes cron edit 8514c223c460 --model
  deepseek-v4.1-flash` (le tool `update` ne suffit pas, seul le CLI a l'effet).
- **`node_modules` committé par erreur** : c'est un **symlink** vers
  `~/projets/deepseek-harness/node_modules` — git suit le lien. Retiré du suivi,
  ajouté au `.gitignore`.
- **Codex hors dépôt** : `codex exec` refuse de tourner hors d'un dépôt de
  confiance ⇒ `--skip-git-repo-check` obligatoire.
- **Le serveur preview ne survit pas à un reset** : le tick le relance lui-même
  s'il trouve `:5211` éteint (le poll répond alors `SERVEUR_MORT`).

## Voir aussi

- [[Projet]] · [[2026-09-21]]
- Skill `qa-loop` (mandat, clapet, frontières) · `crons-hermes` (règles des crons)

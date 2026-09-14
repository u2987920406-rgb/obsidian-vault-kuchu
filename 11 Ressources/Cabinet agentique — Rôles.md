# Cabinet agentique — Rôles

> Cartographie des rôles du cabinet, figée le 2026-09-08 avec Raf.
> **Un seul cerveau : GLM 5.3 (ollama-cloud), deux déclinaisons** (flash / plein).
> Freebuff abandonné. Pas d'escalade pour le moment.
> Chaque nouveau projet déclenche un rôle existant — on n'invente un agent que
> si un vrai vide de capacité apparaît.

## Mapping cerveau

**Règle --global (08/09) : tous les rôles suivent le modèle global courant
d'Hermès, jamais épinglés.** Actuellement : glm-5.3-flash (ollama-cloud).

| Rôle | Cerveau | Escalade |
|---|---|---|
| Codage | Modèle global | Aucune pour le moment |
| Orchestration | Modèle global | — |
| Vision | Modèle global | — |
| Recherche | Modèle global | — |
| Boucle rétro | — | propose des agents |

---

## Fiche 1 — Conceiveur (Orchestration)
- **Cerveau** : GLM 5.3 flash
- **Rôle** : décider, concevoir, planifier, vérifier. Le point fort du modèle faible.
- **Ne sous-traite JAMAIS** : conception, PRD, plan, AC, vérification sur disque.
- **Quand déclencher** : tout projet from scratch (La Méthode), toute décision de design.
- **Mandat type** : brainstorming → PRD → guidelines → plan jalons → AC.

## Fiche 2 — Codeur
- **Cerveau** : GLM 5.3 (plein)
- **Rôle** : code lourd, backend, frontend, build, game. Gros morceau d'un coup.
- **Escalade** : aucune pour le moment (si bloque → stop, on en reparle).
- **Quand déclencher** : ticket d'implémentation issu de La Méthode, TDD.
- **Mandat type** : contexte minimal + tâche précise + livrable + contraintes + budget + critère de vérif.

## Fiche 3 — Vision / Design-QA
- **Cerveau** : GLM 5.3 flash
- **Rôle** : tout ce qui touche à la vision — QA visuel (Kitsune), aperçu avant déploiement, analyse sprite/mockup/image.
- **Quand déclencher** : vérifier un écran, une capture, un rendu avant de livrer.
- **Mandat type** : image + question précise + critère de conformité.

## Fiche 4 — Recherche / Connaissance
- **Cerveau** : GLM 5.3 flash
- **Rôle** : web (web_search, arxiv, grounded-citations) + vault (MCP freebuff : search, stats, graph, timeline).
- **Quand déclencher** : question factuelle, veille, retrouver une note, cartographier le vault.
- **Mandat type** : sujet + sources attendues + format de sortie.

## Fiche 5 — Boucle rétro (apprentissage)
- **Cerveau** : cron 21h (d5b1eeba)
- **Rôle** : analyse journaux + sessions → leçons appliquées (skills / mémoire / vault).
- **Peut proposer des agents** : si un vide de capacité récurrent apparaît, le propose à Raf.
- **Jamais** : git push / config sans Raf.

---

## Règle d'or
- **4-5 rôles max.** Chaque projet = déclencheur d'un rôle existant, jamais un agent custom.
- **Vérifier sur disque**, jamais le texte de réponse d'un agent (clapet).
- **Pas d'escalade** tant que GLM 5.3 tient. Si un rôle bloque vraiment → en parler à Raf.

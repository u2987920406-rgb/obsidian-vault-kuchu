# Kitsune to Yōkai — Guide-lines

Règles de code/archi/nommage — ne jamais transiger. Étape 3, validées par Raf le 2026-09-03.

## Les 7 axes

1. **Langage** : GDScript (pas de C#) — natif Godot, plus simple. (protégé)
2. **Arborescence de scène** : une scene Godot par entité (`Player`, `Enemy`, `Projectile`, `FanPowerUp`, `HUD`), nommage snake_case des fichiers.
3. **Séparation logique/affichage** : les stats (vitesse, PV, dégâts) en **Resources .tres** (`PlayerStats`, `EnemyStats`, `ProjectileStats`), PAS en export var sur les nodes — jamais en dur dans _process.
4. **Signaux** : interaction entre entités par signaux de NOTIFICATION (mort, touché, ramassage) — jamais un canal de calcul. Le calcul de tir (auto-aim 8 dir, cônes) vit dans une classe pure (`ShootingMath`, `FanPattern`) appellée en synchrone par le node.
5. **Autoloads** : découper en 3 rôles uniques — `GameState` (score/vies/run, PURE, sans réf Node) / `GameEvents` (bus de signaux déclaratif) / `WaveManager`. Les entités n'émettent que vers GameEvents, et ne lisent/modifient l'état que via des méthodes de GameState, jamais d'attribut public à la volée.
6. **Tests** : extraire le gameplay de `_process` en classes pures testables (`ShootingMath`, `BossBrain` FSM, `WaveDirector`) ; _process n'applique que des décisions. Timer par **delta réel injectable** (pas framecount). Vagues en **données** (tuples purs). Tester en GUT.
7. **Nommage** : variables snake_case, constantes MAJUSCULES, fonctions verbe (get_score, spawn_wave).

## Statut

Protégées après validation humaine (Raf). Ne pas modifier sans re-validation.

## Prochaine étape

Étape 4 — Relecture croisée brainstorming/PRD/guidelines (obligatoire).

# Kitsune to Yōkai — Guide-lines

Règles de code/archi/nommage — ne jamais transiger. Étape 3, validées par Raf le 2026-09-03.

## Les 7 axes

1. **Langage** : GDScript (pas de C#) — natif Godot, plus simple, parfait pour l'apprentissage.
2. **Arborescence de scène** : une scene Godot par entité (`Player`, `Enemy`, `Projectile`, `FanPowerUp`, `HUD`), nommage snake_case des fichiers.
3. **Séparation logique/affichage** : stats (vitesse, PV, dégâts) en export vars sur les nodes, jamais en dur dans `_process`.
4. **Signaux** : interaction entre entités par signaux (mort, touché, ramassage), jamais d'appels directs croisés.
5. **Autoloads** : un seul singleton `Game` pour l'état global (score, vies, run), pas de variables globales éparpillées.
6. **Tests** : logique pure séparée du moteur (maths de tir, PV, collisions logiques) en GUT ; garder les nodes Godot légères.
7. **Nommage** : variables snake_case, constantes MAJUSCULES, fonctions verbe (get_score, spawn_wave).

## Statut

Protégées après validation humaine (Raf). Ne pas modifier sans re-validation.

## Prochaine étape

Étape 4 — Relecture croisée brainstorming/PRD/guidelines (obligatoire).

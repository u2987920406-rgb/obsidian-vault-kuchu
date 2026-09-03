# Kitsune to Yōkai — Plan jalonné

Étape 5 (plan jalonné, risque décroissant) — 2026-09-03. Référent central après les guidelines.

## Casting d'agents (validé par Raf 2026-09-03)

- Hermès (deepseek-v4-flash) — orchestration, conception, plan, AC, vérification sur disque
- Freebuff GLM 5.3 Flash — code lourd / gros morceau (2 sessions/jour), brikes isolées
- Kimi k2.7-code (repli ollama-cloud) — volume sans quota quand GLM épuisé
- gemma4:31b / qwen3.5:397b — vision (sprites/mockups)
- Vérification — Hermès + tests automatisés, jamais un gros modèle

## Jalons (risque décroissant, approche verticale)

### Jalon 1 — Socle Godot Android *(risque max : export/build APK)*
- Projet Godot 2D, orientation paysage, contrôles tactiles stub.
- Build **APK** qui s'installe et se lance sur le téléphone Android.
- Checklist : APK généré sans erreur, installable, écran vide paysage s'affiche.
- *Pourquoi en premier :* l'export Android est la plus grosse incertitude technique. Se lever tôt évite de construire le gameplay sur un socle qu'on ne saurait pas livrer.

### Jalon 2 — Kitsune jouable
- Perso kitsune : déplacement au joystick virtuel, tir **8 directions**, boule de feu (spam).
- Checklist : bouge au stick, tire dans 8 directions, boule de feu va droit + portée longue.

### Jalon 3 — Ennemis + vagues
- Kappa (tank), tengu (sniper), oni (sprinter). Spawn par vagues, dégâts reçus, score par ennemi.
- Checklist : 3 comportements distincts, vagues montent en difficulté, score compte.

### Jalon 4 — Power-up éventail
- Drop du kappa et du mini-boss, ramassage, **20 tirs** puis consommation, visuel.
- Checklist : ramasse → tire éventail (large/court) 20×, puis revient à la boule de feu.

### Jalon 5 — Vie, mini-boss, écrans *(boucle complète)*
- 3 vies, touché = lâche l'éventail + invincibilité courte + respawn au checkpoint.
- Mini-boss **oni géant** (charge → saut → rafale), écrans menu/victoire/game over.
- Checklist : game loop complet de bout en bout, kill mini-boss → victoire, 3 morts → game over.

## Porte renforcée (bonus, validé en discussion)

Lancer un **design-review adversarial multi-agents** (game designer, systems designer, QA, lead programmer × 2) sur le PRD + ce plan **avant le jalon 1**. Une passe, pas par jalon.

## Critère de v1

Tous les jalons cochés + passe globale sans régression → v1. Le backlog (coffres, autres niveaux, bosses, difficulté réglable) devient la v2.

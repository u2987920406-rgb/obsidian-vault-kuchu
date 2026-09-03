# Kitsune to Yōkai — Plan jalonné

Étape 5 (plan jalonné) + recul design (étape 4 / porte renforcée). MAJ 2026-09-04 après le design-review adversarial (8 agents) et validation Raf.

## Casting d'agents (validé par Raf 2026-09-03)

- Hermès (deepseek-v4-flash) — orchestration, conception, plan, AC, vérification sur disque
- Freebuff GLM 5.3 Flash — code lourd / gros morceau (2 sessions/jour), brikes isolées
- Kimi k2.7-code (repli ollama-cloud) — volume sans quota quand GLM épuisé
- gemma4:31b / qwen3.5:397b — vision (sprites/mockups)
- Vérification — Hermès + tests automatisés, jamais un gros modèle

## Jalons (risque décroissant, approche verticale) + Décisions de recul intégrées

### Jalon 1 — Socle Godot Android *(risque max : export/build APK)*
- Godot **4.4.1 stable**, JDK **17**, Android SDK (build-tools 35, platform android-35), **keystore dédié**.
- Orientation **Landscape bloquée** (DisplayServer), ABI **arm64-v8a seule**, viewport **1280×720 stretch canvas_items**, APK < 100 Mo.
- Build **APK** qui s'installe et se lance sur Android.
- Checklist : APK généré sans erreur, installable, écran vide paysage s'affiche.

### Jalon 2 — Kitsune jouable
- Perso kitsune : déplacement joystick virtuel gauche (déplacement seul).
- **Tir auto-aim semi-auto** : part vers l'ennemi le plus proche dans un cône avant, **snap 8 directions**. Boule de feu (spam, droit, portée longue).
- Archi : `ShootingMath` (vélocité tir = f(direction), pure/testable), `PlayerStats` resource .tres.
- Checklist : bouge au stick, tire auto-aim vers cible dans cône, boule de feu droit + longue.

### Jalon 3 — Ennemis + vagues
- Kappa (tank, drope éventail), tengu (sniper zigzag + projectile), oni (sprinter charge).
- Vagues en **données** (tuples purs : type, spawn_at_y), `WaveDirector` les instancie. Score par ennemi.
- Archi : patterns purs testables, `_process` applique uniquement.
- Checklist : 3 comportements distincts, vagues montent en difficulté, score compte.

### Jalon 4 — Power-up éventail
- Drop kappa + mini-boss, ramassage. **30 tirs** puis consommation.
- **Pas d'empilement** : ramasser actif = +25 pts (« éventail plein »).
- Large/court/dégâts forts (shotgun à éventail). `FanPattern` pur.
- Checklist : ramasse → tire éventail large/court 30×, puis boule de feu ; ramassage déjà actif = +25 sans empiler.

### Jalon 5 — Vie, mini-boss, écrans *(boucle complète)*
- **3 vies**. **Découplage de punition** : au contact une vie OU l'éventail (-1/3), pas les deux. Invincibilité **1,5 s** (timer delta réel, injectable).
- **Checkpoints entre sections**, respawn safe + **filet d'éventail** (kappa de secours) → pas de softlock.
- Mini-boss **oni géant** : charge → saut → rafale, **saut visant la position du joueur**, **enrage < 50% PV**. `BossBrain` FSM pure testable.
- Écrans menu titre → niveau → victoire / game over. High-score en local.
- Checklist : game loop de bout en bout, kill mini-boss → victoire, 3 morts → game over, respawn sans softlock.

## Portes de conception (validées en méthode)

- Étape 4 (relecture croisée) : **faite** — pas de contradiction bloquante ; scrolling vertical paysage clarifié.
- **Porte renforcée : design-review adversarial multi-agents** (8 agents : game designer ×2, systems ×2, QA ×2, lead programeur ×2) → **fait avant J1**. Décisions de recul détaillées dans [[10.04 Kitsune to Yōkai/Décisions-recul-design]] et fusionnées ci-dessus et dans le PRD.
- Étape 6 (revérification globale) : à confirmer avant la clôture v1.

## Critère de v1

Tous les jalons cochés + passe globale sans régression → v1. Le backlog (coffres, niveaux/boss multiples, difficulté réglable, combo/upgrades) devient la v2.

# Kitsune to Yōkai — Décisions de recul design (post-review adversarial)

Validées par Raf le 2026-09-04, après le design-review adversarial (8 agents : game designer ×2, systems ×2, QA ×2, lead programmer ×2). Ces décisions **modifient** le PRD et le plan.

## 1. Contrôles → auto-aim semi-automatique (consensus 4/8)

**Problème** : un seul stick qui bouge ET vise = impossible de tirer en esquivant, impossible de toucher un ennemi de flanc (tengu, oni). Le scrolling vertical aggrave (on tient le stick à 12h, qui dérive → tir diagonal raté).

**Décision** : stick gauche = **déplacement seul**. Le tir part **automatiquement** vers l'ennemi le plus proche dans un cône devant le kitsune, **snap 8 directions**. Un seul stick (simple, mobile-friendly, fidèle P&R). On peut tirer en esquivant. Bouton tir droite = feu (boule de feu spam).

## 2. Économie de l'éventail → découpler la punition (consensus 4/8)

**Problème** : 20 tirs + perte totale au 1er contact + perte de vie en même temps = double punition qui pousse à thésauriser l'éventail (l'inverse du but). 20 tirs ne survit pas à une vague.

**Décisions** :
- **Plafond 30 tirs** (couvre ~2 vagues).
- **Découpler** : un seul contact → une seule perte. Au contact on **perd une vie** OU on **perd l'éventail**, pas les deux. (Choisi : perd une vie = respawn ; l'éventail est conservé mais avec réserve réduite -1/3.)

## 3. Ramassage d'éventail déjà actif

**Décision** : pas d'empilement. Ramasser un éventail quand on en a déjà un = refus + **+25 pts** (« éventail plein »).

## 4. Mini-boss → pas de pattern fixe prévisible

**Décision** : le saut vise la **position actuelle** du joueur (pas un point fixe). Sous 50% PV → **enrage** (charge → rafale immédiate). Télégraphié mais conditionnel à la position, pour tuer le par-cœur.

## 5. Checkpoints → entre sections uniquement + respawn safe

**Décision** : checkpoints **entre sections**, jamais au milieu d'une vague. Respawn : **filet d'éventail** si le joueur est sans éventail (kappa de secours qui drope) pour éviter le softlock sur le mini-boss. 1,5 s d'invincibilité au respawn.

## 6. Architecture (lead programmer ×2) — intégré aux guidelines

- Découper l'autoload unique en **3** : `GameState` (score/vies/run, PURE, sans ref Node) / `GameEvents` (bus de signaux) / `WaveManager`.
- Extraire le gameplay de `_process` en **classes pures** : `ShootingMath` (vélocité tir = f(direction), testable), `BossBrain` (FSM charge→saut→rafale pure), `WaveDirector` (quels ennemis/quand/où). `_process` n'applique que des décisions.
- Stats en **Resources .tres** (`EnemyStats`, `ProjectileStats`, `PlayerStats`), PAS en export var.
- **Vagues = données** (tuples purs : type, spawn_at_y, intervalle), pas en dur.
- Timer par **delta réel injectable** (pas framecount) → invincibilité/testable.

## 7. Build APK (lead programmer) — intégré au jalon 1

- Godot **4.4.x stable**, JDK **17**, Android SDK, **keystore dédié**, orientation **Landscape bloquée**, ABI **arm64-v8a seule**, viewport **1280×720 stretch canvas_items** (APK léger, rendu net).

## COUPÉ (→ backlog v2) — clapet anti-retour, trop pour MVP

Combo multiplicateur ×8, boss à 2 phases, 4e ennemi (kappa accélérateur), recharge-par-kill, buff à temps limité, 2e stick, sections « nommées ».

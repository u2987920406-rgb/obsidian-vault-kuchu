# Kitsune to Yōkai — PRD

Run-and-gun arcade Android, feeling Pocky and Rocky. Étape 2 (PRD) — 2026-09-03.

## Contexte

- Run-and-gun arcade en **scrolling vertical** (le décor défile vers le bas, on avance vers le haut), un seul perso jouable : le kitsune (renard), écran **paysage**.
- Feelings cibles Pocky and Rocky : tir multi-directionnel, vagues d'ennemis, mini-boss à patterns.

## Objectifs

- **Apprentissage** : Godot, game design d'un run-and-gun, pipeline Android complet (projet → build APK → install téléphone).
- **Plaisir / projet perso** : non commercial, pas de monétisation, pas de store obligatoire.

## Utilisateur

- Raf, seul joueur cible du MVP. Pas de contrainte produit/marché.

## Stack & plateforme

- Moteur : **Godot** (2D)
- Cible : **Android**, livrable **APK installable**
- Orientation : **paysage**
- Contrôles tactiles : **joystick virtuel gauche** (déplacement seul) + **bouton tir droite**. **Tir auto-aim semi-automatique** : part vers l'ennemi le plus proche dans un cône devant le kitsune, snap 8 directions (décision recul → permet de tirer en esquivant, fidèle au feeling) .

## Fonctionnalités (MVP)

### Gameplay
- **Tir 8 directions**, **auto-aim** vers la cible la plus proche dans le cône avant (snap 8 directions).
- **2 armes** :
  - **Boule de feu** : tir rapide, droit, portée longue, dégâts moyens (mitrailleuse, appui maintenu pour spammer)
  - **Éventail magique** : tir lent, large (éventail de projectiles), portée courte, dégâts forts (shotgun), **power-up à ramasser, 30 tirs puis se consomme** (voir décisions de recul)
- **Power-up** : l'éventail **tombe des kappa (tank) et du mini-boss**, et se récupère au ramassage. **Pas d'empilement** : ramasser déjà actif = refus + 25 pts (« éventail plein »). Coffres = v2.
- **Score** : compteur de points par ennemi tué, affiché en haut + à la fin de partie.

### Niveau 1
- **1 niveau** en ~**5 sections-vagues**, montée en difficulté, mini-boss au sommet.
- **3 vies**. **Découplage de punition** : au contact, on perd **une vie** (respawn) OU on perd l'éventail, pas les deux ; si l'éventail est conservé, réserve réduite -1/3.
- **Checkpoints entre sections** (jamais au milieu d'une vague). Respawn : filet d'éventail (kappa de secours si sans éventail) + 1,5 s d'invincibilité.
- Mini-boss : **oni géant**, **saut visant la position du joueur** ; sous 50% PV → enrage (charge → rafale immédiate). Télégraphié mais conditionnel (anti par-cœur).

### Ennemis (3)
- **Kappa** (tortue) : lent, avance droit, tanky. Drops l'éventail.
- **Tengu** (corbeau) : vole, zigzague, tire des projectiles. Sniper.
- **Oni** (ogre) : charge en ligne droite, rapide, fragile. Sprinter.

### Écrans
- Menu titre → niveau → écran victoire (mini-boss battu) / game over.

### Difficulté
- **Douce** : ennemis lents, patterns lisibles (premier niveau non frustrant). Difficulté réglable en v2.

## Contraintes

- Godot 2D, export Android APK.
- Un stick pour viser à 8 directions + bouton tir — mapping tactile simple et lisible.
- Performance OK sur téléphone Android milieu de gamme (sprites 2D simples, pas de 3D).

## Risques

- **Build/export APK Android** (config keys, permissions, orientation) — à valider tôt.
- **Tir 8 directions au stick tactile** : précision du visé — à tester en réel sur téléphone.
- **Feeling P&R** difficile à atteindre (rythme, feedback) ; MVP doux pour ne pas frustrer.

## Backlog v2 (hors MVP)

- Coffres / drops variés
- Autres niveaux, persos, bosses
- Difficulté réglable
- Autres power-ups / armes

## Méthode

- la-methode complète. Prochaine étape : **Guide-lines** (étape 3), puis relecture croisée (étape 4).

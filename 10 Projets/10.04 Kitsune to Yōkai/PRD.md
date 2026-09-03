# Kitsune to Yōkai — PRD

Run-and-gun arcade Android, feeling Pocky and Rocky. Étape 2 (PRD) — 2026-09-03.

## Contexte

- Run-and-gun arcade en **scrolling vertical** (montée), un seul perso jouable : le kitsune (renard).
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
- Contrôles tactiles : **joystick virtuel gauche** (bouger + viser 8 directions) + **bouton tir droite**

## Fonctionnalités (MVP)

### Gameplay
- **Tir 8 directions**, visé au stick
- **2 armes** :
  - **Boule de feu** : tir rapide, droit, portée longue, dégâts moyens (mitrailleuse, appui maintenu pour spammer)
  - **Éventail magique** : tir lent, large (éventail de projectiles), portée courte, dégâts forts (shotgun), **power-up à ramasser, 20 tirs puis se consomme**
- **Power-up** : l'éventail **tombe des kappa (tank) et du mini-boss**, et se récupère au ramassage. Coffres = v2.
- **Score** : compteur de points par ennemi tué, affiché en haut + à la fin de partie.

### Niveau 1
- **1 niveau** en ~**5 sections-vagues**, montée en difficulté, mini-boss au sommet.
- **3 vies**. Touché = perd l'éventail (si actif) + invincibilité courte + respawn au checkpoint. Game over → restart.
- Mini-boss : **oni géant**, pattern en boucle **charge → saut → rafale**.

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

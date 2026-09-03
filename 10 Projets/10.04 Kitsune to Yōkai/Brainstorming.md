# Kitsune to Yōkai — Brainstorming (étape 1)

Run-and-gun arcade Android, feeling Pocky and Rocky. Clôturé le 2026-09-03.

## Décisions actées

- **Feeling P&R** : run-and-gun, tir multi-directionnel, vagues d'ennemis, boss à patterns
- **Perso** : un seul jouable, le kitsune (renard)
- **Tir** : 8 directions
- **Armes** :
  - **Boule de feu** : rapide, droit, portée longue, dégâts moyens (la « mitrailleuse »)
  - **Éventail magique** : lent, large (éventail de projectiles), portée courte, dégâts forts (le « shotgun »)
- **Power-up** : à ramasser (tombent des ennemis/coffres), usage limité — l'éventail = **20 tirs** puis se consomme
- **Niveau 1** : scrolling vertical (montée, vagues d'ennemis, mini-boss au sommet)
- **Ennemis** (3, comportements distincts) :
  - **Kappa** (tortue yōkai) : lent, avance droit, tanky (beaucoup de PV) — le « tank »
  - **Tengu** (corbeau yōkai) : vole, zigzague, tire des projectiles — le « sniper »
  - **Oni** (ogre) : charge en ligne droite, rapide mais fragile — le « sprinter »
- **Mini-boss** : **oni géant**, pattern en boucle : charge → saut → rafale
- **MVP** : 1 niveau, perso + armes, 3 ennemis, mini-boss. Reste = backlog v2

## Méthode

- la-methode complète (brainstorming → PRD → plan → tickets → TDD)
- Cabinet agentique : Hermès orchestre, Freebuff GLM 5.3 (repli Kimi k2.7-code) pour le code, gemma4/qwen3.5 pour la vision

## Prochaine étape

Étape 2 — PRD (contexte, objectifs, fonctionnalités, utilisateurs, contraintes, risques), une question à la fois.

# Kitsune to Yōkai — Bilan v1

**Déclarée le 2026-09-05** après la passe globale validée par Raf.

## Ce qui est livré (MVP jouable)

- **Run-and-gun vertical** style Pocky and Rocky, jouable en **navigateur** (web export Godot, tunnel HTTPS) — plus besoin d'APK pour tester.
- **Kitsune** : déplacement joystick virtuel gauche (fixe, deadzone), tir auto-aim + visée manuelle fluide au bouton droit (double action : press = auto-aim, drag = visée continue lissée, sans snap).
- **2 armes** : boule de feu (rapide/droit) + éventail magique (large/court, 30 tirs, drop kappa).
- **3 ennemis** : kappa (tank), tengu (zigzag), oni (charge) — hordes continues, difficulté croissante.
- **Mini-boss** oni géant à 60 s : charge → saut → rafale, enrage < 50 % PV (70 PV).
- **Boucle complète** : menu → jeu → boss → victoire / game over (arrêt total + rejouer).
- **Progression** : score, XP, niveau (cadence de tir +10 %/niveau), 3 vies, HUD (PV, arme, score, jauge XP).
- **Plein écran** responsive (stretch expand), décor scrolling vertical bouclable (miroir), sprites IA nettoyés (transparence réelle).

## Validation

- Tests E2E headless : **10/10 gameplay + 5/5 boss + 15/15 game over**.
- **Capture Xvfb + analyse vision** avant chaque livraison visuelle (régressions de rendu interceptées).
- Passe globale Raf : flow tenu, feeling « assez plaisant », équilibrage ajusté (difficulté, boss, taille sprites, fluidité visée).

## Leçons de méthode (à retenir)

1. **Déléguer tôt** : bug bloqué 2 échecs → subagent expert immédiatement (le tactile a coûté une journée par entêtement).
2. **Vérifier le rendu, pas que la logique** : les tests headless passaient pendant que le kitsune était invisible (z-order), le voile gris (Panels), le damier (transparence). La capture vision est devenue une porte obligatoire.
3. **Cache web** : le cache-bust (`?v=timestamp`) est indispensable — Raf rejouait des versions fantômes.
4. **Tunnel trycloudflare temporaire** : l'URL change à chaque redémarrage. Pour une URL fixe → tunnel nommé (compte Cloudflare).

## Backlog v2 (non priorisé)

- **Harmonisation sprite/décor** : sprites chibi contours épais vs fond peint détaillé — à unifier (générer un fond assorti, ou sprites top-down via Blender).
- **Sprites top-down 4 directions** (le point d'extension `_orient_sprite` est prêt).
- **Contenu** : niveaux multiples, boss variés, power-ups additionnels, combo.
- **Difficulté réglable** / modes.
- **URL fixe** (tunnel nommé) + éventuel APK signé release.
- **Son / musique** (aucun pour l'instant).

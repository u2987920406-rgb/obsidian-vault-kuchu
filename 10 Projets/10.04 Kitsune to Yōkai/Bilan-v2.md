# Kitsune to Yōkai — Bilan v2

**Déclarée le 2026-09-07**, boucle de rétroaction automatique (Raf : « fini le jeu, je fais le test final »).

## Ce qui est livré (v2)

- **URL fixe** : `https://raf-bmax.tail14baaa.ts.net:8444/` (Tailscale Serve → 8092). Plus jamais de tunnel changeant.
- **Sprites top-down 4 directions** (kitsune up/down/left/right, frame par angle de visée, flip profils) — point d'extension `_orient_sprite` consommé.
- **Ennemis nettoyés** : damier de fond retiré, vraie transparence (`tools/make_game_sprites.py`).
- **Décor Gemini bouclé proprement** : tuile 1536×2752 (temple, pagode, torii, pont, kitsune statues), couture masquée par haie latérale + grandes dalles sur l'allée (`tools/prep_gemini.py`). Désaturé 40%/assombri 80% pour la lisibilité (`tools/desature_fond.py`).
- **Niveau 2** : le même sanctuaire au crépuscule (ambre, vignette) dès le niveau 4 de XP (`tools/make_level2_tile.py`).
- **Power-ups** : bouclier (absorbe 1 coup, aura bleue) + tir triple 10 s (3 boules en éventail serré). Drops : kappa garde l'éventail, 18 % de drop bouclier/triple.
- **Son complet** : 9 SFX synthétisés (`tools/gen_sfx.py`) — tir, fan, hit, mort, drop, hurt, corne de boss, victoire, game over — via AudioManager (pool 6 players, pitch jitter).
- **Boss 2 phases** : sous 50 % PV → vitesse ×1.35, rafales 10 (au lieu de 7), saut qui re-cible en vol.
- **Difficulté lissée** : décroissance exponentielle (plus de cassure à 20 s/45 s), plancher relevé 0.55→0.7.

## Validation

- Tests E2E : **10/10 gameplay + 8/8 powerup + 5/5 boss + 15/15 gameover + fire**.
- QA vision sur chaque livraison (capture 2400×1080, ratio réel du téléphone de Raf).
- Écran titre vérifié : temple lisibles, HUD complet, kitsune de dos, pas d'artefact.

## Leçons v2

1. **La vision locale change tout** : installation Ollama+qwen2.5vl (user-space) → les captures sont VUES, plus seulement mesurées. Les hallucinations du modèle se corrigent par double contrôle (pixels + questions ciblées).
2. **Boucler une image architecturée est impossible par fondu** : la seule méthode fiable est le masquage (haie+dalles) ou la génération native tileable.
3. **Les lambdas multi-lignes GDScript 4 parsent mal** → méthodes nommées pour les connexions de signaux.
4. **tmpfs /tmp limité à 7.2 Go** : les grosses archives (1.3 Go ollama) doivent être supprimées après usage sinon OOM killer sur les gros modèles.

## Reste pour v3 (non priorisé)

- Musique de fond bouclée (les SFX sont en place, il manque la nappe).
- Niveaux multiples (3+ décors), boss variés.
- APK signé release (le build Android existe déjà, il faut le keystore final).
- Combo/score multiplier, leaderboard local.
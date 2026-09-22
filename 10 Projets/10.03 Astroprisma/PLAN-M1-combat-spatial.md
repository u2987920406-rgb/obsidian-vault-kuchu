# PLAN M1 — Combat de vaisseau (Astroprisma maquette tactique)

## Décision Raf
« En partant de cette base il faut pouvoir faire la même chose pour les combats
de vaisseau » — 22/09, après validation du duel au sol. Merge PR #10 fait,
roadmap votée. La boucle QA continue en parallèle sur #6/#8.

## État des lieux (vérifié au code, pas supposé)
- Moteur Rust `space_combat.rs` : `roll_action_dice`, `roll_space_initiative`,
  `resolve_module_activation(die, "6"|"3-5"|"X")`, `simulate_space_turn`,
  `can_board`, `repair_in_combat`, `escape_space_combat`. **Déjà écrit.**
- `wasm_api.rs` n'expose en JS que : `space_is_critical`, `space_escape`,
  `space_repair`, `auto_combat_js`. Les fonctions action-dice/initiative/modules
  ne sont PAS exportées wasm_bindgen → à ajouter (3-4 #[wasm_bindgen] wrappers).
- Données : `starships-db.json` = 18 vaisseaux complets (hull, actions, moves
  d10, skill, mods, class, difficulty). `starship.json` modules + slots.
  `space-combat.json` procédure p.51-52 (initiative GRACE+ACTIONS, enemy acts
  ACTIONS fois/tour, shields consommés, critical ≤10, fuite 2 dés).
- Maquette actuelle (`astroprisma-dsh/app`) : vue duel au sol = base visuelle à
  décliner. Pas de code spatial côté app (engine.js n'a rien).

## Périmètre M1 (MVP livrable sur :8451)
1. **Pont WASM** : exposer `space_roll_initiative`, `space_roll_action_dice`,
   `space_resolve_activation`, `space_enemy_move` (rebuild pkg + symlink).
2. **Vue duel spatiale** (`duel-space.js`) : réutilise le canvas split-screen —
   vaisseau joueur (gauche) / vaisseau ennemi (droite), lasers animés,
   coque+boucliers affichés, dégâts flottants. Même style que duel.js.
3. **HUD Action Dice** : d6 visibles lancés au début DU tour joueur, tap sur un
   dé puis sur un module (SPARK MULTILASERS « 3-5 », etc.) → activation
   vérifiée par le moteur Rust (`resolve_module_activation`), dé consommé.
4. **Tour ennemi** : `simulate_space_turn` ou boucle JS sur moves d10 — le
   vaisseau agit ACTIONS fois (Vector Ace = 2), journal comme le combat au sol.
5. **Écran d'entrée** : depuis la scène tactique, un vaisseau ennemi apparaît
   (déclencheur : bouton « DÉFENSE ORBITALE » ou événement ink).
6. **Fin** : hull 0 = destroyed (loot roll), fuite = 2 Action Dice + GRACE vs
   ACTIONS ennemi, critical ≤10 affiché (icône/teinte).

## Critères d'acceptation (chacun = test TDD dés forcés, rouge avant / vert après)
- AC1 : initiative spatiale — d10+GRA joueur vs d10+ACTIONS ennemi, ordre juste.
- AC2 : Action Dice — nombre = Engines, affiché, consommables une fois.
- AC3 : activation module — dé 4 active « 3-5 » (oui), dé 2 sur « 6 » (non),
  dé X prend la valeur du dé (dégâts = X).
- AC4 : shields — bloquent TOUTE une attaque puis consommés ; « ignoring
  Shields » les ignore.
- AC5 : tour ennemi — Vector Ace agit 2 fois, moves d10 corrects.
- AC6 : critical condition ≤10 détectée (can_board débloqué).
- AC7 : fuite — 2 dés dépensés, GRACE vs ACTIONS, succès = fin sans loot.
- AC8 : hull 0 → destroyed + loot (tier-driven).
- AC9 : mobile — cibles ≥44px, 11px plancher, safe-area (audit-ui 412 passe).
- AC10 : tous les dés passent par le WASM (grep : zéro Math.random dans le js de combat spatial).

## Non-négociables
- Dés 100% Rust/WASM ; TDD dés forcés (verif/*.cjs + PORT_SERVI) ; jamais de
  merge auto (PR → Raf) ; boucle QA : une PR à la fois.
- Style visuel : même charte que la maquette (bandeaux violet, néon, police
  mono, portrait/zone joueur en bas).

## Découpage en PR (taille ~1 issue GitHub chacune)
- PR-a : pont WASM spatial + tests moteur (AC1, AC2, AC3) — fondations.
- PR-b : vue duel spatiale + HUD Action Dice (AC4, AC5, AC9).
- PR-c : entrée de combat, critical, fuite, destruction/loot (AC6-AC8).

## Statut
- [x] Recherche fondations (22/09 11:00) — moteur déjà écrit, WASM à exposer.
- [ ] PR-a en cours.
# ROADMAP — Astroprisma : du sol au spatial (base maquette tactique)

## Contexte

La maquette tactique au sol (https://raf-bmax.tail14baaa.ts.net:8451/) est validée par Raf :
ink (récit) + moteur Rust/WASM (dés) + vue duel Canvas + HUD mobile + tour canonique p.30-32.
C'est la BASE. On l'étend, on ne repart pas de zéro.

## Extensions demandées par Raf (22/09)

1. **Combat de vaisseau** (space-combat.json, p.51+) — vue duel spatiale :
   Action Dice d6 alloués aux modules (préfixe 6 / 3-5 / X), coque, boucliers,
   abordage, fuite. Le moteur Rust a déjà le squelette spatial (wasm_api).
2. **Settlements + factions** (factions.json, settlement.rs déjà dans le moteur) :
   marchands, faveurs (favor system : join à 2 favor), rencontres de faction.
3. **Animation découverte de planète** (starmap.json mappingDiscoveries) :
   petite animation quand on révèle un hex — planète dessinée dans l'hex.

## Ordre proposé (un jalon à la fois, la boucle QA en continu)

### M1 — Combat de vaisseau (le plus gros morceau)
- Vue duel spatiale : deux vaisseaux côte à côte, HUD Action Dice (d6 visibles,
  dépensables sur modules), boucliers/coque, dégâts animés.
- Règles p.51 : roll d6 = Engines, spend sur préfixes, hull, enemy actions.
- Source de vérité : data/space-combat.json + starship.json (modules).
- Test TDD : scénario combat spatial aux dés forcés (rouge avant / vert après).

### M2 — Settlements & marchands
- Écran settlement point-and-click (maquette p.40 « Une escale sous les
  verrières ») : marchand (achat/vente avec scraps), faveurs de faction,
  première rencontre de faction.
- Source : factions.json (favorSystem), equipment/weapons (prix).
- Moteur : settlement.rs existe déjà côté Rust.

### M3 — Animation découverte de planète
- Sur la carte starmap : révélation d'un hex avec animation courte
  (zoom/scan + dessin de la planète dans l'hex), son visuel du style.
- Source : starmap.json mappingDiscoveries.

## Non-négociables (acquis de la base, à conserver partout)
- Tous les dés passent par le moteur Rust/WASM (jamais de Math.random en JS).
- TDD : chaque règle a son test aux dés forcés, rouge avant / vert après.
- Mobile Pixel : cibles ≥44px, plancher 11px, safe-area, pas de tir auto.
- Raf merge les PR ; la boucle QA ne merge jamais.
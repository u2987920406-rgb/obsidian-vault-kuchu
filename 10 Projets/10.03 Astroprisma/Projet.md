# Astroprisma — Projet de jeu (Space Opéra solo)

Source de vérité : le dépôt GitHub (voir ci-dessous). Ce fichier est le résumé
indexé dans le Vault, à tenir à jour quand le projet bouge.

## Nature
**Astroprisma Companion** — application web compagnon pour jouer au jeu de rôle
solo **ASTROPRISMA** (space opéra post-apocalyptique) : fiche de personnage,
vaisseau, carte du système stellaire, Oracle, générateurs procéduraux, assistant
de combat et journal de bord. PWA : utilisable sur PC comme sur téléphone, sans
serveur.

> ASTROPRISMA © Camila Mera / Crescent Chimera. Outil personnel non officiel
> destiné au propriétaire du livre ; les données de jeu ne doivent pas être
> redistribuées.

## Repo & branche active
- Local : `~/projets/Astroprisma_app_EMERGENT`
- GitHub : `u2987920406-rgb/Astroprisma_app_EMERGENT` (privé)
- **Branche active** : `claude/app-status-fmj9ow` (la plus aboutie)
- Autres branches : `main` (quasi vide, 1 commit README), `V1` (auto-commits,
  histoire SANS ancêtre commun avec la branche active)
- Dernier travail : fixes générateurs + harnais de test (voir plus bas)

## Stack technique
- Frontend : **Vite + React 19 + TypeScript + Tailwind 4 + Zustand**, PWA
  (`vite-plugin-pwa`).
- Le jeu est **headless-friendly** : le moteur (dés/générateurs) est du TS pur
  importable hors navigateur → testable par harnais.

## Structure du dépôt
- `app/` — l'application (Vite + React + TS + Tailwind + Zustand, PWA)
- `app/src/engine/` — moteur de dés (`dice.ts`), Challenge Rolls (`rolls.ts`)
- `app/src/engine/types.ts` — modèle de données du jeu (Character, Starship,
  Campaign, Journal…)
- `app/src/data/` — données JSON transcrites depuis le livre (générateurs),
  branchées sur `data/*.json`
- `app/src/features/` — pages : combat, carte (hexmap), Oracle, création,
  fiche, vaisseau, journal
- `app/src/store/game.ts` — store Zustand persistant (`astroprisma-save`)
- `data/` — règles du livre transcrites en JSON structuré (17 fichiers)
- `docs/regles/` — synthèses des règles en français
- `docs/parties/` — rapports de parties jouées (ex. `partie-01.md`)
- `app/scripts/` — scripts : `check-data.mjs` (validation JSON), `playtest-harness.ts` (harnais de jeu)

## Moteur de jeu (à connaître pour tester / développer)
- **Dés** : `rollDie`, `roll`, `rollNotation` — source unique d'aléatoire, tout
  passe par le roll log du journal.
- **Challenge Roll** (`rolls.ts`) : joueur `d10 + stat` **strictement >** dé
  challenge `d10 + stat adverse` → réussite.
- **Combat** : initiative (Challenge Roll GRA), moves ennemis (d10 sur table du
  statblock), dégâts, EXP.
- **Carte** : hexmap axial, 1 tuile STAR centrale + 36 hexes en 3 anneaux
  (Inner 6 / Middle 12 / Outer 18). Voyage = 1 cycle + 1 Fuel.
- **Oracle** : questions fermées (d6) et ouvertes (2d6, table d'inspiration).
- **Générateurs** (`data/generators.ts`) : Hostile/Neutral Encounters, Ring
  Events, Settlements, Planètes/Satellites, Faction Encounters, Abyssal Scars,
  Sidequests, Random Tables (noms PNJ/vaisseaux/planètes), PNJ complet.

## Harnais de jeu automatisé (anti-bug)
Script `app/scripts/playtest-harness.ts`, lancé via **`npm run test:harness`**
(dans `app/`).
- Simule **40 parties × 20 cycles** (~800 cycles) en exerçant le VRAI moteur :
  générateurs, dés, combat, carte, oracle, factions, tables aléatoires.
- Détecte : crash, NaN, tables qui renvoient `?`, invariants violés (fuel/HP
  négatifs, dé non couvert sur les statblocks, positions hors carte).
- Après les fixes ci-dessous : **0 bug**.

## Bugs détectés & réparés (28-08 / 02-09)
1. **`settlementNames` / `satelliteNames` → `? ?`** — les tables de noms
   composés (ex. « Aries Arc ») étaient traitées comme des tables `{die, entries}`,
   mais ce sont des objets indexés par clé (1-20) avec le dé sur la table
   parente. Corrigé dans `genRandomTable` (lit le dé parent + résout les deux
   sous-tables).
2. **`Faction Battles` → `?`** — les rencontres hostiles (d6=6) ont un champ
   `battle`, pas `name`/`ship`/`event`. Ajouté à la chaîne de résolution dans
   `encounterFrom`.
3. **Config Playwright** : le chemin du binaire Chromium (`/opt/pw-browsers/…`)
   était inexistant → tests impossibles. Corrigé vers
   `~/.cache/ms-playwright/chromium-1234/chrome-linux64/chrome`.

Identité de commit sur ce repo : **AstroPrismaXHermes
<AstroPrismaXHermes@users.noreply.github.com>** (posée au niveau repo uniquement).

## Règle de travail (Raf, 2026-09-06)
À **chaque ajout de fonctionnalité** : tester la fonctionnalité en conditions
réelles (e2e Playwright / navigateur) ET vérifier que la tâche est remplie —
pas seulement typecheck/build/tests unitaires. Dire franchement si non testé.

## Mandat refonte complète (Raf, 2026-09-06)
Refonte du jeu en **boucle automatique** : audit complet (toutes les règles
`data/*.json` = source de vérité, rien à inventer) puis implémentation de
chaque parcours **start→end point**, TDD vert + e2e + audit non-régression,
et **COMMIT+PUSH entre chaque fonctionnalité** (branche
`claude/app-status-fmj9ow`, identité AstroPrismaXHermes). Pilotage :
`docs/revue/PILOTAGE.md` + `docs/revue/PLAN-REVUE-2026-09-06.md`.

## Refonte UI/UX — La Méthode (Raf, 2026-09-08)
Refonte UI/UX du jeu via **La Méthode** (skill `la-methode`) : Phase 1
conception (brainstorming → PRD → guidelines → plan jalonné) puis exécution
(tickets, TDD, review). **Le QA agent fait partie de la méthode** : chaque
écran/jalon livré passe par subagent QA + tests + e2e avant validation
humaine. Validation finale = toujours Raf.

### État d'avancement (2026-09-08)
- **Phase 1 close** : maquette validée (20/20 AC QA + visuel Raf), PRD
  (`docs/PRD.md`), guidelines (`docs/GUIDELINES.md`), plan jalonné
  (`docs/PLAN-JALONS.md`), relecture croisée, revérification globale,
  `plan.html` interactif (`docs/ui/plan.html`).
- **Phase 2 close** : spec Jalon 1 (`docs/spec-jalon1.md`) + tickets
  GitHub #12-#18.
- **Jalon 1 — Fondations UI : livré** (issues #12-#18 fermées, commits
  `6bf22ff`→`10f065e`). QA ×2 : audit → 4 écarts corrigés → re-audit PASS.
  Fix responsive mobile (header wrap, dock safe-area) → `9a2a989`.
- **Porte J1→J2 : 6/7** — reste la validation humaine de Raf sur mobile
  (header + dock sans dézoomer).

### Convention porte inter-jalon (inscrite dans La Méthode, 2026-09-08)
7 critères obligatoires avant de passer au jalon suivant :
1. Tickets fermés (AC vérifiées) · 2. Suites vertes (unit+TDD+e2e build prod)
· 3. QA agent PASS · 4. Zéro régression · 5. plan.html à jour (jalon coché +
preuves) · 6. Commit/push propre · 7. Validation humaine Raf.
**Règle : 7/7 = porte franchie. 6/7 = livré mais porte NON franchie.**
+ plan.html mis à jour à chaque jalon (règle inscrite dans la méthode).

## Commandes utiles
- Dev : `cd app && npm run dev`
- Build statique : `npm run build` → `app/dist/` (hébergeable n'importe où)
- Preview : `npm run preview`
- Typecheck : `npm run build` (inclut `tsc -b`) ou `npx tsc -b`
- Test harnais : `npm run test:harness`
- Test e2e (Playwright) : `npm run test:e2e`
- Validation données : `npm run check:data`

## Voir aussi
- [[00 Index]]

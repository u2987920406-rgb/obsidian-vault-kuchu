# Astroprisma — Projet de jeu (Space Opéra solo)

Source de vérité : le dépôt GitHub (voir ci-dessous). Ce fichier est le résumé
indexé dans le Vault, à tenir à jour quand le projet bouge.

> **Dernière mise à jour : 2026-10-01.** Deux chantiers coexistent : l'app **TS**
> (`Astroprisma_app_EMERGENT`, historique, sections ci-dessous) et le **port Unity**
> en C# (`astroprisma-unity`, chantier courant — nouvelle section).

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
- Dernier travail : refonte UI/UX via La Méthode — Jalon 1 + Jalon 2 livrés,
  correctifs UX et pont combat spatial (voir « État d'avancement »).

## Maquette tactique (hors repo ci-dessus)
- Local : `~/projets/astroprisma-dsh` — **hors git** pour l'instant.
- Maquette de combat au sol branchée sur les règles du livre : vue **duel**
  à deux fenêtres (façon Advance Wars) + tour canonique p.30-32 (initiative
  `d10+GRA`, ENEMY MOVE → MAIN → SIDE, HACKs du livre avec Malware).
- Servie en local sur `:5211`, publiée sur le tailnet `:8451`
  (`tailscale serve`, entrée persistée dans `~/docker/stack/serve.sh`).
- Vérification : `bash verif/tout.sh` → 53 OK / 0 FAIL (Chrome réel).
  Détail : [[2026-09-21]].

## Port Unity (chantier courant, 2026-10-01)

Portage de l'app TS vers **Unity 6 / C#**, sprint par sprint (jalons `A1`…`A17`),
avec **gate visuel obligatoire** avant chaque annonce : capture 1080×1920 du
rendu réel + analyse vision. `FAIL` = correction de la cause puis re-capture
(plafond 3 tours).

- Local : `~/projets/astroprisma-unity` (le projet vit dans `AstroprismaUnity/`)
- GitHub : `u2987920406-rgb/astroprisma-unity` (privé) — branche `master`
- **Versionné depuis A10** : commit `f6a4cf3`, 1 044 fichiers ; `.gitignore`
  Unity (Library/Temp/Builds/journaux exclus — 7 Go non versionnés).
- Reference TS : `~/projets/astroprisma-dsh/ref` + `Astroprisma_app_EMERGENT/app/src`
- Plan : [[10 Projets/10.03 Astroprisma/Plans/Plan 01 Projet Astroprisma sansref 30sept26_06h59]]

### Avancement Unity — A1 → A10 livrés

8 sprints + 3 jalons d'écrans, **11 scènes** : StarMap, Combat, SpaceCombat,
Journal, Sheet, Bestiary, Settlement, Factions, Campaigns, EventNovel,
CharacterCreation.

| Jalon | Objet | État |
|---|---|---|
| A1 → A9 | moteur (dés, Challenge Rolls, combat sol + spatial), carte, écrans reliés, bestiaire | livrés |
| **A10** | **journal de bord — un seul fil de campagne** | **livré** |
| A11 → A17 | réputation & sidequests, … | à faire |

**A10 en une phrase** : les journaux locaux (log de combat au sol, log spatial,
journal d'événements) sont unifiés en un **fil unique** daté et situé (cycle +
case) ; les **jets du livre** y entrent verbatim (`JournalKind.Roll`) —
initiative d10+GRA, ✕ROLL d'attaque, ✕ROLL de fuite, Action Dice, d6
d'exploration — une seule fois, aucun doublon au second passage.

### Chiffres (2026-10-01)

- **148** scripts C# (~**23 276** lignes) · **21** fichiers de tests
- **473/473** tests EditMode verts (dont 4 neufs pour A10)
- Lancement des suites : `run-tests.sh`, `run-a10-tests.sh` ; captures :
  `run-sprint<n>.sh`, `run-a9.sh`, `run-a10-combat.sh`

### Méthode (acquis des sprints)

- **Jamais** de Play mode en `-batchmode` sans GPU : `WaitForEndOfFrame` ne se
  déclenche pas, Unity boucle à 1200 % CPU sans écrire de PNG. Chemin prouvé :
  Unity **avec** affichage X (`DISPLAY=:5`), `EditorApplication.Exit` auto.
- Diagnostic : `grep -c "error CS" unity-s<n>.log` **avant** de suspecter le GPU
  (une erreur CS fait bloquer Unity en silence, sans PNG).
- Après un réimport massif, les références de prefab chargées avant sont
  invalidées (`fileID: 0`) → re-câbler en dernier.
- Les initialiseurs de champ dans une `struct` (C# 9 / Unity 6) cassent la
  compilation en silence.
- **Gate visuel** : capturer **le livrable**, pas son voisin. Un combat peut
  détruire le `StarMapController` qui change de scène → réacquérir le
  contrôleur à chaque cycle, et jouer le chemin réel (combat → retour carte →
  bouton JOURNAL) avant de capturer.

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

### État d'avancement (2026-09-14)
- **Phase 1 close** : maquette validée (20/20 AC QA + visuel Raf), PRD
  (`docs/PRD.md`), guidelines (`docs/GUIDELINES.md`), plan jalonné
  (`docs/PLAN-JALONS.md`), relecture croisée, revérification globale,
  `plan.html` interactif (`docs/ui/plan.html`).
- **Phase 2 close** : spec Jalon 1 (`docs/spec-jalon1.md`) + tickets
  GitHub #12-#18.
- **Jalon 1 — Fondations UI : livré** (issues #12-#18 fermées, commits
  `6bf22ff`→`10f065e`). QA ×2 : audit → 4 écarts corrigés → re-audit PASS.
  Fix responsive mobile (header wrap, dock safe-area) → `9a2a989`.
- **Porte J1→J2 : 7/7 — FRANCHIE** (Raf a validé sur mobile, 2026-09-08 21h :
  « Je valide top ✨ »).
- **Jalon 2 — Map & cycles : livré, QA VALIDABLE** (2026-09-09). J2-T0 i18n
  FR/EN (issue #19 fermée) ; carte 100 % FR ; cycle du livre (soin MEDIC,
  −1 Fuel, salaires) ; règle p.27 (hex hostile reste inexploré tant que le
  combat n'est pas résolu) ; traduction FR du livre (~2 200 chaînes joueur :
  origins, crew, enemies, factions, encounters/events, worlds, starships).
  **Re-QA #3 (`deleg_d4ecdf90`) : VALIDABLE** — modules vaisseau 32/32 FR,
  pont hostile→combat spatial 3/3 + 1 scénario faction, e2e 4/4. Rapport
  `audit-jalon1/proofs/qa-i18n3-RAPPORT-JALON2.md` (commit `b72fd97`, docs
  `c2f524f`). **Reste la validation humaine de Raf pour la porte J2→J3.**
- **Correctifs UX + bug combat (2026-09-14, commit `1b9cd77`)** : le combat de
  **vaisseau** ouvrait le combat personnage (détection depuis le livre
  `vaisseau`/`chasseurs` + pont titre EN → vaisseau du catalogue) ; cases
  grisées expliquées (liseré cyan pointillé + raison) ; **roue de réglages**
  unique (thème/infobulles/langue) sur tous les écrans ; création de perso avec
  fil d'étapes + barre collante ; libellés du dialogue d'exploration clarifiés.
  Vérifié en navigateur (dés imposés → événement Medusa reproductible) :
  route `space-combat`, Stingray Frontier engagé, tour joué. Détail :
  [[2026-09-14]].
- **Nouvelle exigence (Raf, 2026-09-08)** : jeu **entièrement en français**,
  toggle langue FR/EN dans Réglages → **J2-T0** (issue #19), posé en tête du
  Jalon 2 pour que tous les écrans à venir naissent bilingues. Inscrit dans
  les guidelines + plan.html.

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


## Jets du livre — 78 branchés (2026-09-17)

Chantier « chaque contexte où un dé doit être lancé pour un impact réel ».
Inventaire : **206** éléments citant un dé dans les 17 JSON → **78 vrais jets**
(issue écrite par le livre). Les 9 domaines sont livrés :

planètes (20) · factions (22) · capacités d'ennemis (16) · HACK au sol (3) ·
fuite spatiale (1) · objets à effet de zone (5) · Cybersphere (4) ·
rencontres neutres (4) · événements d'anneau (1).

Moteur : `app/src/engine/checks.ts` (parse `ROLL`/`JET`, FR **et** EN) +
`CheckPanel` partagé (proposer → lancer → RÉUSSITE/ÉCHEC → conséquence
appliquée). Plan consultable : [[JETS-PLAN.html]] (`docs/JETS-PLAN.html`).

Commits : `1177865`, `e8b861c`, `81207b7`, `afa0c61`, `3b5cbea`, `1124130`,
`79cbe2f`, `4bb1095`, `64c985f`.

Les jets multiples (« Trois Jets de défi ») résolvent désormais les **N** lancers
(commit `3e00bab`). Un choix qui exige une ressource est **refuse** si le joueur
ne l'a pas (commit `0bc1850`). Reste connu : les 6 gabarits CARTE du Cybersphere sont
dérivés (visuels du livre absents).

Serveur de jeu : service **systemd** `astroprisma.service` (Restart=always +
Linger) → http://100.101.17.46:5199/

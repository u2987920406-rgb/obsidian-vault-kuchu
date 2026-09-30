# Plan 01 Projet Astroprisma sansref 30sept26_06h59 — source d'écriture

| Champ | Valeur |
|---|---|
| **Rang** | `01` |
| **Type** | plan principal |
| **Rattaché à** | — |
| **Statut** | en cours |
| **Créé le** | 30sept26_06h59 |
| **Dernière mise à jour** | 30/09/26 07h35 |
| **Code** | `sansref` (dossier `~/projets/astroprisma-unity/` non versionné) |

**Plan : `Plan 01 Projet Astroprisma sansref 30sept26_06h59.html`** — c'est la
couche de validation (onglets, AC cochables, KPI). Ce `.md` est la source
d'écriture, selon le skill `plan-traceable`.

**Ce plan est le plan principal du port Unity** : les grandes lignes (Acquis
J1–J8) et les étapes (jalons J9→J17). Toute demande qui survient en cours de
route devient un **sous-plan** (`Plan 01.SS …`, ex. « plan bestiaire » rattaché à
l'étape 4), jamais un nouveau plan principal — c'est ce qui garde le fil.

- Règle de nommage : `Plan <NN>[.<SS>] Projet <Nom> ref-<sha> <JJmoisAA_HHhMM>`
- `sansref` = le dossier `~/projets/astroprisma-unity/` **n'est pas un dépôt git**
  (`git rev-parse` remonte à `/home/raf/projets`) — information consignée telle quelle.
- Trace vault : `vault/10 Projets/10.03 Astroprisma/Plans/`
- Copie de travail : `~/projets/astroprisma-unity/Plan 01 … 30sept26_06h59.html`
- Servi : `~/projets/Astroprisma_app_EMERGENT/docs/` → http://100.101.17.46:5183/
  (service `astroprisma-plan.service`, python http.server sur `docs/`)
- Index du projet : note-hub [[10.03 Astroprisma]], section « ## Plans »
- Format : template `plan.html` (skills `la-methode` étape 6bis + `template-plan-html`)

## Situation au 2026-09-30 (état du code à cette date)

**LIVRÉ : 8 jalons / sprints, 10 écrans, 450 tests verts, chaque écran passé au
gate visuel** (capture 1080×1920 + vision + mesure pixel).

| Écran | Scène | Tests |
|---|---|---|
| Combat au sol | `Combat.unity` | 268/268 |
| Combat spatial | `SpaceCombat.unity` | 342/342 |
| Escale | `Settlement.unity` | 368/368 |
| Découverte de planète | `StarMap.unity` | 381/381 |
| Factions | `Factions.unity` | 407/407 |
| Fiche d'équipement | `Sheet.unity` | 431/431 |
| Carnet de campagnes | `Campaigns.unity` | 443/443 |
| Bestiaire | `Bestiary.unity` | 450/450 |
| Création | `CharacterCreation.unity` | (Sprint 2-4) |
| Événement | `EventNovel.unity` | (Sprint 8-10) |

Illustrations IA via OpenRouter : 6 portraits d'Origin, 18 vaisseaux, 30 ennemis.

## Reste : 9 jalons (J9 → J17)

| Jalon | Objet | Risque |
|---|---|---|
| **J9** | Bestiaire relié (bouton depuis carte + combat) | Low |
| **J10** | Journal de bord (entrées datées, unification des journaux locaux) | **Hi** |
| **J11** | Réputation & sidequests (5 factions, 5 tables d6 p.60) | Low |
| **J12** | Équipage (crew.json : embauche, rôles, skills, salaires) | Mid |
| **J13** | Vaisseau (fiche, modules, achat/installation) | Mid |
| **J14** | Cybersphere (tech.json : hacks, malware, drones, cybertech) | **Hi** |
| **J15** | Oracle & générateurs (questions d6/2d6, tables aléatoires) | Mid |
| **J16** | Écran Référence | Low |
| **J17** | Passe de parité finale + build Android ARM64 | Mid |

**Ordre proposé** : J9 (petit, rend visible l'existant) → J10/J11 (visibles
côté joueur) → J12/J13 → **J14** (plus gros bloc de contenu) → J15/J16 → J17.

## Constat de départ (vérifié sur disque, 2026-09-30)

- `Rules/Cybersphere/` **vide** — `tech.json` (hacks, masterHacks, malwareTable,
  drones, cybertech) non porté ; `Content/Hacks/` existe mais **vide**.
- `Rules/Crew/` **vide** — `crew.json` (hiring, crewRules, skills, baseRoles) non porté.
- `random-tables.json` (29 Ko) + `generators-*.json` (106 Ko) : aucune table
  aléatoire portée (`grep random-tables` → 0 résultat).
- Journal : 21 fichiers citent « journal », mais ce sont des **journaux locaux**
  (settlement, combat, espace) — pas un journal de bord de campagne.
- `Rules/Events/Oracle.cs` existe (les 6 réponses de la question fermée, ordre du
  livre) mais **aucun écran** ne l'expose.

## Chaîne des plans

- **Plan 01** (ce fichier) — 30sept26_06h59 — état : 8 livrés / 9 restants.
- Plan précédent : aucun.

## Voir aussi

- [[10.03 Astroprisma]] · [[10.03 Astroprisma/Projet]]
- [[30 Journal/2026-09-30]] — livraisons du jour et leçons de méthode

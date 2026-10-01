# Plan 01 Projet Astroprisma sansref 30sept26_06h59 — source d'écriture

| Champ | Valeur |
|---|---|
| **Rang** | `01` |
| **Lettre** | `A` — les jalons de ce plan sont `A1`…`A17` |
| **Statut** | en cours |
| **Créé le** | 30sept26_06h59 |
| **Dernière mise à jour** | 01/10/26 23h50 |
| **Code** | `sansref` (dossier `~/projets/astroprisma-unity/`, dépôt privé `astroprisma-unity`) |

**Plan : `Plan 01 Projet Astroprisma sansref 30sept26_06h59.html`** — c'est la
couche de validation (onglets, AC cochables, KPI). Ce `.md` est la source
d'écriture, selon le skill `plan-traceable`.

**Ce plan est le plan INITIAL du port Unity** (les grandes lignes, lettre A).
Les plans sont **successifs** : quand une demande survient en cours de route — un
bestiaire à compléter, une partie omise — elle donne un **nouveau plan daté**
(`Plan 02`, lettre B, jalons `B1`…), pas un sous-plan. `Plan 01` reste la
référence initiale ; l'index dit lequel est le plus récent.

- Règle de nommage : `Plan <NN> Projet <Nom> ref-<sha> <JJmoisAA_HHhMM>`
- Le dossier `~/projets/astroprisma-unity/` **est désormais un dépôt git** (privé
  `astroprisma-unity`, créé au jalon A10, commit `f6a4cf3`). Le nom du plan porte
  encore `sansref` : c'est l'état à sa création, conservé tel quel.
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

## Livré depuis : A10 (journal de bord — un seul fil de campagne)

**A10 est livré** (vérifié : 473/473 tests EditMode, dont 4 neufs). Les journaux
locaux (log de combat au sol, log spatial, journal d'événements) sont unifiés en
un fil unique, daté et situé (cycle + case). Les **jets du livre** y entrent
verbatim (`JournalKind.Roll`) : initiative d10+GRA, ✕ROLL d'attaque, ✕ROLL de
fuite, tirage des Action Dice, d6 d'exploration — écrits une seule fois, aucun
doublon au second passage. Gate visuel : capture 1080×1920 du fil après un vrai
parcours (combat gagné → retour carte → bouton JOURNAL).

Projet versionné : dépôt privé `astroprisma-unity` créé et poussé (commit `f6a4cf3`,
1 044 fichiers ; `.gitignore` Unity — Library/Temp/Builds/journaux exclus).

## Reste : 7 jalons (A11 → A17)

| Jalon | Objet | Risque |
|---|---|---|
| **A11** | Réputation & sidequests (5 factions, 5 tables d6 p.60) | Low |
| **A12** | Équipage (crew.json : embauche, rôles, skills, salaires) | Mid |
| **A13** | Vaisseau (fiche, modules, achat/installation) | Mid |
| **A14** | Cybersphere (tech.json : hacks, malware, drones, cybertech) | **Hi** |
| **A15** | Oracle & générateurs (questions d6/2d6, tables aléatoires) | Mid |
| **A16** | Écran Référence | Low |
| **A17** | Passe de parité finale + build Android ARM64 | Mid |

**Ordre proposé** : A9 (petit, rend visible l'existant) → A10/A11 (visibles
côté joueur) → A12/A13 → **A14** (plus gros bloc de contenu) → A15/A16 → A17.

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

- **Plan 01** (ce fichier, lettre A) — 30sept26_06h59 — état : 8 livrés (A1–A8) / 9 restants (A9–A17).
- Plan précédent : aucun — c'est le plan initial.
- Plan suivant : à créer quand une demande le justifie (`nommer_plan.sh` → `Plan 02`, lettre B).

## Voir aussi

- [[10.03 Astroprisma]] · [[10.03 Astroprisma/Projet]]
- [[30 Journal/2026-09-30]] — livraisons du jour et leçons de méthode

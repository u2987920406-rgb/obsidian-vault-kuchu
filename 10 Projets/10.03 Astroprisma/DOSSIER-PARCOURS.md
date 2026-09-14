# Dossier des parcours — start points → endpoints

> **Fichier généré** par `npm run gen:parcours`. Ne pas éditer à la main.
> Source de vérité : `data/*.json` (le livre). Aucune route inventée.

**Start points recensés : 243** · **choix : 294**

## Matrice de couverture

| Section | Start points | Jet | Combat sol | Combat spatial | Achat | Soin | Réparation | Ravitaillement | Gain | Dépense | Favor | Déplac. | Marquage | Activité | Mort | Aucun |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Événement d’anneau | 36 | 1 | 4 | 5 | 3 | 7 | 1 | 4 | 27 | 19 | 14 | 4 | 0 | 0 | 0 | 4 |
| Hostile | 30 | 0 | 30 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Neutre | 30 | 4 | 2 | 0 | 3 | 4 | 1 | 0 | 13 | 8 | 3 | 5 | 4 | 0 | 0 | 6 |
| Planète | 72 | 49 | 5 | 1 | 0 | 1 | 0 | 0 | 10 | 11 | 3 | 0 | 0 | 0 | 0 | 18 |
| Colonies & activités | 9 | 0 | 0 | 0 | 3 | 2 | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 6 | 0 | 0 |
| Faction | 60 | 0 | 36 | 3 | 2 | 9 | 3 | 0 | 17 | 9 | 10 | 2 | 1 | 0 | 0 | 0 |
| Cybersphere | 6 | 6 | 6 | 0 | 0 | 0 | 0 | 0 | 18 | 6 | 0 | 0 | 0 | 6 | 6 | 0 |

## 1. Anneau intérieur (d6=2 → parité → d6)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Chaleur extrême** `innerRing/odd/d6=1` | Exploration d6=2 → innerRing → parité odd → d6=1 | JET VIGOR | Dépense, Gain | — | — |
| **Colonnes de feu** `innerRing/odd/d6=2` | Exploration d6=2 → innerRing → parité odd → d6=2 | JET GRACE | Dépense, Gain | — | — |
| **Incendie à bord** `innerRing/odd/d6=3` | Exploration d6=2 → innerRing → parité odd → d6=3 | JET TECH | Gain, Soin, Dépense | — | — |
| **Vague de radiation solaire** `innerRing/odd/d6=4` | Exploration d6=2 → innerRing → parité odd → d6=4 | JET MIND | Dépense, Gain, Soin | — | — |
| **Explosion de la surface solaire** `innerRing/odd/d6=5` | Exploration d6=2 → innerRing → parité odd → d6=5 | Encaisser le coup | Dépense, Réparation | — | — |
| **Explosion de la surface solaire** `innerRing/odd/d6=5` | Exploration d6=2 → innerRing → parité odd → d6=5 | Puiser dans les réserves de carburant | Dépense | — | — |
| **Signal de détresse (Transporteur Birotor)** `innerRing/odd/d6=6` | Exploration d6=2 → innerRing → parité odd → d6=6 | Réparer leur vaisseau | Dépense | — | — |
| **Signal de détresse (Transporteur Birotor)** `innerRing/odd/d6=6` | Exploration d6=2 → innerRing → parité odd → d6=6 | Transporter l’équipage vers la Colonie | Gain | — | — |
| **Contrebandiers de carburant corsaires à la ferme hélios** `innerRing/even/d6=1` | Exploration d6=2 → innerRing → parité even → d6=1 | Attendre ton tour pour le ravitaillement | Soin, Dépense | — | — |
| **Contrebandiers de carburant corsaires à la ferme hélios** `innerRing/even/d6=1` | Exploration d6=2 → innerRing → parité even → d6=1 | Payer le carburant illégal | Dépense | — | — |
| **Récepteurs d’énergie sphériques** `innerRing/even/d6=2` | Exploration d6=2 → innerRing → parité even → d6=2 | Connect | Soin | — | — |
| **Fragment de sphère de Dyson inachevée** `innerRing/even/d6=3` | Exploration d6=2 → innerRing → parité even → d6=3 | Puiser dans la réserve d’énergie | Ravitaillement, Soin | — | — |
| **STATION RED SOL** `innerRing/even/d6=4` | Exploration d6=2 → innerRing → parité even → d6=4 | Ravitailler ton vaisseau | Dépense | — | — |
| **STATION RED SOL** `innerRing/even/d6=4` | Exploration d6=2 → innerRing → parité even → d6=4 | Entrer au bar | Gain, Achat | — | — |
| **Frontière Raie de Medusa embusquée** `innerRing/even/d6=5` | Exploration d6=2 → innerRing → parité even → d6=5 | Combattre l’embuscade rebelle | Combat spatial, Ravitaillement, Favor, Soin | /space-combat | — |
| **Frontière Raie de Medusa embusquée** `innerRing/even/d6=5` | Exploration d6=2 → innerRing → parité even → d6=5 | Se joindre à l’attaque | Combat spatial, Ravitaillement, Favor | /space-combat | — |
| **Ferme solaire en croissant endommagée** `innerRing/even/d6=6` | Exploration d6=2 → innerRing → parité even → d6=6 | JET TECH | Ravitaillement | — | — |
| **Ferme solaire en croissant endommagée** `innerRing/even/d6=6` | Exploration d6=2 → innerRing → parité even → d6=6 | JET MIND | Soin | — | — |

## 2. Anneau intermédiaire

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Vaisseau terraformeur ISF** `middleRing/odd/d6=1` | Exploration d6=2 → middleRing → parité odd → d6=1 | Terminer la mission | Favor, Déplac. | — | — |
| **Incendie du yacht de luxe (Transporteur Shell-4)** `middleRing/odd/d6=2` | Exploration d6=2 → middleRing → parité odd → d6=2 | Vendre des BurnPatches | Dépense, Gain | — | — |
| **Vaisseau-colonie en cryo- capsules** `middleRing/odd/d6=3` | Exploration d6=2 → middleRing → parité odd → d6=3 | Les réveiller | Aucun | — | — |
| **Vaisseau-colonie en cryo- capsules** `middleRing/odd/d6=3` | Exploration d6=2 → middleRing → parité odd → d6=3 | Les laisser tranquilles | Dépense, Gain | — | — |
| **Survivante du laboratoire cyber** `middleRing/odd/d6=4` | Exploration d6=2 → middleRing → parité odd → d6=4 | L’emmener à une Colonie | Favor, Gain | — | — |
| **Vaisseau fantôme Titan Tarrasque** `middleRing/odd/d6=5` | Exploration d6=2 → middleRing → parité odd → d6=5 | Négocier avec les hors-la-loi | Gain | — | — |
| **Vaisseau fantôme Titan Tarrasque** `middleRing/odd/d6=5` | Exploration d6=2 → middleRing → parité odd → d6=5 | Combattre les ennemis | Combat spatial, Gain | /space-combat | — |
| **Vaisseau-génération Edgecharger W** `middleRing/odd/d6=6` | Exploration d6=2 → middleRing → parité odd → d6=6 | Trade | Achat | — | — |
| **Avant-poste militaire improvisé WARG (Transporteur Bélouga)** `middleRing/even/d6=1` | Exploration d6=2 → middleRing → parité even → d6=1 | Apporter du soutien | Favor, Gain | — | — |
| **Avant-poste militaire improvisé WARG (Transporteur Bélouga)** `middleRing/even/d6=1` | Exploration d6=2 → middleRing → parité even → d6=1 | Infiltrer le vaisseau | Favor, Gain | — | — |
| **Inspection de checkpoint ISF (Edgecharger W)** `middleRing/even/d6=2` | Exploration d6=2 → middleRing → parité even → d6=2 | Tout est en ordre | Aucun | — | — |
| **Inspection de checkpoint ISF (Edgecharger W)** `middleRing/even/d6=2` | Exploration d6=2 → middleRing → parité even → d6=2 | Révéler la cargaison | Favor, Gain | — | — |
| **Inspection de checkpoint ISF (Edgecharger W)** `middleRing/even/d6=2` | Exploration d6=2 → middleRing → parité even → d6=2 | Corrompre les soldats | Dépense | — | — |
| **Inspection de checkpoint ISF (Edgecharger W)** `middleRing/even/d6=2` | Exploration d6=2 → middleRing → parité even → d6=2 | Vaincre les soldats | Favor | — | — |
| **Stations de recherche de la planète naine** `middleRing/even/d6=3` | Exploration d6=2 → middleRing → parité even → d6=3 | Aider la scientifique | Combat sol, Gain | /combat | — |
| **Station médicale de l’Apôtre Synth** `middleRing/even/d6=4` | Exploration d6=2 → middleRing → parité even → d6=4 | Yes | Favor | — | — |
| **Station médicale de l’Apôtre Synth** `middleRing/even/d6=4` | Exploration d6=2 → middleRing → parité even → d6=4 | No | Combat sol, Favor | /combat | — |
| **Station de recherche computationnelle NGHTMR** `middleRing/even/d6=5` | Exploration d6=2 → middleRing → parité even → d6=5 | Les aider à craquer le code | Favor, Jet | — | — |
| **Station de recherche computationnelle NGHTMR** `middleRing/even/d6=5` | Exploration d6=2 → middleRing → parité even → d6=5 | Les combattre | Combat sol, Favor | /combat | — |
| **Bunker du PROJET ATLAS** `middleRing/even/d6=6` | Exploration d6=2 → middleRing → parité even → d6=6 | Utiliser l’unité de chirurgie | Dépense | — | — |

## 3. Anneau extérieur

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Essais de l’Intercepteur Epsilon** `outerRing/odd/d6=1` | Exploration d6=2 → outerRing → parité odd → d6=1 | Se rendre | Dépense | — | — |
| **Essais de l’Intercepteur Epsilon** `outerRing/odd/d6=1` | Exploration d6=2 → outerRing → parité odd → d6=1 | Attaquer le Pirate spatial | Gain | — | — |
| **Contrôle d’identité du Prédateur stellaire** `outerRing/odd/d6=2` | Exploration d6=2 → outerRing → parité odd → d6=2 | Acheter ta liberté | Dépense | — | — |
| **Contrôle d’identité du Prédateur stellaire** `outerRing/odd/d6=2` | Exploration d6=2 → outerRing → parité odd → d6=2 | Affronter le Pirate spatial | Combat sol, Gain | /combat | — |
| **Contrôle d’identité du Prédateur stellaire** `outerRing/odd/d6=2` | Exploration d6=2 → outerRing → parité odd → d6=2 | Sous condition de Faveur (+2 Corsaire) | Favor, Gain | — | — |
| **Raider Speeder de contrebandier** `outerRing/odd/d6=3` | Exploration d6=2 → outerRing → parité odd → d6=3 | Attaquer le vaisseau | Gain, Favor | — | — |
| **Sniper Delta Blizzard** `outerRing/odd/d6=4` | Exploration d6=2 → outerRing → parité odd → d6=4 | Affronter le vaisseau | Combat spatial, Gain | /space-combat | — |
| **Sniper Delta Blizzard** `outerRing/odd/d6=4` | Exploration d6=2 → outerRing → parité odd → d6=4 | Fuir la rencontre | Dépense, Déplac. | — | — |
| **Vétéran de la Mante Vector-7** `outerRing/odd/d6=5` | Exploration d6=2 → outerRing → parité odd → d6=5 | S’échapper par un champ d’astéroïdes | Dépense | — | — |
| **Vétéran de la Mante Vector-7** `outerRing/odd/d6=5` | Exploration d6=2 → outerRing → parité odd → d6=5 | Combattre le vaisseau | Combat spatial, Gain | /space-combat | — |
| **Base militaire pirate du Vieux Monde** `outerRing/odd/d6=6` | Exploration d6=2 → outerRing → parité odd → d6=6 | Réclamer le Voyageur A-1 | Aucun | — | — |
| **Essaim d’astéroïdes accélérés** `outerRing/even/d6=1` | Exploration d6=2 → outerRing → parité even → d6=1 | JET GRACE | Dépense, Gain | — | — |
| **Champ de débris artificiels** `outerRing/even/d6=2` | Exploration d6=2 → outerRing → parité even → d6=2 | Scavenge | Gain | — | — |
| **Cellule WARG cachée et Duskwing rebelle** `outerRing/even/d6=3` | Exploration d6=2 → outerRing → parité even → d6=3 | Combattre le vaisseau rebelle | Aucun | — | — |
| **Cellule WARG cachée et Duskwing rebelle** `outerRing/even/d6=3` | Exploration d6=2 → outerRing → parité even → d6=3 | Sous condition de Faveur (+1 W.A.R.G.) | Déplac. | — | — |
| **Course des corsaires dans les astéroïdes** `outerRing/even/d6=4` | Exploration d6=2 → outerRing → parité even → d6=4 | Visiter le bar | Gain, Achat | — | — |
| **Course des corsaires dans les astéroïdes** `outerRing/even/d6=4` | Exploration d6=2 → outerRing → parité even → d6=4 | Course de vaisseaux | Gain | — | — |
| **Station de la Synth Arch dans un champ artificiel** `outerRing/even/d6=5` | Exploration d6=2 → outerRing → parité even → d6=5 | Sous condition de Faveur (+3 Synth Arch) | Gain, Déplac. | — | — |
| **Planque de contrebandiers dans l’océan d’astéroïdes** `outerRing/even/d6=6` | Exploration d6=2 → outerRing → parité even → d6=6 | Piller la cache | Gain | — | — |

## 4. Rencontres hostiles (d6=3 → catégorie → d6)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Speeder de contrebandier** `hostile/Outlaws & Looters/d6=1` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=1 | Combat ; gagne 1 Colis de contrebande après les avoir vaincus. | Combat sol | /combat | — |
| **Transporteur Bélouga** `hostile/Outlaws & Looters/d6=2` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=2 | Combat. | Combat sol | /combat | — |
| **Transporteur Birotor** `hostile/Outlaws & Looters/d6=3` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=3 | Combat. | Combat sol | /combat | — |
| **Mite Orion** `hostile/Outlaws & Looters/d6=4` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=4 | Combat. | Combat sol | /combat | — |
| **Transporteur Shell-4** `hostile/Outlaws & Looters/d6=5` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=5 | Combat. | Combat sol | /combat | — |
| **Edgecharger W (vaisseau-mère)** `hostile/Outlaws & Looters/d6=6` | Exploration d6=3 → catégorie d6=2 (Outlaws & Looters) → d6=6 | Combat. | Combat sol | /combat | — |
| **Duskwing** `hostile/Space Pirates/d6=1` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=1 | Combat. | Combat sol | /combat | — |
| **Mante Vector-7** `hostile/Space Pirates/d6=2` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=2 | Combat. | Combat sol | /combat | — |
| **Delta Blizzard** `hostile/Space Pirates/d6=3` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=3 | Combat. | Combat sol | /combat | — |
| **Intercepteur Epsilon** `hostile/Space Pirates/d6=4` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=4 | Combat ; gagne 1 Alcool zéro-G après les avoir vaincus. | Combat sol | /combat | — |
| **Prédateur stellaire** `hostile/Space Pirates/d6=5` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=5 | Combat. | Combat sol | /combat | — |
| **Titan Tarrasque** `hostile/Space Pirates/d6=6` | Exploration d6=3 → catégorie d6=3 (Space Pirates) → d6=6 | Combat ; la fuite est impossible. | Combat sol | /combat | — |
| **Chasseur As Vector (modifié)** `hostile/Mercenaries/d6=1` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=1 | Combat. | Combat sol | /combat | — |
| **Delta Blizzard** `hostile/Mercenaries/d6=2` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=2 | Combat. | Combat sol | /combat | — |
| **Mante Vector-7** `hostile/Mercenaries/d6=3` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=3 | Combat. | Combat sol | /combat | — |
| **Mite Orion (chasseur de primes)** `hostile/Mercenaries/d6=4` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=4 | Combat ; gagne un Exosquelette de combat après l’avoir vaincu. | Combat sol | /combat | — |
| **Prédateur stellaire** `hostile/Mercenaries/d6=5` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=5 | Combat. | Combat sol | /combat | — |
| **Edgecharger W (transporteur)** `hostile/Mercenaries/d6=6` | Exploration d6=3 → catégorie d6=4 (Mercenaries) → d6=6 | Combat. | Combat sol | /combat | — |
| **Intercepteur Scarabée** `hostile/Spacefarers/d6=1` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=1 | Combat. | Combat sol | /combat | — |
| **Twinrotor Hauler** `hostile/Spacefarers/d6=2` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=2 | Combat ; gagne un Tesseract en les vainquant. | Combat sol | /combat | — |
| **Transporteur Bélouga** `hostile/Spacefarers/d6=3` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=3 | Combat. | Combat sol | /combat | — |
| **Voyageur A-1** `hostile/Spacefarers/d6=4` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=4 | Combat. | Combat sol | /combat | — |
| **Gardien Eclipse** `hostile/Spacefarers/d6=5` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=5 | Combat. | Combat sol | /combat | — |
| **Frontière Raie** `hostile/Spacefarers/d6=6` | Exploration d6=3 → catégorie d6=5 (Spacefarers) → d6=6 | Combat. | Combat sol | /combat | — |
| **?** `hostile/Faction Battles/d6=1` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=1 | (combat direct) | Combat sol | /combat | ✅ |
| **?** `hostile/Faction Battles/d6=2` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=2 | (combat direct) | Combat sol | /combat | ✅ |
| **?** `hostile/Faction Battles/d6=3` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=3 | (combat direct) | Combat sol | /combat | ✅ |
| **?** `hostile/Faction Battles/d6=4` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=4 | (combat direct) | Combat sol | /combat | ✅ |
| **?** `hostile/Faction Battles/d6=5` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=5 | (combat direct) | Combat sol | /combat | ✅ |
| **?** `hostile/Faction Battles/d6=6` | Exploration d6=3 → catégorie d6=6 (Faction Battles) → d6=6 | (combat direct) | Combat sol | /combat | ✅ |

## 5. Rencontres neutres (d6=4 → catégorie → d6)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Chasseur d’avant-guerre** `neutral/Derelict Ships/d6=1` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=1 | Gagne un Carabine à ions et un mod d’arme à distance aléatoire. | Gain | — | — |
| **Transporteur congelé** `neutral/Derelict Ships/d6=2` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=2 | Gagne 2 Trousse de soins. | Gain, Soin | — | — |
| **Vieux vaisseau de forage d’astéroïdes** `neutral/Derelict Ships/d6=3` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=3 | Trouve 50 Sérum dans un casier. | Gain | — | — |
| **Transporteur fissuré** `neutral/Derelict Ships/d6=4` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=4 | JET VIGOR — Réussite : détruis la tourelle de défense automatique [+1 XP] ; Échec : perds 2d4 Santé. | Combat sol, Gain, Jet, Dépense | /combat | — |
| **Deux intercepteurs écrasés** `neutral/Derelict Ships/d6=5` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=5 | Récupère 60 Ferraille. | Gain | — | — |
| **Vaisseau de contrebande endommagé** `neutral/Derelict Ships/d6=6` | Exploration d6=4 → catégorie d6=2 (Derelict Ships) → d6=6 | Gagne 2 narcobiotiques aléatoires et une cache secrète avec 100 Sérum. | Gain | — | — |
| **Artisan armurier (Transporteur Bélouga)** `neutral/Cargo Transport/d6=1` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=1 | Tu peux acheter n’importe quelle arme ou mod d’arme. | Achat | — | — |
| **Signal de détresse (Transporteur Bélouga)** `neutral/Cargo Transport/d6=2` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=2 | JET GRACE — Réussite : récupère leur cargaison perdue ; ils t’offrent 30 Ferraille et 70 Sérum. | Dépense, Jet | — | — |
| **Marchand de sang itinérant (Transporteur Birotor)** `neutral/Cargo Transport/d6=3` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=3 | Tu peux perdre 4 Santé pour gagner 80 Sérum, répétable jusqu’à 5 fois. | Gain, Soin, Dépense | — | — |
| **Transporteur de checkpoint (Transporteur Birotor)** `neutral/Cargo Transport/d6=4` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=4 | Tu peux acheter n’importe quel objet ou Tenue d’armure. | Achat | — | — |
| **Transporteur Shell-4 pillé par des pirates** `neutral/Cargo Transport/d6=5` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=5 | Récupère un module de vaisseau Rayons disrupteurs. | Aucun | — | — |
| **Équipage de clones androïdes (Transporteur Shell-4)** `neutral/Cargo Transport/d6=6` | Exploration d6=4 → catégorie d6=3 (Cargo Transport) → d6=6 | Tu peux acheter n’importe quel cybertech pour son coût en Sérum plus 10 Énergie, au lieu du coût en XP affiché. | Achat, Dépense, Gain | — | — |
| **Survivants d’une bataille (Intercepteur Scarabée)** `neutral/Civilian Transport/d6=1` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=1 | Tu peux leur donner 100 Sérum ; gagne +1 Faveur Medusa si tu le fais. | Dépense, Favor | — | — |
| **Révolutionnaires WARG (Mante Vector-7)** `neutral/Civilian Transport/d6=2` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=2 | If you know one, you may reveal the location of a WARG Settlement or a Pirate Hideout; gain +1 W.A.R.G. Favor if you do. | Favor, Marquage | — | — |
| **Vaisseau de réfugiés (Delta Blizzard)** `neutral/Civilian Transport/d6=3` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=3 | You may provide 40 Scraps for repairs; they reveal the location of a Sylvanian planet, 2 tiles north. | Gain, Réparation, Marquage, Déplac. | — | — |
| **Voyageur A-1 infesté** `neutral/Civilian Transport/d6=4` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=4 | Élimine 3 Rampants du vide ; l’équipage te récompense avec 100 Sérum et 50 Ferraille. | Gain | — | — |
| **Mite Orion à la dérive** `neutral/Civilian Transport/d6=5` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=5 | Tu peux perdre 5 Carburant pour gagner un DRONE_Coccinelle. | Dépense | — | — |
| **Vaisseau-hôpital ISF (Edgecharger W)** `neutral/Civilian Transport/d6=6` | Exploration d6=4 → catégorie d6=4 (Civilian Transport) → d6=6 | Tu peux donner 2 Trousses de soins ; gagne +1 Faveur ISF si tu le fais. | Favor, Gain, Soin | — | — |
| **Alerte à ogive vieille de décennies** `neutral/Radio Signals/d6=1` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=1 | Une Lune nucléaire est désormais localisée à 1 tuile au sud-est (aux coordonnées de la colonie lunaire). | Marquage, Déplac. | — | — |
| **Le Silence** `neutral/Radio Signals/d6=2` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=2 | Tire une Rencontre hostile sur le prochain hex inexploré. | Aucun | — | — |
| **Résonance du vide spatial** `neutral/Radio Signals/d6=3` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=3 | JET TECH — Réussite : sauve les systèmes du vaisseau [+1 XP] ; Échec : perds 8 Coque. | Dépense, Gain, Jet | — | — |
| **Signal du réseau d’urgence** `neutral/Radio Signals/d6=4` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=4 | Une Colonie Synth à 1 tuile au sud-ouest passe sous contrôle WARG. | Déplac. | — | — |
| **Code morse hexadécimal** `neutral/Radio Signals/d6=5` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=5 | JET MIND — Réussite : marque l’emplacement d’une Colonie Medusa, à 2 tuiles au sud-est. | Jet, Marquage, Déplac. | — | — |
| **Station de radio pirate** `neutral/Radio Signals/d6=6` | Exploration d6=4 → catégorie d6=5 (Radio Signals) → d6=6 | Aucun effet mécanique. | Aucun | — | — |
| **Comète de passage** `neutral/Supranatural Events/d6=1` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=1 | Gagne 10 XP et restaure ta Santé et ton Énergie à 20. | Gain, Soin | — | — |
| **Rampant du vide dans la salle des machines** `neutral/Supranatural Events/d6=2` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=2 | Attaquer : combats la créature. La laisser se nourrir : elle se téléporte dans l’espace ; perds 4 Carburant. | Combat sol, Dépense | /combat | — |
| **Horloge à l’envers** `neutral/Supranatural Events/d6=3` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=3 | Tu te réveilles exactement d6 ans plus jeune. | Aucun | — | — |
| **Prophétie du Gardien Eclipse** `neutral/Supranatural Events/d6=4` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=4 | No mechanical effect. | Aucun | — | — |
| **Prisme de cristal rose** `neutral/Supranatural Events/d6=5` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=5 | Gagne un +1 permanent à une stat de ton choix. | Aucun | — | — |
| **Champ électromagnétique étrange** `neutral/Supranatural Events/d6=6` | Exploration d6=4 → catégorie d6=6 (Supranatural Events) → d6=6 | Ton vaisseau est téléporté vers l’hex de ton choix. | Déplac. | — | — |

## 6. Planètes (d6=5 → type → site/rencontre)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Gaïenne — Mer de dunes** `planet/gaian/spot-1` | Exploration d6=5 → planète d6=1 → site d6=1 | Explorer le site | Jet | — | — |
| **Gaïenne — Oasis** `planet/gaian/spot-2` | Exploration d6=5 → planète d6=1 → site d6=2 | Explorer le site | Jet | — | — |
| **Gaïenne — Cité souterraine** `planet/gaian/spot-3` | Exploration d6=5 → planète d6=1 → site d6=3 | Explorer le site | Jet | — | — |
| **Gaïenne — Camp nomade** `planet/gaian/spot-4` | Exploration d6=5 → planète d6=1 → site d6=4 | Explorer le site | Jet | — | — |
| **Gaïenne — Carcasse de ver géant** `planet/gaian/spot-5` | Exploration d6=5 → planète d6=1 → site d6=5 | Explorer le site | Jet | — | — |
| **Gaïenne — Raffinerie de minéraux** `planet/gaian/spot-6` | Exploration d6=5 → planète d6=1 → site d6=6 | Explorer le site | Jet | — | — |
| **Gaïenne — Enlisement dans les sables mouvants** `planet/gaian/enc-1` | Exploration d6=5 → planète d6=1 → rencontre d6=1 | Enlisement dans les sables mouvants | Jet, Gain, Dépense | — | — |
| **Gaïenne — Le Vagabond** `planet/gaian/enc-2` | Exploration d6=5 → planète d6=1 → rencontre d6=2 | Le Vagabond | Aucun | — | — |
| **Gaïenne — Cauchemar ailé** `planet/gaian/enc-3` | Exploration d6=5 → planète d6=1 → rencontre d6=3 | Cauchemar ailé | Combat sol | /combat | — |
| **Gaïenne — Pilleurs de joyaux** `planet/gaian/enc-4` | Exploration d6=5 → planète d6=1 → rencontre d6=4 | Pilleurs de joyaux | Combat sol, Favor | /combat | — |
| **Gaïenne — Perdu dans le mirage** `planet/gaian/enc-5` | Exploration d6=5 → planète d6=1 → rencontre d6=5 | Perdu dans le mirage | Jet, Dépense | — | — |
| **Gaïenne — Le Temple du désert** `planet/gaian/enc-6` | Exploration d6=5 → planète d6=1 → rencontre d6=6 | Le Temple du désert | Aucun | — | — |
| **Calorienne — Plateforme pétrolière** `planet/calorian/spot-1` | Exploration d6=5 → planète d6=2 → site d6=1 | Explorer le site | Jet | — | — |
| **Calorienne — Champ d’antennes** `planet/calorian/spot-2` | Exploration d6=5 → planète d6=2 → site d6=2 | Explorer le site | Jet | — | — |
| **Calorienne — Canyon** `planet/calorian/spot-3` | Exploration d6=5 → planète d6=2 → site d6=3 | Explorer le site | Jet | — | — |
| **Calorienne — Mine de fer** `planet/calorian/spot-4` | Exploration d6=5 → planète d6=2 → site d6=4 | Explorer le site | Jet | — | — |
| **Calorienne — Ravin géant** `planet/calorian/spot-5` | Exploration d6=5 → planète d6=2 → site d6=5 | Explorer le site | Jet | — | — |
| **Calorienne — Rover téléguidé** `planet/calorian/spot-6` | Exploration d6=5 → planète d6=2 → site d6=6 | Explorer le site | Jet | — | — |
| **Calorienne — Observatoire Alpha 2** `planet/calorian/enc-1` | Exploration d6=5 → planète d6=2 → rencontre d6=1 | Observatoire Alpha 2 | Jet, Favor, Dépense | — | — |
| **Calorienne — Fissure au sol** `planet/calorian/enc-2` | Exploration d6=5 → planète d6=2 → rencontre d6=2 | Fissure au sol | Jet, Gain, Dépense | — | — |
| **Calorienne — Butin de guerre** `planet/calorian/enc-3` | Exploration d6=5 → planète d6=2 → rencontre d6=3 | Butin de guerre | Gain | — | — |
| **Calorienne — Équipe de nuit sans fin** `planet/calorian/enc-4` | Exploration d6=5 → planète d6=2 → rencontre d6=4 | Équipe de nuit sans fin | Jet, Gain, Dépense | — | — |
| **Calorienne — Le Martien** `planet/calorian/enc-5` | Exploration d6=5 → planète d6=2 → rencontre d6=5 | Le Martien | Aucun | — | — |
| **Calorienne — Escarmouche au Fort Météore** `planet/calorian/enc-6` | Exploration d6=5 → planète d6=2 → rencontre d6=6 | Escarmouche au Fort Météore | Aucun | — | — |
| **Vaporienne — Station spatiale** `planet/vaporian/spot-1` | Exploration d6=5 → planète d6=3 → site d6=1 | Explorer le site | Jet | — | — |
| **Vaporienne — Raffinerie de vapeurs** `planet/vaporian/spot-2` | Exploration d6=5 → planète d6=3 → site d6=2 | Explorer le site | Jet | — | — |
| **Vaporienne — Astéroïde aménagé** `planet/vaporian/spot-3` | Exploration d6=5 → planète d6=3 → site d6=3 | Explorer le site | Jet | — | — |
| **Vaporienne — Lune terraformée** `planet/vaporian/spot-4` | Exploration d6=5 → planète d6=3 → site d6=4 | Explorer le site | Jet | — | — |
| **Vaporienne — Mégastructure en anneau** `planet/vaporian/spot-5` | Exploration d6=5 → planète d6=3 → site d6=5 | Explorer le site | Jet | — | — |
| **Vaporienne — Planque secrète** `planet/vaporian/spot-6` | Exploration d6=5 → planète d6=3 → site d6=6 | Explorer le site | Jet | — | — |
| **Vaporienne — Dans l’œil de la tempête** `planet/vaporian/enc-1` | Exploration d6=5 → planète d6=3 → rencontre d6=1 | Dans l’œil de la tempête | Jet, Dépense, Gain | — | — |
| **Vaporienne — Relais orbital** `planet/vaporian/enc-2` | Exploration d6=5 → planète d6=3 → rencontre d6=2 | Relais orbital | Aucun | — | — |
| **Vaporienne — Distillerie de Sérum** `planet/vaporian/enc-3` | Exploration d6=5 → planète d6=3 → rencontre d6=3 | Distillerie de Sérum | Aucun | — | — |
| **Vaporienne — Mer de vapeurs** `planet/vaporian/enc-4` | Exploration d6=5 → planète d6=3 → rencontre d6=4 | Mer de vapeurs | Jet, Combat spatial, Dépense, Gain | /space-combat | — |
| **Vaporienne — Le Marchand étrange** `planet/vaporian/enc-5` | Exploration d6=5 → planète d6=3 → rencontre d6=5 | Le Marchand étrange | Aucun | — | — |
| **Vaporienne — Mort ou vif** `planet/vaporian/enc-6` | Exploration d6=5 → planète d6=3 → rencontre d6=6 | Mort ou vif | Aucun | — | — |
| **Aquarienne — Îlots rocheux** `planet/aquarian/spot-1` | Exploration d6=5 → planète d6=4 → site d6=1 | Explorer le site | Jet | — | — |
| **Aquarienne — Avant-poste de recherche marine** `planet/aquarian/spot-2` | Exploration d6=5 → planète d6=4 → site d6=2 | Explorer le site | Jet | — | — |
| **Aquarienne — Cité flottante** `planet/aquarian/spot-3` | Exploration d6=5 → planète d6=4 → site d6=3 | Explorer le site | Jet | — | — |
| **Aquarienne — Installation sous-marine secrète** `planet/aquarian/spot-4` | Exploration d6=5 → planète d6=4 → site d6=4 | Explorer le site | Jet | — | — |
| **Aquarienne — Archipel** `planet/aquarian/spot-5` | Exploration d6=5 → planète d6=4 → site d6=5 | Explorer le site | Jet | — | — |
| **Aquarienne — Récif corallien** `planet/aquarian/spot-6` | Exploration d6=5 → planète d6=4 → site d6=6 | Explorer le site | Jet | — | — |
| **Aquarienne — Vague après vague** `planet/aquarian/enc-1` | Exploration d6=5 → planète d6=4 → rencontre d6=1 | Vague après vague | Jet, Gain, Dépense | — | — |
| **Aquarienne — Nautilus** `planet/aquarian/enc-2` | Exploration d6=5 → planète d6=4 → rencontre d6=2 | Nautilus | Aucun | — | — |
| **Aquarienne — Installation TEL 22** `planet/aquarian/enc-3` | Exploration d6=5 → planète d6=4 → rencontre d6=3 | Installation TEL 22 | Jet, Dépense | — | — |
| **Aquarienne — L’échouage** `planet/aquarian/enc-4` | Exploration d6=5 → planète d6=4 → rencontre d6=4 | L’échouage | Aucun | — | — |
| **Aquarienne — Monstre des profondeurs** `planet/aquarian/enc-5` | Exploration d6=5 → planète d6=4 → rencontre d6=5 | Monstre des profondeurs | Combat sol | /combat | — |
| **Aquarienne — Prison sous-marine** `planet/aquarian/enc-6` | Exploration d6=5 → planète d6=4 → rencontre d6=6 | Prison sous-marine | Aucun | — | — |
| **Sylvanienne — Grotte de la bête** `planet/sylvanian/spot-1` | Exploration d6=5 → planète d6=5 → site d6=1 | Explorer le site | Jet | — | — |
| **Sylvanienne — Branches entrelacées** `planet/sylvanian/spot-2` | Exploration d6=5 → planète d6=5 → site d6=2 | Explorer le site | Jet | — | — |
| **Sylvanienne — Chutes d’eau géantes** `planet/sylvanian/spot-3` | Exploration d6=5 → planète d6=5 → site d6=3 | Explorer le site | Jet | — | — |
| **Sylvanienne — Savane** `planet/sylvanian/spot-4` | Exploration d6=5 → planète d6=5 → site d6=4 | Explorer le site | Jet | — | — |
| **Sylvanienne — Marécages** `planet/sylvanian/spot-5` | Exploration d6=5 → planète d6=5 → site d6=5 | Explorer le site | Jet | — | — |
| **Sylvanienne — Temple de pierre** `planet/sylvanian/spot-6` | Exploration d6=5 → planète d6=5 → site d6=6 | Explorer le site | Jet | — | — |
| **Sylvanienne — Colonie ravagée** `planet/sylvanian/enc-1` | Exploration d6=5 → planète d6=5 → rencontre d6=1 | Colonie ravagée | Aucun | — | — |
| **Sylvanienne — En territoire ennemi** `planet/sylvanian/enc-2` | Exploration d6=5 → planète d6=5 → rencontre d6=2 | En territoire ennemi | Aucun | — | — |
| **Sylvanienne — Chlorophylle hostile** `planet/sylvanian/enc-3` | Exploration d6=5 → planète d6=5 → rencontre d6=3 | Chlorophylle hostile | Jet, Gain, Dépense | — | — |
| **Sylvanienne — La course-poursuite dans la forêt** `planet/sylvanian/enc-4` | Exploration d6=5 → planète d6=5 → rencontre d6=4 | La course-poursuite dans la forêt | Aucun | — | — |
| **Sylvanienne — Parkour dans la canopée** `planet/sylvanian/enc-5` | Exploration d6=5 → planète d6=5 → rencontre d6=5 | Parkour dans la canopée | Jet, Gain, Soin, Dépense | — | — |
| **Sylvanienne — Grotte alien** `planet/sylvanian/enc-6` | Exploration d6=5 → planète d6=5 → rencontre d6=6 | Grotte alien | Combat sol | /combat | — |
| **Écuménopolis — Quartier législatif** `planet/ecumenopolis/spot-1` | Exploration d6=5 → planète d6=6 → site d6=1 | Explorer le site | Jet | — | — |
| **Écuménopolis — Légions de gratte-ciel** `planet/ecumenopolis/spot-2` | Exploration d6=5 → planète d6=6 → site d6=2 | Explorer le site | Jet | — | — |
| **Écuménopolis — Ruelles étroites** `planet/ecumenopolis/spot-3` | Exploration d6=5 → planète d6=6 → site d6=3 | Explorer le site | Jet | — | — |
| **Écuménopolis — Décharge** `planet/ecumenopolis/spot-4` | Exploration d6=5 → planète d6=6 → site d6=4 | Explorer le site | Jet | — | — |
| **Écuménopolis — Tour polygonale** `planet/ecumenopolis/spot-5` | Exploration d6=5 → planète d6=6 → site d6=5 | Explorer le site | Jet | — | — |
| **Écuménopolis — Ascenseur spatial** `planet/ecumenopolis/spot-6` | Exploration d6=5 → planète d6=6 → site d6=6 | Explorer le site | Jet | — | — |
| **Écuménopolis — Parc municipal** `planet/ecumenopolis/enc-1` | Exploration d6=5 → planète d6=6 → rencontre d6=1 | Parc municipal | Aucun | — | — |
| **Écuménopolis — Patrouille commerciale ISF** `planet/ecumenopolis/enc-2` | Exploration d6=5 → planète d6=6 → rencontre d6=2 | Patrouille commerciale ISF | Aucun | — | — |
| **Écuménopolis — Instinct primaire** `planet/ecumenopolis/enc-3` | Exploration d6=5 → planète d6=6 → rencontre d6=3 | Instinct primaire | Jet, Favor | — | — |
| **Écuménopolis — Mech de combat MK-2 SX** `planet/ecumenopolis/enc-4` | Exploration d6=5 → planète d6=6 → rencontre d6=4 | Mech de combat MK-2 SX | Aucun | — | — |
| **Écuménopolis — Coincé dans les égouts** `planet/ecumenopolis/enc-5` | Exploration d6=5 → planète d6=6 → rencontre d6=5 | Coincé dans les égouts | Jet, Combat sol, Gain | /combat | — |
| **Écuménopolis — Fuite de données** `planet/ecumenopolis/enc-6` | Exploration d6=5 → planète d6=6 → rencontre d6=6 | Fuite de données | Aucun | — | — |

## 7. Factions (d6=6 → d10 faction → Favor → d6)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **W.A.R.G. — A WARG rebel outpost on a small asteroid** `faction/warg/neutral/d6=1` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=1 | A WARG rebel outpost on a small asteroid station invites you into their encampment. | Gain, Favor, Soin, Marquage | — | — |
| **W.A.R.G. — A modified Snowstorm Delta carrying a WA** `faction/warg/neutral/d6=2` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=2 | A modified Snowstorm Delta carrying a WARG rebel squadron asks for spare armament. | Gain, Dépense | — | — |
| **W.A.R.G. — A battle-scarred Duskwing fighter with d** `faction/warg/neutral/d6=3` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=3 | A battle-scarred Duskwing fighter with damaged navigation sensors asks for repairs. | Favor, Soin, Réparation, Dépense | — | — |
| **W.A.R.G. — A Vector-7 Mantis pilot on an infiltrati** `faction/warg/neutral/d6=4` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=4 | A Vector-7 Mantis pilot on an infiltration mission proposes to trade ships with you. | Combat spatial | /space-combat | — |
| **W.A.R.G. — A Vector-7 Mantis chased by ISF security** `faction/warg/neutral/d6=5` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=5 | A Vector-7 Mantis chased by ISF security begs you to take his illegal explosive cargo. | Combat spatial, Gain | /space-combat | — |
| **W.A.R.G. — A WARG recon squad's Starpredator is bei** `faction/warg/neutral/d6=6` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor ≥ 0 → d6=6 | A WARG recon squad's Starpredator is being devoured from within by a Mutated Chimaera hatched from an alien egg in their cargo hold. | Combat spatial, Gain | /space-combat | — |
| **W.A.R.G. — A high-precision radio antenna on a near** `faction/warg/hostile/d6=1` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=1 | A high-precision radio antenna on a nearby mesoplanet base has located your ship; two Vector Ace Fighters launch to destroy it. | Combat sol | /combat | — |
| **W.A.R.G. — A lone Rebel Fighter fabricating explosi** `faction/warg/hostile/d6=2` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=2 | A lone Rebel Fighter fabricating explosives on a man-made moon attacks you from a speeder bike. | Combat sol | /combat | — |
| **W.A.R.G. — A Guerrilla Commander who infiltrated yo** `faction/warg/hostile/d6=3` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=3 | A Guerrilla Commander who infiltrated your ship at your last stop sets off an explosive aboard and attacks you from the cargo hold. | Combat sol | /combat | — |
| **W.A.R.G. — Crossing guerrilla-controlled airspace, ** `faction/warg/hostile/d6=4` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=4 | Crossing guerrilla-controlled airspace, a Vector-7 Mantis and a Duskwing attack you from both flanks. | Combat sol | /combat | — |
| **W.A.R.G. — Rebels ambush you at an abandoned space ** `faction/warg/hostile/d6=5` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=5 | Rebels ambush you at an abandoned space station: EMP inhibitors let two Rebel Fighters warp inside your ship; a WARG Major General teleports in when the last Rebel Fighter is defeated. | Combat sol | /combat | — |
| **W.A.R.G. — A gigantic WARG mothership (a captured, ** `faction/warg/hostile/d6=6` | Exploration d6=6 → d10 faction (W.A.R.G.) → Favor < 0 → d6=6 | A gigantic WARG mothership (a captured, re-armored Tarrasque Titan) prepares to attack. | Combat sol | /combat | — |
| **Intersolar Federation — A Twinrotor Hauler circles helplessly: M** `faction/isf/neutral/d6=1` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=1 | A Twinrotor Hauler circles helplessly: Medusa hackers infected its navigation and stole its data. | Gain | — | — |
| **Intersolar Federation — A veteran ISF Sentinel captaining a Shel** `faction/isf/neutral/d6=2` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=2 | A veteran ISF Sentinel captaining a Shell-4 Transporter merchant ship invites you to dock and trade. | Achat | — | — |
| **Intersolar Federation — A merchant on a docked Edgecharger W wit** `faction/isf/neutral/d6=3` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=3 | A merchant on a docked Edgecharger W with a failed engine offers to pay for a delivery. | Gain | — | — |
| **Intersolar Federation — A Vector Ace Fighter and a Twinrotor Hau** `faction/isf/neutral/d6=4` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=4 | A Vector Ace Fighter and a Twinrotor Hauler laying stealth mines before a battle against WARG ask for help, paying in Serum. | Combat sol, Gain, Dépense | /combat | — |
| **Intersolar Federation — An ISF Beluga Transporter with a malfunc** `faction/isf/neutral/d6=5` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=5 | An ISF Beluga Transporter with a malfunctioning warp engine asks for help; fix it and you keep their teleportation drive. | Favor, Soin, Dépense | — | — |
| **Intersolar Federation — An orbital laboratory station hosting th** `faction/isf/neutral/d6=6` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor ≥ 0 → d6=6 | An orbital laboratory station hosting three Scientists and two ISF Soldiers; a scientist offers to fully restore your Health and proposes a research project. | Gain | — | — |
| **Intersolar Federation — A glass-domed research station's securit** `faction/isf/hostile/d6=1` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=1 | A glass-domed research station's security recognizes you in the ISF criminal database; two ISF Soldiers corner you in a dead end. | Combat sol | /combat | — |
| **Intersolar Federation — An ISF checkpoint cargo scan identifies ** `faction/isf/hostile/d6=2` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=2 | An ISF checkpoint cargo scan identifies your spaceship; the ISF Sentinel inspecting your ship is ordered to attack. | Combat sol | /combat | — |
| **Intersolar Federation — On a gem-covered small moon you stumble ** `faction/isf/hostile/d6=3` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=3 | On a gem-covered small moon you stumble on a clandestine clone-labor mining operation; a guard alerts 2 Mercenaries who open fire. | Combat sol | /combat | — |
| **Intersolar Federation — On a remote frozen colony, two patrollin** `faction/isf/hostile/d6=4` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=4 | On a remote frozen colony, two patrolling ISF Soldiers pass by while you hide in an abandoned building. ROLL GRACE. Success: they leave after a few minutes and you return to your ship. Failure: the soldiers find your hiding spot. | Combat sol | /combat | — |
| **Intersolar Federation — The security cameras of a nuclear reacto** `faction/isf/hostile/d6=5` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=5 | The security cameras of a nuclear reactor station identify you; the ISF Trade Baron overseeing it confronts you himself to claim your bounty. | Combat sol | /combat | — |
| **Intersolar Federation — Pushed into a gravitational well, you fa** `faction/isf/hostile/d6=6` | Exploration d6=6 → d10 faction (Intersolar Federation) → Favor < 0 → d6=6 | Pushed into a gravitational well, you face an ISF-operated Tarrasque Titan mothership tasked with destroying your ship. | Combat sol | /combat | — |
| **Medusa Sector — A Medusa-aligned Scientist in a laborato** `faction/medusa/neutral/d6=1` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=1 | A Medusa-aligned Scientist in a laboratory Twinrotor Hauler needs an undamaged Tesseract to study the artifact. | Combat sol, Favor, Gain, Déplac. | /combat | — |
| **Medusa Sector — An empty Medusa data server station orbi** `faction/medusa/neutral/d6=2` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=2 | An empty Medusa data server station orbits the system's star: a password-locked control room and a large computer room. | Gain | — | — |
| **Medusa Sector — Medusa operatives aboard three Scarab In** `faction/medusa/neutral/d6=3` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=3 | Medusa operatives aboard three Scarab Interceptors extract data from a large space telescope and ask to see your exploration data. | Favor, Gain | — | — |
| **Medusa Sector — A Drone Operator tries to weld new titan** `faction/medusa/neutral/d6=4` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=4 | A Drone Operator tries to weld new titanium panels onto her looter-damaged A-1 Voyager. | Soin, Dépense | — | — |
| **Medusa Sector — An A-1 Voyager captured from Space Pirat** `faction/medusa/neutral/d6=5` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=5 | An A-1 Voyager captured from Space Pirates by Medusa agents was damaged in a previous battle; the pilot asks for repairs. | Favor, Réparation | — | — |
| **Medusa Sector — A Stingray Frontier fitted with full-imm** `faction/medusa/neutral/d6=6` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor ≥ 0 → d6=6 | A Stingray Frontier fitted with full-immersion virtual reality capsules used by NetHackers to train their mental skills; you may try one. | Favor, Gain, Soin | — | — |
| **Medusa Sector — In a preserved Old World drone factory o** `faction/medusa/hostile/d6=1` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=1 | In a preserved Old World drone factory of the defunct VARON-SAITO INDUSTRIES, two hostile Drone Operators scavenging for parts confront you. | Combat sol | /combat | — |
| **Medusa Sector — A hacked civilian Scarab Interceptor tar** `faction/medusa/hostile/d6=2` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=2 | A hacked civilian Scarab Interceptor targets you with its own missiles. | Combat sol | /combat | — |
| **Medusa Sector — A NetHacker sent by Medusa attacks your ** `faction/medusa/hostile/d6=3` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=3 | A NetHacker sent by Medusa attacks your vision with a cybernetic glitch virus. | Combat sol | /combat | — |
| **Medusa Sector — On a rocky centaur under an artificial z** `faction/medusa/hostile/d6=4` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=4 | On a rocky centaur under an artificial zero-gravity field, a Cyber-Terrorist attacks you. | Combat sol | /combat | — |
| **Medusa Sector — The PERSEUS II malware has taken over a ** `faction/medusa/hostile/d6=5` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=5 | The PERSEUS II malware has taken over a Medusa control center; its infected android leader attacks you mercilessly. | Combat sol | /combat | — |
| **Medusa Sector — On a deserted O'Neill cylinder station, ** `faction/medusa/hostile/d6=6` | Exploration d6=6 → d10 faction (Medusa Sector) → Favor < 0 → d6=6 | On a deserted O'Neill cylinder station, a dead Cyborg puppeteered remotely through its implants by a Medusa malware attacks you savagely. | Combat sol | /combat | — |
| **Corsair Syndicate — A smuggler dodging a cargo control check** `faction/corsair/neutral/d6=1` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=1 | A smuggler dodging a cargo control checkpoint aboard a Smuggler Speeder offers to trade if you keep his position secret. | Achat | — | — |
| **Corsair Syndicate — 'THE BULLDOZER', a rusty Edgecharger W h** `faction/corsair/neutral/d6=2` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=2 | 'THE BULLDOZER', a rusty Edgecharger W hosting a clandestine boxing ring on a nomadic route. | Combat sol, Gain, Favor, Soin | /combat | — |
| **Corsair Syndicate — A chrome Orion Moth hosts illegal raves ** `faction/corsair/neutral/d6=3` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=3 | A chrome Orion Moth hosts illegal raves with Syndicate-supplied narcobiotics, out of ISF regulation range. | Dépense, Gain, Soin | — | — |
| **Corsair Syndicate — Two Syndicate Mercenaries race their mod** `faction/corsair/neutral/d6=4` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=4 | Two Syndicate Mercenaries race their modified starships and take bets. | Favor | — | — |
| **Corsair Syndicate — Sirio Hex, a legendary Bounty Hunter abo** `faction/corsair/neutral/d6=5` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=5 | Sirio Hex, a legendary Bounty Hunter aboard a dragon-figurehead Starpredator, offers you a Syndicate job. | Gain, Favor | — | — |
| **Corsair Syndicate — An Edgecharger W converted into a makesh** `faction/corsair/neutral/d6=6` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor ≥ 0 → d6=6 | An Edgecharger W converted into a makeshift zoo for rare alien creatures by an ISF Trade Baron turned smuggler. | Combat sol, Gain | /combat | — |
| **Corsair Syndicate — Looters and raiders ambush your starship** `faction/corsair/hostile/d6=1` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=1 | Looters and raiders ambush your starship at a captured outpost crossing: a Smuggler Speeder crashes into your wing dealing 5 Hull damage and stalling your engines, and a Looter breaks into your ship. | Combat sol | /combat | — |
| **Corsair Syndicate — A hostile Starpredator of the LINESTAR G** `faction/corsair/hostile/d6=2` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=2 | A hostile Starpredator of the LINESTAR GROUP, a mercenary paramilitary outfit hired to destroy competition on illegal trade routes, closes in on your ship. | Combat sol | /combat | — |
| **Corsair Syndicate — At a space saloon, a bartender gifts you** `faction/corsair/hostile/d6=3` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=3 | At a space saloon, a bartender gifts you a free Zero-G Liquor; before you can drink, a Bounty Hunter walks in whispering your name. | Combat sol | /combat | — |
| **Corsair Syndicate — Two Space Pirates attack you at the main** `faction/corsair/hostile/d6=4` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=4 | Two Space Pirates attack you at the main gate of an abandoned space dock their gang took over. | Combat sol | /combat | — |
| **Corsair Syndicate — A chain of EMP mines disables your elect** `faction/corsair/hostile/d6=5` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=5 | A chain of EMP mines disables your electronics; two Mercenaries teleport inside your spacecraft from a nearby mothership and force the gates to your control room. | Combat sol | /combat | — |
| **Corsair Syndicate — An O2 system failure forces a stop at a ** `faction/corsair/hostile/d6=6` | Exploration d6=6 → d10 faction (Corsair Syndicate) → Favor < 0 → d6=6 | An O2 system failure forces a stop at a remote asteroid, which turns out to be the secret hideout of an Outer Ring Crime Boss on the run; he opens fire. | Combat sol | /combat | — |
| **Synth Arch — An abandoned shipyard turned titanium ch** `faction/synthArch/neutral/d6=1` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=1 | An abandoned shipyard turned titanium chapel, where a Synth Apostle preaches the transhumanist creed to human followers. | Déplac. | — | — |
| **Synth Arch — A breached derelict ship holds a dead An** `faction/synthArch/neutral/d6=2` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=2 | A breached derelict ship holds a dead Android Titan, killed by a bounty hunter while transporting a Tesseract to the nearest Settlement. | Gain, Soin | — | — |
| **Synth Arch — A Snowstorm Delta low on fuel carries sy** `faction/synthArch/neutral/d6=3` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=3 | A Snowstorm Delta low on fuel carries synthetic human clones escaped from a forced-labor factory on an orbiting asteroid, 4 tiles northeast. | Combat sol, Dépense | /combat | — |
| **Synth Arch — On a moon's dark side, twelve Android Sy** `faction/synthArch/neutral/d6=4` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=4 | On a moon's dark side, twelve Android Synths and a Synth Apostle perform a ritual to upload a dead human's consciousness. | Soin, Dépense | — | — |
| **Synth Arch — A damaged STARLINE-series Automaton Bot,** `faction/synthArch/neutral/d6=5` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=5 | A damaged STARLINE-series Automaton Bot, an Old World ship-repair machine, drifts through space; you may haul it in and try to fix it. | Dépense, Réparation | — | — |
| **Synth Arch — In an abandoned brutalist factory, a tor** `faction/synthArch/neutral/d6=6` | Exploration d6=6 → d10 faction (Synth Arch) → Favor ≥ 0 → d6=6 | In an abandoned brutalist factory, a torn-apart Android Synth reports being ambushed by a Mutated Chimaera during a reconnaissance mission. | Combat sol | /combat | — |
| **Synth Arch — A gravity jammer on a small asteroid for** `faction/synthArch/hostile/d6=1` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=1 | A gravity jammer on a small asteroid formation slows your ship; when you land to disable it, two Android Synths ambush you in payback for past confrontations with the Arch. | Combat sol | /combat | — |
| **Synth Arch — A remote-controlled Epsilon Interceptor ** `faction/synthArch/hostile/d6=2` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=2 | A remote-controlled Epsilon Interceptor warship bearing the Synth Arch crest warps in, piloted remotely by Android Apostles from their HQ, and fires its particle cannons. | Combat sol | /combat | — |
| **Synth Arch — On a derelict low-gravity space station,** `faction/synthArch/hostile/d6=3` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=3 | On a derelict low-gravity space station, a Cyborg ex-space marine snipes at you with a modified gravity rifle, then charges with inhuman speed. | Combat sol | /combat | — |
| **Synth Arch — At a deserted recon satellite you siphon** `faction/synthArch/hostile/d6=4` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=4 | At a deserted recon satellite you siphon its reserves (gain 6 Energy), unknowingly activating the security protocol: an Android Titan attacks you. | Combat sol | /combat | — |
| **Synth Arch — The auto-pilot AI of a civilian Eclipse ** `faction/synthArch/hostile/d6=5` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=5 | The auto-pilot AI of a civilian Eclipse Warden has turned hostile, killed its crew, and attacks your spacecraft to take control of it too. | Combat sol | /combat | — |
| **Synth Arch — In a deserted pyramid-shaped station lit** `faction/synthArch/hostile/d6=6` | Exploration d6=6 → d10 faction (Synth Arch) → Favor < 0 → d6=6 | In a deserted pyramid-shaped station littered with the bodies of mercenaries sent to capture an experimental android, a Synth Apostle attacks you before you reach the computer room. | Combat sol | /combat | — |

## 8. Colonies & activités (d6=1)

| Start point | Déclenché par | Choix | Endpoints | Route | Manuel |
|---|---|---|---|---|---|
| **Hangar** `settlement/hangar` | Exploration d6=1 → Settlement → onglet Hangar | Hangar | Réparation, Ravitaillement, Achat | — | — |
| **Wiredoc** `settlement/wiredoc` | Exploration d6=1 → Settlement → onglet Wiredoc | Wiredoc | Soin, Achat | — | — |
| **Trading Hub** `settlement/tradingHub` | Exploration d6=1 → Settlement → onglet Trading Hub | Trading Hub | Achat | — | — |
| **Home Pods** `settlement/homePods` | Exploration d6=1 → Settlement → onglet Home Pods | Home Pods | Soin, Activité | — | — |
| **QG de faction** `settlement/headquarters` | Exploration d6=1 → Settlement → onglet QG de faction | QG de faction | Favor, Activité | — | — |
| **Test Flight** `settlement/activity/Test Flight` | Settlement → Activités | Test Flight | Activité | — | — |
| **Cybersphere** `settlement/activity/Cybersphere` | Settlement → Activités | Cybersphere | Activité | — | — |
| **Scrapyard** `settlement/activity/Scrapyard` | Settlement → Activités | Scrapyard | Activité | — | — |
| **Combat Sim** `settlement/activity/Combat Sim` | Settlement → Activités | Combat Sim | Activité | — | — |

# 10.06 Panda roux — Animation (course, queue, saut)

Suite du projet [[10.06 Panda roux]] : après la modélisation (v7), on a monté
un **rig** puis écrit une **animation** : course, queue qui remue, et saut
par-dessus un petit ruisseau.

## Livrables

- Rig : `~/projets/panda-roux/panda_roux_rig.blend` — 23 os
- Animation : `~/projets/panda-roux/panda_roux_saut.blend` — 132 images à 24 i/s
- Vidéo : `~/projets/panda-roux/renders/panda_ruisseau.mp4`
- Planches : `renders/planche_v7.png`, `renders/sequence_saut.png`
- Scripts : `scripts/rig_panda.py`, `scripts/build_run.py`, `scripts/skin_lib.py`

## Structure du squelette

    root
     +- hanches
         +- colonne - poitrine - cou - tete - oreille_G / oreille_D
         |           +- epaule_av - bras_av - pied_av    (patte avant, 3 os)
         +- hanche_ar - cuisse_ar - pied_ar              (patte arriere, 3 os)
         +- queue1 - queue2 - queue3

Les 27 détails (yeux, nez, oreilles, coussinets, moustaches) sont attachés
**rigidement** à un seul os : un globe oculaire doit rester sphérique, une
pondération douce le déformerait.

## Les 3 pièges du rig et de l'animation

### 1. La « peau automatique » de Blender peut être muette

`bpy.ops.object.parent_set(type="ARMATURE_AUTO")` (bone heat) a créé **23
groupes de sommets tous vides** sur ce maillage de 369 000 sommets — **sans
lever la moindre erreur**. Le rig paraissait correct, mais aucun sommet ne
bougeait (déplacement mesuré : 0,00 mm partout).

Correctif : pondération calculée (distance aux os, `1/d⁴`, 4 influences,
lissage sur 3 itérations du maillage) et **vérification de la somme des poids**
après coup. Toujours vérifier un rig par une mesure, jamais par l'apparence.

### 2. `to_mesh()` renvoie des coordonnées LOCALES

Mesurer la hauteur du personnage via le maillage évalué ne voit **pas**
`arm.location` : l'espace local d'un maillage parenté à l'armature n'en dépend
pas. Résultat : le contact au sol « dérivait » et le panda flottait malgré une
correction affichée. Correctif : appliquer explicitement `matrix_world`.

### 3. Contacter le sol : boucle de convergence

Pose appliquée → abaisser l'armature change la pose (rotations locales) → la
mesure change. Une seule mesure ne suffit donc pas : on **itère 3 fois**
mesure → correction jusqu'à convergence, puis on contrôle le résidu.

Critère de contact : la cote z **du maillage**, pas de la pointe d'os (celle-ci
est ~34 mm au-dessus du maillage réel du pied).

## Axes de rotation (mesurés, pas supposés)

Le `roll` d'un os décide de l'orientation de ses axes locaux. Un diagnostic
(`scripts/axes_diag.py`) a mesuré pour chaque os le déplacement de sa pointe
selon chaque axe :

- colonne / cou / tete / queue : **X** = inclinaison haut-bas, **Z** = latéral
- pattes : **Z** = balancier avant-arrière (−Z vers l'avant), **X** = écart
- oreille : **X** = inclinaison

Sans cette mesure, on écrit une animation qui « part de travers ».

## Contraintes d'animation retenues

- **Trot diagonal** : pattes opposées en phase (av_G avec ar_D). Indispensable
  pour une démarche crédible — des décalages arbitraires donnent des pattes
  incohérentes.
- **Déplacement global sur l'objet armature**, pas sur l'os root : les axes
  d'un os dépendent de son roll, la translation d'objet est en coordonnées monde.
- **Caméra de profil qui suit** : une course se juge de profil.
- **Interpolation linéaire** entre images (pas de dépassement).
- Le saut **franchit** le ruisseau : décollage calé sur la berge gauche,
  atterrissage sur la berge droite.

## Vérification automatisée

`scripts/check_contact.py` contrôle image par image la cote z du maillage :

- course (1–55) : 0 image flottante ✓
- saut (72–88) : en l'air ✓
- toutes les autres : posées au sol ✓

Un rendu « joli » ne prouve rien : c'est cette mesure qui a permis de trouver
les 3 pièges ci-dessus.

## Note d'outillage

Ce build de Blender n'embarque pas le support **FFMPEG** (`FFMPEG` absent de
l'enum des formats d'image). Le rendu se fait donc en PNG puis la vidéo est
assemblée par `ffmpeg`.

## Suite

- Affiner le galbe au décollage et à la réception (poses encore proches).
- Ajouter une queue plus vivante sur la phase aérienne.
- Envisager un rendu plus fin (samples) pour une vidéo de présentation.

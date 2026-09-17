# 10.06 Panda roux — Animation (course, queue, saut)

Suite du projet [[10.06 Panda roux]] : après la modélisation (v7), on a monté
un **rig** puis écrit une **animation** : course, queue qui remue, et saut
par-dessus un petit ruisseau.

**v2 (courante)** — le timing du saut a été recalculé à partir de la physique.

## Livrables

- Rig : `~/projets/panda-roux/panda_roux_rig.blend` — 23 os
- Animation : `~/projets/panda-roux/panda_roux_saut.blend` — 82 images à 24 i/s
- Vidéo : `~/projets/panda-roux/renders/panda_ruisseau_v2.mp4` (3,4 s)
- Planches : `renders/planche_animation.png`, `planche_saut_v2.png`,
  `planche_franchissement.png`
- Scripts : `scripts/rig_panda.py`, `scripts/build_run.py`, `scripts/skin_lib.py`,
  `scripts/check_timing.py`

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

## Timing du saut : tout est déduit de la physique

Leçon principale de la v2. La v1 restait **24 images en l'air (1,00 s)**, ce qui
correspond à une chute de 4,90 m — pour un animal de 70 cm, c'est un saut
lunaire. Aucun réglage de pose ne rattrape ça : c'est le timing qu'il fallait
reprendre. Un saut raté l'est presque toujours par le timing, pas par les poses.

Tout part d'une **échelle déclarée** :

    echelle          1 unite = 0,83 m   (corps ~0,85 u ~ 0,70 m)
    gravite          g = 9,81 / 0,83 = 11,82 u/s^2
    duree de vol     10 images = 0,417 s
    sommet           H = g*T^2/8 = 0,256 u  (~21 cm reels)
    vitesse          1,05 u/s (0,87 m/s)
    saut             0,438 u (x -0,07 -> 0,37)

La durée de vol est **fixée**, la hauteur en est **déduite** (ou l'inverse via
`T = 2*sqrt(2h/g)`). Les deux ne sont jamais choisis indépendamment.

Cohérences imposées :

- Trajectoire = **parabole exacte** `4h·u(1−u)`, pas une cloche.
- **Vitesse horizontale constante** pendant le vol (un projectile n'accélère
  pas) et égale à la vitesse de course : le saut ne « pousse » pas.
- **Le décor se dimensionne par le saut** : largeur du ruisseau = `V × T`
  (0,35 u pour un saut de 0,44 u). Il est franchi par construction, pas par
  chance.
- **Poses de vol ancrées aux deux bouts** : `u=0` égale exactement la pose
  d'appui au décollage, `u=1` celle de l'atterrissage. Sans ça, la pose
  « saute » à la jonction sol/vol.
- **Squash & stretch continu** : écrasement au ramassage, étirement à l'appel,
  écrasement à l'impact, volume conservé (`sx = 1 + (1-sz)*0.6`).

Découpage retenu (24 i/s) : ramassage ~5 images, vol 10, absorption ~8.

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
mesure change. Une seule mesure ne suffit donc pas : on **itère 3-4 fois**
mesure → correction jusqu'à convergence, puis on contrôle le résidu.

Critère de contact : la cote z **du maillage**, pas de la pointe d'os (celle-ci
est ~34 mm au-dessus du maillage réel du pied).

## Les 2 pièges du script de CONTRÔLE

Le contrôle aussi peut se tromper. Deux erreurs commises, corrigées :

1. **Mesurer la parabole sur le point le plus bas du maillage.** Une
   trajectoire de projectile décrit le **centre de masse**. Pendant le vol la
   pose change (groupé puis extension), donc l'extrémité d'un membre monte et
   descend par rapport au centre : l'écart mesuré n'a aucun sens. Il faut
   mesurer `arm.location.z`. Corrigé, l'écart tombe à **0,00 mm**.
2. **Juger la continuité par un seuil en position.** Un écart de 100 mm entre
   deux images n'est pas un défaut : c'est la vitesse de décollage réelle
   (`g·T/2 = 2,46 u/s` → 103 mm/image). Un seuil en position condamne toute
   animation correcte. Juger la **vitesse** et son évolution (l'accélération).

## Vérification automatisée

`scripts/check_timing.py` (mesures, pas impressions) :

- **contact au sol** : 0 image flottante, 0 qui perce, sur 71 images hors vol
- **parabole du centre** : écart max **0,00 mm** vs trajectoire théorique
- **gravité mesurée** : −11,82 u/s² = gravité théorique exactement
- **durée de vol** : 0,417 s animée = 0,417 s théorique
- **franchissement** : vérifié image par image — jamais de contact avec l'eau,
  atterrissage sur la berge droite
- **phases** : anticipation, appel, groupé, extension, pattes tendues à
  l'arrivée, absorption — validées sur les images clés

Un rendu « joli » ne prouve rien : c'est cette mesure qui a permis de trouver
les pièges ci-dessus.

## Axes de rotation (mesurés, pas supposés)

Le `roll` d'un os décide de l'orientation de ses axes locaux. Un diagnostic
(`scripts/axes_diag.py`) a mesuré pour chaque os le déplacement de sa pointe
selon chaque axe :

- colonne / cou / tete / queue : **X** = inclinaison haut-bas, **Z** = latéral
- pattes : **Z** = balancier avant-arrière (−Z vers l'avant), **X** = écart
- oreille : **X** = inclinaison

Sans cette mesure, on écrit une animation qui « part de travers ».

## Autres contraintes retenues

- **Trot diagonal** : pattes opposées en phase (av_G avec ar_D). Indispensable
  pour une démarche crédible — des décalages arbitraires donnent des pattes
  incohérentes.
- **Déplacement global sur l'objet armature**, pas sur l'os root : les axes
  d'un os dépendent de son roll, la translation d'objet est en coordonnées monde.
- **Caméra de profil qui suit** : une course se juge de profil.

## Note d'outillage

- Ce build de Blender n'embarque pas le support **FFMPEG** (`FFMPEG` absent de
  l'enum des formats d'image). Le rendu se fait donc en PNG puis la vidéo est
  assemblée par `ffmpeg`.
- Blender 5 a refondu les **actions** (slots/layers) : `action.fcurves` n'existe
  plus, passer par `layer → strips → channelbags → fcurves`.
- Les **pose bones n'ont pas** `animation_data` : ils partagent l'action de
  l'armature.

## Suite

- Affiner le galbe au décollage et à la réception (poses encore proches).
- Queue plus vivante sur la phase aérienne.
- Rendu plus fin (samples) pour une vidéo de présentation.

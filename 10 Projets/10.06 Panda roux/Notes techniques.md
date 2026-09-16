# 10.06 Panda roux — Notes techniques

Détail des techniques utilisées pour générer le panda roux par script.
Ces notes servent à reproduire ou étendre le modèle.

## Vue d'ensemble de la chaîne

    profils 2D (polylignes + rayons)
      -> tubes balayés le long de splines Catmull-Rom
      -> fusion voxel (voxel_remesh)  == UNE seule surface
      -> lissage léger + subdivision
      -> relief de fourrure (displace + noise)
      -> coloration par sommets (FLOAT_COLOR)
      -> détails posés par-dessus (oreilles, yeux, nez, moustaches)
      -> rendu EEVEE, 5 vues

## Fichiers

- `scripts/roux_lib.py` — bibliothèque géométrique, sans dépendance Blender :
  - `catmull(ctrl, per_seg)` — spline passant par les points de contrôle
  - `tube_data(ctrl, radii)` — maillage d'un tube lisse le long de la spline
  - `frames(pts)` — repères de Frenet (référence adaptative, pas de
    dégénérescence sur les segments verticaux)
  - `arclen_param(p, pts)` — position en **longueur d'arc** (indispensable
    pour les anneaux d'une queue courbe)
  - `dist_to_polyline(p, pts)` — distance d'un point à une polyligne
- `scripts/build_v7.py` — génère la scène complète et les rendus.

## Repères du modèle

- `+X` = avant (le museau pointe vers +X), `+Z` = haut, sol à `z = 0`
- Longueur du corps ≈ 0,85 u, hauteur ≈ 0,62 u
- Chaque patte est une polyligne de 5 points : épaule/hanche → coude/genou →
  cheville/jarret → pied

## Pourquoi une coloration par sommets

`color_at(p)` renvoie la couleur d'un sommet à partir de sa **position** :

- roux par défaut
- **masque facial** : appartenance à une ellipsoïde englobant museau, joues
  et yeux, avec transition adoucie (`smoothstep`)
- **anneaux de queue** : paramètre en longueur d'arc × 7, bandes alternées
  avec transition douce aux bords
- **pattes** : proximité de l'axe du membre (et non un seuil en `z`, sinon
  tout le pied serait pris)
- **ventre** : faces tournées vers le bas, sous l'axe de la colonne

La priorité va à la queue, puis aux pattes, puis au ventre, puis au masque.

## Réglages qui comptent

| Réglage | Valeur | Pourquoi |
|---|---|---|
| `remesh_voxel_size` | 0,0088 | Plus fin = plus de détail, mais coût en faces |
| `SMOOTH iterations` | 3 | Au-delà, les pattes fines se rétractent |
| `SUBSURF levels` | 2 | Densité nécessaire pour le relief de fourrure |
| `DISPLACE strength` | 0,0058 | Relief de poil ; au-delà l'aspect devient granuleux |

## Pistes d'amélioration

- Passer par une **sculpture manuelle** pour l'organique fin (les yeux
  sphériques posés sur la tête se voient).
- Ajouter des **paupières**, une **bouche**, un modelé d'oreille plus réaliste.
- Remplacer le relief de déplacement par de vraies **particules de poils**
  si un rendu plus réaliste est visé.
- Après le rig : animer la queue et les oreilles (ce sont les éléments qui
  donnent le plus de vie à un personnage cartoon).

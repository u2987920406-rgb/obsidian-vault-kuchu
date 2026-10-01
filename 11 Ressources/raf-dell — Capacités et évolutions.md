# raf-dell — Capacités et évolutions

Compte-rendu du 2026-10-01 : ce que le Dell peut sortir, quels modèles locaux
tiennent dessus, et jusqu'où la machine peut évoluer.

## 1. Inventaire réel (relevé le 2026-10-01)

| Élément | Valeur mesurée |
|---|---|
| Modèle | Dell Precision Tower 5810 |
| Carte mère | Dell `0K240Y`, BIOS `A29` (2018-12-13) |
| CPU | Xeon E5-1620 v3 @ 3,50 GHz — **4 cœurs / 8 threads** (LGA2011-3, 1 socket) |
| RAM | **40 Go** installés (4×4 + 4×16 Go DDR4 ECC) — **4 slots libres sur 8** |
| GPU | GTX 1080 Ti **11 Go**, driver `580.178.04`, compute cap **6.1** (Pascal, **pas de tensor cores**) |
| CUDA / PyTorch | CUDA 12.9 · torch `2.7.1+cu126` (env conda `gpu` → `~/miniforge3/envs/gpu`) |
| Blender | **5.2.2 LTS** (Cycles GPU) |
| Stockage | sda 465 Go WD (OS) · sdb 238 Go Kingston SSD monté sur `/data` (**222 Go libres**) |
| Réseau | LAN `192.168.1.7` · Tailscale `100.91.24.46` |
| Alim | 425 W / 685 W / 825 W selon configuration d'usine (à confirmer au visuel) |

## 2. Puissance réelle

- **Rendu de référence** : décor japonais v3 (1600×1000, Cycles GPU, 123 605 verts,
  620 branches) rendu en **47 secondes**, scène complète en ~2 s (meshes fusionnés
  par `from_pydata`, une seule passe).
- **Rapport au BMAX** : le BMAX (Ryzen 7 8745HS, iGPU Radeon 780M, 14 Go) ne fait
  pas de Cycles GPU du tout. Le Dell est **la seule machine à faire du vrai rendu
  3D**. En pratique : BMAX = orchestration, code, bureautique ; Dell = calcul GPU.

## 3. Qualité de rendu : jusqu'où aller

Trois paliers sont accessibles, du plus sûr au plus ambitieux :

1. **Statique photoréaliste (immédiat)** — Cycles GPU, 1080p, éclairage HDRI,
   subsurface scattering, profondeur de champ. Temps : secondes à quelques minutes.
   C'est déjà ce qu'on fait, en beaucoup plus fin.
2. **Rendu par lots / variations (immédiat)** — un script pilote N variantes de
   caméra/lumière/couleur en une nuit. Idéal pour choisir un plan de décor.
3. **Animation courte (possible, lente)** — 1080p / 24 i/s en Cycles CPU+GPU.
   La 1080 Ti n'a pas de tensor cores et seulement 11 Go : compter **plusieurs
   heures par minute d'animation**. Pour de l'animation de jeu, EEVEE (temps réel)
   est le bon outil, Cycles réservé aux plans héros.
4. **Dénaturation / upscale** — Real-ESRGAN / waifu2x pour passer 2D en 4K, ou
   SDXL pour des textures. Tient largement dans 11 Go.

## 4. Modèles 100 % locaux que la machine tient

**Le facteur limitant est le Pascal : pas de tensor cores, donc pas de FP16/FP8
accéléré ni de flash-attention efficace.** Tout tourne, mais à vitesse modeste.

| Domaine | Modèles réalistes | Notes |
|---|---|---|
| **LLM texte** | 7B–8B en Q4_K_M (Qwen, Llama) via llama.cpp | ~8–9 tok/s mesuré par la communauté sur 1080 Ti. 14B : possible avec offload CPU, lent. |
| **LLM code** | 7B code en Q4 (Qwen-Coder) | Utilisable en complément, pas en remplacement d'un modèle cloud. |
| **Vision** | Qwen2-VL 7B, LLaVA 7B quantifiés | Rendent le Dell autonome pour juger une image. |
| **Image** | SD 1.5, **SDXL** (1024), SDXL-Turbo, Flux.1-schnell en Q4 GGUF | SDXL = le bon compromis. Flux Q4 est à la limite des 11 Go. |
| **Vidéo** | AnimateDiff (SD1.5), Stable Video Diffusion (14 frames 576×1024), LTX-Video quantifié | Vidéo = génération, pas du montage. Très lent, mais faisable. |
| **Voix** | Whisper (transcription), Piper/Kokoro (synthèse) | Tournent bien, même sur CPU. |
| **3D** | Blender (déjà là), Trellis / TripoSR pour du text-to-3D | Trellis tient dans 11 Go, sortie maillée exploitable. |

**Ce qu'il ne faut pas tenter** : entraîner un modèle (fine-tuning LoRA d'un 7B
est à la limite ; SDXL LoRA passe en revanche), les modèles vidéo récents type
gros transformers, et tout LLM > 14B dense.

## 5. Logiciels à ajouter pour la création de jeux

Rien n'est installé pour l'instant hormis Blender. Priorités :

- **ComfyUI** — le standard pour SDXL/Flux + upscale + animation. Interface
  web, pilotable depuis le BMAX.
- **Stable Diffusion WebUI Forge** — alternative plus simple que ComfyUI.
- **Krita + plugin AI Diffusion** — peinture de sprites et de décors avec l'IA
  dans le pinceau (le BMAX a déjà un skill Krita headless).
- **Aseprite** — sprites pixel art et animation frame par frame (le skill
  `pixel-art-sprites` existe déjà).
- **Godot 4** — 2D, 2D HD et 3D isométrique, léger, exporte en web et Android.
  C'est l'outil le plus adapté à la 2D HD et à l'iso.
- **Unity + URP** — 3D et iso 3D, plus lourd, pipeline de sprites 2D complet.
- **Blender + Grease Pencil** — décors 2D animés et plans de caméra.
- **Tiled** — éditeur de niveaux iso/2D, format standard lu par Godot et Unity.
- **Material Maker** — textures PBR procédurales pour la 3D.
- **Inkscape** — vectoriel, pour l'UI et les logos de jeu.

## 6. Évolutions matérielles possibles

Le T5810 est une station de travail : c'est la machine la plus évolutive du parc.

- **RAM** — 4 slots libres sur 8, max **256 Go** (8×32 Go DDR4 ECC RDIMM 2133).
  Passer à 72–128 Go coûte ~60–150 € d'occasion. Gain réel : modèles plus gros
  en RAM (offload), Blender avec de grosses scènes, plusieurs services simultanés.
- **GPU** — 2 slots PCIe x16 Gen3 + 1 slot x16 câblé x8. Le total graphique
  supporté va **jusqu'à 500 W avec l'alim 825 W**. Candidats : RTX 3060 12 Go
  (budget, +1 Go seulement), **RTX 4060 Ti 16 Go** (le bond raisonnable : tensor
  cores + 16 Go), **RTX 3090 24 Go d'occasion** (le vrai saut : 24 Go, tensor
  cores, mais 350 W et connecteurs d'alim Dell propriétaires à vérifier).
  La 1080 Ti peut rester en second GPU pour Blender (rendu multi-GPU).
- **CPU** — socket LGA2011-3 mono-processeur. Le T5810 accepte les Xeon E5-1600
  v3/v4 et certains E5-2600 v4 **jusqu'à 18 cœurs**. Passer d'un 4c/8t à un
  6c/12t (E5-1650 v3) ou 8c/16t coûte 30–80 € d'occasion et change le rendu CPU.
- **Carte mère** — non remplaçable par une carte standard (format Dell propriétaire).
- **Alimentation** — 425 W / 685 W / 825 W, **échangeable sans outil** sur le
  T5810. Si une RTX 3090 est envisagée, il faut passer en 825 W et vérifier les
  connecteurs PCIe propriétaires Dell.
- **Stockage** — 2 baies 3,5" internes + 4 emplacements 2,5". Un SSD NVMe n'est
  pas supporté (pas de slot M.2) ; le /data 238 Go est déjà là et presque vide.

**Ordre de priorité suggéré** : RAM (le moins cher, gain immédiat) → GPU
(le gain le plus visible pour tout le travail IA) → CPU (le rendu Blender) →
alim (seulement si GPU gourmand).

## 7. Où sont les scènes

- **Dell** : `/home/raf-linux/blender-out/` (.blend + PNG + logs) — sur `sda2`,
  406 Go libres. C'est là que le travail se fait, y compris le SSD `/data`.
- **BMAX** : copie de sauvegarde dans `~/projets/blender-decor/`.
- **Non versionné aujourd'hui.** Recommandation : garder les `.blend` dans
  `~/projets/blender-decor/` et les committer (un `.blend` de 3 Mo est acceptable,
  au-delà passer git-lfs ou archiver les seuls scripts + PNG).

## Voir aussi
- [[11 Ressources/raf-dell — Fiche opérationnelle]]
- [[11 Ressources/raf-bmax — Fiche opérationnelle]]

# 10.07 BenchIA

App maison de **benchmark hardware-aware de nos pipelines IA** (usage réel :
Hermès/apps, ComfyUI+Flux+ControlNet, vidéo, Unity/Unreal, computer use local et
cloud). Dépôt `~/projets/benchmark-ia` (git local), lien tailnet :
`https://raf-bmax.tail14baaa.ts.net:8460/` (grille de faisabilité).

**État courant : v1.0 livrée et validée par Raf (2026-09-26)** — les 6 jalons
du plan (J1-J6) et les 10 critères d'acceptation sont faits, preuves comprises.
Détail du plan et des preuves : `docs/Plan.md`, tableau de bord
`docs/plan.html`, rapports `docs/rapport.md` + `docs/verdicts.json`.

Ce que l'app fait :
- **Moteur de faisabilité réévaluable** : chaque paramètre de pipeline reçoit
  `ok | limite | refuse` + raison + seuil manquant, daté par profil matériel
  (`profils-materiel/`). Au changement de hardware, réévaluation automatique :
  les cases refusées sur le BMAX recochent (10 configs recouchables d'ores et
  déjà identifiées : Flux, Wan, LTX, Unreal, SDXL…). C'est le besoin central
  de Raf : « le jour où je change de hardware, je pourrai cocher les paramètres
  qui m'étaient refusés ».
- **Runner multi-envs** : oracles écrits à la main, score par EXÉCUTION
  (sandbox, timeout, sans secrets), quotas gérés (429/Retry-After, 404), N runs
  avec variance, RAM peak vs baseline, coût par tâche réussie.
- **Mesures réelles du BMAX** (14 Go RAM, iGPU 780M, pas de CUDA) : FastSD
  sd-turbo 512² = ~25 s/image, RAM peak ~2,7 Go ; Hermès+mimo ~16 s ;
  Hermès+ollama qwen3 ~21 s ; NIM nemotron ~1,4 s. Toutes les vidéos/Flux/Unreal
  = refusés motivés avec seuils.
- **Bench computer use** : tâches courtes/longues, oracle binaire sur état
  serveur, cap 50 actions = mur, taux + latence/action (agent réel Hermès
  validé sur l'atelier : ~101 s/action).

**Taxe de harness chiffrée (2026-09-28)** — même modèle `deepseek-v4.1-flash`
via ollama-cloud, cas `distance-velo`, 5 runs, 100 % de réussite partout :

| cellule | harness | médiane | RAM peak | taxe vs API nue |
|---|---|---|---|---|
| C0 | aucun (API directe) | 3,9 s | — | référence |
| C5 | DSH headless | 4,3 s | 0,54 Go | +0,4 s (1,1×) |
| C6 | Hermès CLI | 10,5 s | 0,24 Go | +6,6 s (2,7×) |

DSH : débloqué par `--patch` (le bundle force `deepseek-official/deepseek-flash`,
route sans clé, et la pile headless n'a aucun fournisseur lisible) —
`envs/dsh-default-model.patch.yml`. Hermès paie son bootstrap agent (mémoire,
skills, garde-fous) mais reste le plus léger en RAM. Fichiers : `envs/c0-*`,
`envs/c5-dsh-deepseek41.yaml`, `envs/c6-hermes-deepseek41.yaml`.

AJEAN (C6 du catalogue) = **non_disponible** documenté : v0.16.4 n'expose pas
`/v1/chat/completions` (404) et son CLI exige un moteur local sur :8080 ; le
smoke externe (`ajean test` → « pong ») est validé mais non mesurable par le
runner. Voir `catalogue/llm.yaml`.

Anomalie J4 close : build Unity headless = 12,3 s sur AstroprismaUnity
(éditeur portable : `LD_LIBRARY_PATH=prefix-libxml/lib`, projet dans le
sous-dossier `AstroprismaUnity/`).

**Prochaine étape : rejouer les mesures à chaque changement de profil matériel**
(le moteur de recoche est prêt) ; aucune clé payante requise.

**2e nœud matériel — Dell Tower 810 (profil `profils-materiel/dell-810.yaml`,
28/09)** : GTX 1080 Ti (11 Go VRAM, CUDA certain, driver 580.x max), RAM et
disque encore **non sondés** → type `reel_partiel`, champs `a_confirmer`.
Verdicts : **23 ok / 4 limite / 6 refusé** contre 17/5/11 sur le BMAX, **6
configurations recouchées et 0 régression** (Flux nf4 1024 et Wan 21-1B 480p
passent ok). Mur restant : VRAM 11 Go (Wan 14B, Hunyuan, nanite) et 32 Go RAM
pour Unreal. La machine n'est ni dans le tailnet ni sur le LAN — à re-sonder
avant de considérer les cases « RAM/disque » comme acquises.

**App mobile + grille** : APK Capacitor (`mobile/BenchIA.apk`) pointant sur
`https://raf-bmax.tail14baaa.ts.net:8462` ; grille desktop sur `:8460`. Le CLI
`bench` (verdict / run / rapport) et `scripts/gen_mobile_data.py` régénèrent les
données affichées — zéro valeur estimée.

Liens : [[00 Index]] · méthode : skill la-methode.

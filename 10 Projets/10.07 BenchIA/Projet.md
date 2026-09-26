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

Prochaine étape : clé `DEEPSEEK_API_KEY` pour ouvrir la cellule C4 (DSH),
rejouer les mesures à chaque changement de profil matériel.

Liens : [[00 Index]] · méthode : skill la-methode.

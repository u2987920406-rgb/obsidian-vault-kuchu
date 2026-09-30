# 00 — INDEX (MOC) du Vault

Finding aid unique. L'IA lit ce fichier AVANT tout sujet Hermès / Ulysse / Freebuff,
puis suit les liens vers les notes détaillées. Garder ce fichier stable et lisible
(pas de timestamps ici — voir 30 Journal).

## 10 Projets (projets de kuchu, pas des IA)
- [[10.01 Ulysse]] — ⛔ **STAND-BY — production arrêtée, aucune reprise planifiée.**
  Masque web UI visuel posé sur Hermès (installateur .bat from-scratch pour
  utilisateurs non-tech). Arrêté parce que ses qualités venaient du prompt système
  de Hermès, pas du harness : trop cher en temps et en forfait token pour un
  comportement déjà obtenu par configuration. Service, scripts, salon Discord et
  dossiers de code supprimés ; dépôt GitHub archivé, contexte dans l'issue #129.
  Avant toute reprise : *qu'apporte-t-il que Hermès ne donne pas déjà ?*
  Archi (historique) : [[10.01 Ulysse/ARCHI]].
  **État courant : [[10.01 Ulysse/ETAT-2026-08-09]]** — jalon 4 atteint avant
  l'arrêt.
- [[10.02 Hermes Dashboard]] — Dashboard de pilotage Hermes en Rust (axum,
  `~/projets/hermes-dashboard`, https://github.com/u2987920406-rgb/hermes-dashboard).
  v0.1 : données mockées, 6 endpoints API, port 8091. Généré par Freebuff.
  Création : [[10.02 Hermes Dashboard/Création]].
- [[10.03 Astroprisma]] — App web compagnon (Vite+React+TS, PWA) pour le JdR
  solo ASTROPRISMA. Repo `Astroprisma_app_EMERGENT`, branche
  `claude/app-status-fmj9ow`. Harnais de jeu automatisé (`npm run test:harness`),
  0 bug après fixes. Résumé : [[10.03 Astroprisma/Projet]].
- [[10.04 Kitsune to Yōkai]] — Run-and-gun arcade Android, feeling Pocky and
  Rocky. **v1 DÉCLARÉE** (2026-09-05) : MVP jouable en navigateur, boucle
  complète menu→boss→victoire. Docs : [[10.04 Kitsune to Yōkai/Brainstorming]],
  [[10.04 Kitsune to Yōkai/PRD]], [[10.04 Kitsune to Yōkai/Guide-lines]],
  [[10.04 Kitsune to Yōkai/Plan]], [[10.04 Kitsune to Yōkai/Bilan-v1]].
  Backlog v2 : harmonisation sprite/décor, sprites top-down, contenu.
- [[10.05 Gestion Budget]] — App PWA mobile-first de gestion de budget
  autoentrepreneur. Objectif : en un coup d'œil, savoir si l'activité couvre les
  charges et combien se verser. PRD (étape 2) : [[10.05 Gestion Budget/PRD]].
  Prochaine étape : Guide-lines.
- [[10.06 Panda roux]] — Animation de personnage dans Blender (BMAX).
  **Modèle v7 + rig + animation v2 validés** (2026-09-15) : panda roux cartoon,
  course, queue et saut de ruisseau à timing physique (parabole à 0,00 mm de la
  théorie). Modélisation et animation par script bpy
  (`~/projets/panda-roux`). Détail : [[10.06 Panda roux/Animation]],
  [[10.06 Panda roux/Notes techniques]].
  Prochaine étape : autres animations dans un salon dédié.
- [[10.07 BenchIA]] — App de benchmark hardware-aware de nos pipelines IA
  (`~/projets/benchmark-ia`). **v1.0 validée** (2026-09-26) : verdicts de
  faisabilité réévaluables au changement de matériel (cases refusées →
  recochables), runner multi-envs avec oracles exécutés, mesures réelles du
  BMAX, bench computer use. Détail : [[10.07 BenchIA/Projet]].

## 11 Ressources
- [[11 Ressources/Écosystème Raf — Vue d'ensemble]] — carte mentale des outils
  & processus (La Méthode, Cabinet, QA loop, Boucle rétro, HTML-as-Plan, sandbox,
  Rust). À lire avant de se demander « où en est notre écosystème ? ».
- [[11 Ressources/Cabinet agentique — Rôles]] — les 5 rôles du cabinet (Conceiveur,
  Codeur, Vision, Recherche, Boucle rétro) + mapping cerveau GLM 5.3. Figé 08/09.
- [[11 Ressources/freeB]] — Pont MCP Freebuff ↔ Hermès (33 outils, 11 exposés par
  défaut + `ask` porte universelle, validé e2e).
- [[11 Ressources/Gestion email — Process]] — Process & technique de tri/purge
  d'une boîte Gmail via Himalaya CLI (meta, pas de données de mail).
- [[11 Ressources/raf-bmax — Fiche opérationnelle]] — Le serveur homelab :
  accès (Discord d'abord, SSH en secours), stack Docker, surveillance,
  Hermès, procédures headless. À lire quand quelque chose ne va pas.
- [[11 Ressources/BanditRS — Scanner sécurité Python]] — Scanner de sécurité
  Python (Rust, drop-in `bandit`). Installé sur BMAX dans
  `~/.hermes/.venv-banditrs/`. Audits vault/Astroprisma/Ulysse à 0.
  Config Ulysse (avant stand-by) : `~/projets/ulysse/.bandit.yaml` — **chemin
  disparu, projet en stand-by**.

## 20 Outils (namespace par outil — locataires, JAMAIS à la racine)
- [[20.02 DeepSeek Harness/README|20.02 DeepSeek Harness]] — Locataire `dsh` (DeepSeek Harness, MIT, dev preview) :
  runtime d'agent « tout est plugin », Web UI sur 3080, service systemd user,
  exposé tailnet sur 8447. Modèle branché sur la route Ollama Cloud.
- [[20.01 Hermès/README|20.01 Hermès]] — Locataire Hermès : miroir curé de USER/MEMORY/SOUL, ADM, RECAP,
  [[20.01 Hermès/SKILLS|index des skills]]. Architecture mémoire (couches, décharge,
  décision RAG, config compression) : [[20.01 Hermès/ARCHITECTURE-MEMOIRE]].
  Coût des tokens et comportement d'usage (mesures, gestes, alertes) :
  [[20.01 Hermès/COUTS]].
  (Ajouter un outil = ajouter un 20.0x, jamais recréer un Vault.)

## 30 Journal
- [[30 Journal/2026-09-16]] — Gel du BMAX 7 h sans protection (watchdog étage 1 jamais chargé : sp5100_tco blacklisté par le noyau Ubuntu → faux-vert) + panne Ollama Cloud 17:12→17:38, 6 salons bloqués.
- [[30 Journal/2026-09-15]] — Gel noyau du BMAX (~5 h injoignable) : diagnostic, cause (watchdog désarmé + Kuma sur la machine surveillée), scripts de reprise à distance + section 17 de la fiche.
- [[30 Journal/2026-09-08]] — HTML-as-Plan intégré à La Méthode, validé sur Ulysse (template skill + étape 6bis, rendu jsdom vérifié).
- [[30 Journal/2026-09-05]] — Kitsune to Yōkai : v1 déclarée (passe globale, débogage visuel, process capture vision).
- [[30 Journal/2026-09-04]] — Kitsune to Yōkai : MVP complet codé + APK buildé (session autonome).
- [[30 Journal/2026-09-02]] — Astroprisma : harnais de jeu automatisé, 2 bugs
  réparés, 0 bug au re-test.
- [[30 Journal/2026-08-07]] — Journal de séance (entrée / sortie de travail).
- [[30 Journal/2026-08-10]] — le rail : quatre défauts, dont un que le design
  ne pouvait pas voir
- [[30 Journal/2026-08-09]] — Terminal branché, deux passes de design, socle
  des garde-fous d'écriture. Cinq défauts et ce qu'ils apprennent.

- Document d'origine (2026-08-07, plan d'indexage **exécuté**) :
  [[HANDOFF-Plan-Indexage-Vault]].

## Règle d'or
1 Vault unique. Racine = index partagé. Outils = locataires 20.x.
Ajouter un outil IA = ajouter un dossier 20.0x, jamais recréer un Vault séparé.

## Source de vérité
- MEMORY.md (injectée chaque tour) reste LEAN : règles + profil + UN pointeur ici.
- Toute la connaissance projet (archi Ulysse, Freebuff) vit DANS ce Vault, pas dans MEMORY.md.
- ⛔ Ulysse : **stand-by** (production arrêtée 2026-09-30, issue #129 du dépôt
  `LyssU_googlelike`). Récap historique : [[10.01 Ulysse/ARCHI]].

## Automatisation Git (backup Vault)
- Plugin « Obsidian Git » (Vinzent) installé. Repo privé GitHub : obsidian-vault-kuchu (branche master).
- Pull automatique au démarrage d'Obsidian (récupère l'état distant).
- Push manuel via raccourci **Ctrl+Alt+S** (Commit-and-sync : commit + pull + push groupé).
- Aucun push automatique à intervalle : on pousse quand on veut, en une fois.
- Identité commit : kuchubb / u2987920406@gmail.com. Token PAT géré par le Gestionnaire de credentials Windows.
- Règle : avant de quitter, Ctrl+Alt+S pour figer le travail sur GitHub (survit au PC).
- [[30 Journal/2026-09-22]] — Hermès basculé sur Xiaomi (Token Plan) — modèle, fallback, vision, TTS/STT.
- [[30 Journal/2026-09-21]] — Astroprisma — tour canonique p.30-32 + vue duel (maquette tactique).
- [[30 Journal/2026-09-17]] — Astroprisma — chantier « tous les jets du livre » : 78 jets branchés (9 domaines).
- [[30 Journal/2026-09-14]] — Astroprisma — parcours start→endpoints, dossier + matrice de couverture.
- [[30 Journal/2026-09-09]] — Astroprisma — retours map de Raf, Jalon 2 en boucle nocturne.
- [[30 Journal/2026-09-07]] — Rétro de la boucle quotidienne (Kitsune v1) — leçons et actions.
- [[30 Journal/2026-08-31]] — Ulysse lancé sur le BMAX (handoff Claude) — état vérifié en réel.

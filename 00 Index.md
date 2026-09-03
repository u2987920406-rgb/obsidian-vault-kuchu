# 00 — INDEX (MOC) du Vault

Finding aid unique. L'IA lit ce fichier AVANT tout sujet Hermès / Ulysse / Freebuff,
puis suit les liens vers les notes détaillées. Garder ce fichier stable et lisible
(pas de timestamps ici — voir 30 Journal).

## 10 Projets (projets de kuchu, pas des IA)
- [[10.01 Ulysse]] — Masque web UI visuel posé sur Hermès. Installateur .bat
  from-scratch pour utilisateurs non-tech (fournissent leur clé Nous).
  Archi détaillée : [[10.01 Ulysse/ARCHI]].
  **État courant : [[10.01 Ulysse/ETAT-2026-08-09]]** — jalon 4, dépôt déplacé
  sur le Bureau, Terminal branché (`hermes --tui` derrière `/api/pty`), socle
  des garde-fous d'écriture posé.
- [[10.02 Hermes Dashboard/INDEX]] — Dashboard de pilotage Hermes en Rust (axum,
  `~/projets/hermes-dashboard`, https://github.com/u2987920406-rgb/hermes-dashboard).
  v0.1 : données mockées, 6 endpoints API, port 8091. Généré par Freebuff.
  Création : [[10.02 Hermes Dashboard/Création]].
- [[10.03 Astroprisma]] — App web compagnon (Vite+React+TS, PWA) pour le JdR
  solo ASTROPRISMA. Repo `Astroprisma_app_EMERGENT`, branche
  `claude/app-status-fmj9ow`. Harnais de jeu automatisé (`npm run test:harness`),
  0 bug après fixes. Résumé : [[10.03 Astroprisma/Projet]].

## 11 Ressources
- [[11 Ressources/freeB]] — Pont MCP Freebuff ↔ Hermès (33 outils, 11 exposés par
  défaut + `ask` porte universelle, validé e2e).
- [[11 Ressources/Gestion email — Process]] — Process & technique de tri/purge
  d'une boîte Gmail via Himalaya CLI (meta, pas de données de mail).
- [[11 Ressources/raf-bmax — Fiche opérationnelle]] — Le serveur homelab :
  accès (Discord d'abord, SSH en secours), stack Docker, surveillance,
  Hermès, procédures headless. À lire quand quelque chose ne va pas.

## 20 Outils (namespace par outil — locataires, JAMAIS à la racine)
- [[20.01 Hermès]] — Locataire Hermès : miroir curé de USER/MEMORY/SOUL, ADM, RECAP,
  [[20.01 Hermès/SKILLS|index des skills]].
  (Ajouter un outil = ajouter un 20.0x, jamais recréer un Vault.)

## 30 Journal
- [[30 Journal/2026-09-02]] — Astroprisma : harnais de jeu automatisé, 2 bugs
  réparés, 0 bug au re-test.
- [[30 Journal/2026-08-07]] — Journal de séance (entrée / sortie de travail).
- [[30 Journal/2026-08-10]] — le rail : quatre défauts, dont un que le design
  ne pouvait pas voir
- [[30 Journal/2026-08-09]] — Terminal branché, deux passes de design, socle
  des garde-fous d'écriture. Cinq défauts et ce qu'ils apprennent.

## Règle d'or
1 Vault unique. Racine = index partagé. Outils = locataires 20.x.
Ajouter un outil IA = ajouter un dossier 20.0x, jamais recréer un Vault séparé.

## Source de vérité
- MEMORY.md (injectée chaque tour) reste LEAN : règles + profil + UN pointeur ici.
- Toute la connaissance projet (archi Ulysse, Freebuff) vit DANS ce Vault, pas dans MEMORY.md.
- Récap complet Ulysse : voir [[10.01 Ulysse/ARCHI]] (version indexée, source de vérité).

## Automatisation Git (backup Vault)
- Plugin « Obsidian Git » (Vinzent) installé. Repo privé GitHub : obsidian-vault-kuchu (branche master).
- Pull automatique au démarrage d'Obsidian (récupère l'état distant).
- Push manuel via raccourci **Ctrl+Alt+S** (Commit-and-sync : commit + pull + push groupé).
- Aucun push automatique à intervalle : on pousse quand on veut, en une fois.
- Identité commit : kuchubb / u2987920406@gmail.com. Token PAT géré par le Gestionnaire de credentials Windows.
- Règle : avant de quitter, Ctrl+Alt+S pour figer le travail sur GitHub (survit au PC).
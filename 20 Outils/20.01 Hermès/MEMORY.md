# Hermès — MEMORY (environnement & projets)

> Index/pointeur — source de vérité pour l'environnement et les projets de Raf.
> Le détail vit dans les notes liées (pas de doublon ici). Mis à jour le 2026-09-02.
> Le natif (`~/.hermes/memories/MEMORY.md`) contient les 10 commandements + ce pointeur.

## Machine & infrastructure
- **Fiche opérationnelle BMAX** : [[11 Ressources/raf-bmax — Fiche opérationnelle]]
  (accès, stack Docker, surveillance, Hermès, procédures headless). À lire quand
  quelque chose ne va pas.

## Projets
- [[10.01 Ulysse]] — Masque web UI sur Hermès. Repo `LyssU_googlelike`, port 8090,
  tailnet. Salon #ulysse.
- [[10.02 Hermes Dashboard]] — Dashboard de pilotage Hermès en Rust (axum), port 8091,
  données mockées v0.1. Salon #hermes-dashboard.
- [[10.03 Astroprisma]] — App web compagnon (Vite+React+TS, PWA) pour le JdR solo.
  Repo `Astroprisma_app_EMERGENT`. Salon #astroprimsa.
- MangoOS (MangoAI) — App Tauri, backend Rust, front TS. `~/projets/mangoai`.
  Tout sur Ollama Cloud. Salon #mango-os.
- Dockerwatch — App Python surveille les conteneurs Docker, webhook Discord.
  `~/projets/dockerwatch`.
- [[11 Ressources/freeB]] — Pont MCP Freebuff ↔ Hermès (33 outils, 11 exposés).
  `~/projets/freeB`. Salon #freebuff-test.

## Outils & services
- Outils installés : ChatGPT (Linux), google-drive-ocamlfuse (Drive FUSE),
  claude-code, gh CLI (auth u2987920406-rgb), Rust/cargo 1.98.
- Google Drive monté sur `~/gdrive` (NON persistant au reboot — remonter manuellement).
- Serveur Discord « Hermes server » : #général (discussion seule), #alertes,
  #livrable-projet/#rapports, salons Projets. Rôle Hermes a MANAGE_MESSAGES.
- Crons actifs : vault-backup (23h), boucle-ulysse (15min), rapport-vault-quotidien (2h).
- Modèles & coûts : Ollama Pro 18€/mois (abonnement). deepseek-v4-flash / glm-5.3-flash.

## Commandements
- Les 10 commandements (règles intouchables) : [[20.01 Hermès/COMMANDEMENTS]]

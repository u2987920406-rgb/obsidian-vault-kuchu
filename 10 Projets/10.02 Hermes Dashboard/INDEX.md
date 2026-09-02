# 10.02 Hermes Dashboard

Dashboard web de pilotage Hermes, en **Rust**.

## État courant
**Jalon : v0.1 fonctionnel (2026-09-02)** — dashboard généré par **Freebuff** via le pont MCP, compilé et testé en réel, accessible sur le tailnet.

- Historique complet de la création : **[[10.02 Hermes Dashboard/Création]]**

## Dépôt
`~/projets/hermes-dashboard` — remote : https://github.com/u2987920406-rgb/hermes-dashboard (`origin/master`)

## Stack
- Rust 2021, **axum 0.7** + tokio + serde/serde_json
- `src/main.rs` autonome (525 lignes), port **8091** configurable via `PORT`

## Fonctionnalités (données mockées en mémoire pour l'instant)
- `/` → page dashboard sombre (cartes HTML/CSS/JS embarquées)
- `/api/status` → état général, modèle actif, tokens du jour
- `/api/tokens` → tokens/coût par modèle
- `/api/crons` → liste des tâches planifiées
- `/api/loops` → boucles actives
- `/api/kanban` → colonnes/cartes
- `/api/veilles` → veilles

## Compiler / lancer
```bash
cd ~/projets/hermes-dashboard
cargo build                                  # build
./target/debug/hermes-dashboard              # lancer sur :8091
PORT=9000 ./target/debug/hermes-dashboard    # autre port
```

## Prochaines étapes (non faites)
- [ ] Connecter aux vraies données Hermes (tokens par modèle, crons, boucles) au lieu des mocks
- [ ] Pusher sur un remote git
- [ ] Choix du modèle actif depuis le dashboard (fonctionnel)

---
Voir aussi : [[11 Ressources/freeB]] (pont MCP Freebuff ↔ Hermes)

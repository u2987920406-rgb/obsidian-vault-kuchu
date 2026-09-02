# 10.02 Hermes Dashboard — Création du projet

Création du dashboard Hermes, **2026-09-02**. Généré par **Freebuff** via le pont MCP, depuis le salon Discord #freebuff-test.

## Contexte & déclencheur
Raf souhaitait un dashboard RUST de pilotage Hermès (tokens par modèle, crons, loops, kanban, veilles) — complément du dashboard natif (127.0.0.1:9119, local, basique). Le point d'entrée était un test du pont Freebuff (« teston freebuff »).

## Déroulé
1. **Test du pont Freebuff** (freebuff_ask / freebuff_search / freebuff_stats) — fonctionnement validé.
2. **Premier appel freebuff_code « dashboard Hermes » en Rust** → réponse MCP **polluée** (500 Ko de fichiers internes : cron/, kanban.db, run-state.json) au lieu du code.
3. **Diagnostic** : le serveur MCP tourne avec `cwd = ~/.hermes` → `FREEBUFF_PROJECT_DIR` pointait sur le home d'Hermes, et `_collect_generated_files` ramassait tout comme « fichiers générés ».
4. **Fix du pont** (`freebuff_driver.py`) : filtres `INTERNAL_ARTIFACTS` / `INTERNAL_ARTIFACT_FILES` / `CODE_EXTENSIONS`, filtrage sur parties relatives. 144 tests serveur verts.
5. **Relance** : timeout MCP 300 s — mais Freebuff a **continué en arrière-plan** et a bien écrit le code sur disque.
6. **Récupération & placement** : code trouvé dans `~/.hermes/hermes-dashboard/` (cwd du serveur), déplacé vers `~/projets/hermes-dashboard/`.
7. **Vérification réelle** : installation Rust/cargo 1.98, `cargo build` **zéro erreur**, lancement sur `:8091`, les 6 endpoints API + page HTML testés via curl.
8. **Test depuis le téléphone** : accessible via Tailscale (100.101.17.46:8091). Raf : « Ça marche sur Tailscale. Je trouve ça très correct. »
9. **Versionnement** : repo GitHub privé créé (voir plus bas), push sur `master`.

## Livrables / dépôt
- Dossier local : `~/projets/hermes-dashboard/`
- Git remote : https://github.com/u2987920406-rgb/hermes-dashboard (`origin/master`)
- Fichiers : `Cargo.toml`, `Cargo.lock`, `src/main.rs` (525 lignes, axum), `.gitignore` (exclut `target/`)

## Stack & fonctionnalités (v0.1, données mockées)
- Rust 2021, **axum 0.7** + tokio + serde/serde_json ; port **8091** configurable via `PORT`.
- `/` page sombre HTML/CSS/JS embarquée ; 6 endpoints API : `/api/status`, `/api/tokens`, `/api/crons`, `/api/loops`, `/api/kanban`, `/api/veilles`.

## Décisions
- **Rust** : conforme à l'orientation de Raf (« passer au Rust pour les nouveaux projets »).
- **Un seul fichier main.rs autonome** d'abord, cohérent avec le minimalisme de Raf (pas de sur-engineering).
- **Port 8091** (différent du 8090 d'Ulysse, du 5173/3000 de MangoOS) pour éviter tout conflit.

## Reste à faire (voir INDEX)
- Connecter les vraies données Hermes (tokens/crons/boucles réels) au lieu des mocks — le cœur du dashboard.
- Service systemd pour persister au reboot.
- (Réglé) timeout MCP 300 s trop court pour les grosses générations Freebuff.

---
→ [[10.02 Hermes Dashboard/INDEX|Retour à l'index du projet]]

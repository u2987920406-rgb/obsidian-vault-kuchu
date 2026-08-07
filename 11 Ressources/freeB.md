# freeB (pont MCP Freebuff ↔ Hermès)

RÔLE : ressource réutilisable — un pont MCP prêt à l'emploi entre Hermès Agent et
Freebuff (IA de code cloud). Sert à faire exécuter du code par Freebuff depuis
Hermès, en séparant l'orchestration (modèle faible) de l'exécution (Freebuff).

## Ce que c'est
Dépôt GitHub : https://github.com/u2987920406-rgb/freeB
Deux pièces :
- `freebuff-mcp-server/` : serveur MCP qui encapsule la CLI Freebuff pour qu'Hermès
  l'appelle comme 33 outils typés (freebuff_code, _ask, _vault, _plan…).
- `hermes-bridge/` : pont inverse — expose Hermès comme outil MCP pour qu'un
  « cerveau » (Claude Code, un autre modèle) pilote Hermès → Freebuff.

## Pourquoi ça existe
Mesure du 31/07 : un modèle FAIBLE qui doit deviner l'outil + args en langage
naturel casse 17/10 ; avec des outils typés explicites, 0 erreur. Le pont retire
la décision au petit modèle et laisse l'exécution à Freebuff.

## Installé et validé sur cette machine (2026-08-08)
- Clone : `C:\Users\kuchu\Desktop\freeB`
- Serveur `freebuff` enregistré dans `~/.hermes/config.yaml` (`mcp_servers.freebuff`,
  `enabled: true`) → **33 outils MCP vivants dans Hermès**.
- Tests : `freebuff-mcp-server` 152 verts (143 + 9 canari) ; `hermes-bridge` 55 verts
  (52 unitaires + 3 e2e RÉELS délégués à Claude Code Opus 5).
- Le e2e bridge prouve la chaîne complète : client MCP → server.py → binaire Hermès →
  Freebuff, verdict `[verifie : 1/1]` issu de la relecture du vrai log Hermès.

## Points d'attention (honnêteté)
- Le driver pilote Freebuff en reverse-engineering (tmux + lecture du store disque
  `~/.config/manicode/.../chat-messages.json` + `log.jsonl`). Format non publié →
  un canari (`test_canary_freebuff_store.py`) surveille qu'il ne casse pas.
- `FREEBUFF_TAKEOVER=1` tue le process hébergeant le chat de l'utilisateur (verrou
  machine-wide) — à ne pas activer à la légère.
- 24 des 33 outils sont des gabarits qui ne font qu'envelopper un appel à Freebuff en
  lui demandant ce qu'il sait déjà faire. Exposition réduite à **11 outils par défaut**
  (ask, code, plan + 8 vault) via `FREEBUFF_EXPOSE_TOOLS`. Les autres restent
  accessibles via `freebuff_ask`.

## Comment réutiliser
- Relancer le serveur MCP Freebuff : `cd C:\Users\kuchu\Desktop\freeB\freebuff-mcp-server`
  puis `.venv\Scripts\python -u server.py` (Hermès le charge via config.yaml).
- Tester le bridge e2e : `cd C:\Users\kuchu\Desktop\freeB\hermes-bridge` puis
  `.venv\Scripts\python -m pytest test_e2e_bridge.py -v`.
- Commits clés faits cette session (non pushés) : c82d741 (fiabilité/canari),
  6eab7bd (exposition outils), ed6836a (test e2e), b769440 (vault freeB).

## Liens
- Vault détaillé du projet : `C:\Users\kuchu\Desktop\freeB\vault\` (07-session.md pour
  l'état vivant).

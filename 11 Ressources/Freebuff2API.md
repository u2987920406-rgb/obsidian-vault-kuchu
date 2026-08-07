# Freebuff2API

RÔLE : repli provider personnel (usage perso uniquement, pas produit) pour utiliser
les modèles gratuits de Freebuff via un proxy OpenAI-compatible local.
Bascule MANUELLE à l'écrit (« mode FreeBuff »), jamais automatique.

## Ce que c'est
Proxy Go (MIT) — https://github.com/Quorinex/Freebuff2API
Traduit l'API OpenAI standard en requêtes vers le backend Freebuff (codebuff.com).
Résultat : n'importe quel client compatible OpenAI (dont Hermès) peut utiliser les
modèles gratuits Freebuff en pointant sur http://localhost:8080/v1.

## Capacités notables (README)
- OpenAI-compatible (endpoints standards /v1/*).
- Stealth fingerprints (imite le SDK officiel Freebuff).
- Multi-token rotation (ROTATION_INTERVAL, défaut 6h).
- HTTP proxy en amont optionnel.

## Auth (token Freebuff)
Deux méthodes (README) :
1. Web (recommandée) : https://freebuff.llm.pm → login → token affiché directement.
   (ou flux OAuth login?auth_cc : ouvrir l'URL de connexion, se connecter, copier
    l'URL de RAPPEL vers laquelle Freebuff redirige, la coller, récupérer le jeton)
2. CLI : `npm i -g freebuff` → login → token dans
   C:\Users\<user>\.config\manicode\credentials.json (clé authToken).

## Configuration du proxy
JSON et/ou variables d'env (noms identiques). Config par défaut dans le répertoire
de travail (config.json), ou `-config <chemin>`.
Clés : LISTEN_ADDR (:8080), UPSTREAM_BASE_URL (https://codebuff.com),
AUTH_TOKENS (array), ROTATION_INTERVAL (6h), REQUEST_TIMEOUT (15m),
API_KEYS ([] = accès ouvert), HTTP_PROXY ("").

## Déploiement
- Docker (images GHCR multi-arch) :
  docker run -d --name Freebuff2API -p 8080:8080 -e AUTH_TOKENS="token" ghcr.io/quorinex/freebuff2api:latest
- Build from source : Go 1.23+ → go build -o Freebuff2API . → ./Freebuff2API -config config.json

## Intégration Hermès (projet Ulysse / usage perso)
Implémentée via la SKILL `freebuff-mode` (dans HERMES_HOME, secret) :
- « mode FreeBuff » → démarre le proxy en arrière-plan + bascule provider=custom,
  base_url=http://localhost:8080/v1. Vérifie réellement /v1/models avant de confirmer.
- « quitte FreeBuff » → tue le proxy + restaure provider=nous.
- Token rangé SEUL dans .env Hermès (clé FREEDBUFF_AUTH_TOKEN), jamais en clair ailleurs.
- Binaire + config vivent dans C:\Users\kuchu\AppData\Local\hermes\freebuff\ (secret).

## Réserves (honnêteté)
Service tiers NON officiel, instable, flou ToS. Peut casser / être rate-limité à tout
moment. Repli perso uniquement — pas un provider permanent pour utilisateurs finaux.

## État (2026-08-07)
- Skill `freebuff-mode` créée (SKILL.md + start/stop PowerShell + config.json template).
- Token ajouté dans .env Hermès.
- Build non encore fait : Go absent de la machine → 1re activation via Docker OU
  install Go. À tester quand l'utilisateur le demande.

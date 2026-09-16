# 20.02 DeepSeek Harness — Base de `dsh`

> **Ce que c'est.** `dsh` (DeepSeek Harness) est le *runtime d'agent* open source
> (MIT) publié par DeepSeek AI — architecturé « tout est plugin » (noyau Cordis).
> C'est un **outil à part**, distinct d'Hermès : il ne remplace rien, il cohabite.
> Installé sur le BMAX le 2026-09-15, en **developer preview** (`0.1.x`).

## Où c'est

| Quoi | Chemin / valeur |
|---|---|
| Dépôt source (clone `master`) | `~/projets/deepseek-harness` (~1,9 G avec `node_modules`) |
| Racine de données `DSH_HOME` | `~/projets/deepseek-harness/.dsh-home` |
| Réglages (relus à chaud) | `~/projets/deepseek-harness/.dsh-home/settings.yaml` |
| Clé API (600, hors dépôt) | `~/.config/dsh/dsh.env` → `OLLAMA_API_KEY=...` |
| Service | `~/.config/systemd/user/dsh.service` — **systemd utilisateur**, `enabled` |
| Port | `127.0.0.1:3080` (jamais 0.0.0.0 : refusé par dsh) |
| Exposition tailnet | `https://raf-bmax.tail14baaa.ts.net:8447/` (tailscale serve) |

## S'en servir

Récupérer le lien **avec le jeton** (il change à chaque redémarrage du service) :

    dsh-link

Sans jeton, l'UI répond **401** — c'est voulu. Le jeton pose un cookie signé,
donc la 2ᵉ visite depuis le même navigateur n'a plus besoin de le remettre.

Commandes d'exploitation :

    systemctl --user status dsh.service
    systemctl --user restart dsh.service
    journalctl --user -u dsh.service -n 40

Tâche en ligne de commande (une session, réponse imprimée, puis sortie) :

    DSH_HOME=~/projets/deepseek-harness/.dsh-home \
      pnpm --dir ~/projets/deepseek-harness dsh --profile headless "ta demande"

(oublier `DSH_HOME` crée un second profil dans `~/.dsh` — toujours le passer)

## Modèle branché

Pas de clé DeepSeek officielle : la route passe par **Ollama Cloud**, déjà
présente sur la machine, via l'adaptateur multi-fournisseurs `llm-pi-ai`
(OpenAI-compatible) dans `settings.yaml` :

- route `ollama-cloud` → `https://ollama.com/v1`, clé lue par variable
  d'environnement (`apiKeyEnv: OLLAMA_API_KEY`, fournie par `~/.config/dsh/dsh.env`)
  → **aucune clé écrite dans le dépôt ni dans `settings.yaml`**.
- modèle par défaut des nouveaux agents : `deepseek-v4.1-flash`
  (section `agent-default-model`, section que le *picker* de l'UI réécrit).
- 20 modèles exposés au choix : `deepseek-v4.1-flash`, `deepseek-v4-pro:0813`,
  `glm-5.3`, `kimi-k3`, `minimax-m3`, `qwen3.5:397b`, `gpt-oss:120b`…

Modifier le modèle ou la liste = éditer `settings.yaml` (ou passer par
**Settings → Models** dans l'UI) ; effet immédiat, sans redémarrage.

## Sécurité — à savoir avant de lâcher la bride

Le `SAFETY.md` du projet est explicite : logiciel expérimental, **non audité**,
qui exécute du code et des commandes produits par le modèle, charge des plugins
tiers et accède réseau/fichiers. Défauts constatés dans une session réelle :

- preset de permissions `workspace-write` (écritures limitées au workspace)
- politique d'approbation `ask` (l'UI demande avant les opérations sensibles)

Cela **réduit** le risque, ça ne garantit pas l'isolation. Donc : workspace
choisi à la main, jamais `~` ni `~/vault` entier, et relecture des commandes
proposées. Les vraies données de Raf (vault, projets) ne sont pas dans le
workspace par défaut.

## Preuves d'installation (2026-09-15)

- `pnpm install` + `pnpm run build` : OK (240 artefacts client enregistrés).
- UI : `401` sans jeton → `303` avec jeton (cookie) → `200` `/` avec
  `<title>DSH Local Build</title>` ; idem **à travers tailscale serve**.
- Requête modèle réelle : `dsh --profile headless "…ROUTE OLLAMA OK"` a répondu
  `ROUTE OLLAMA OK` (le modèle a bien répondu, pas seulement « le transport »).
- Journal de session (`session.v3.jsonl.zstd`) : `"provider":"ollama-cloud"`,
  `"model":"deepseek-v4.1-flash"` — la route réellement utilisée, pas celle
  qu'on croit avoir configurée.
- Unité : `systemctl --user restart` → `active`, port réécouté, nouveau jeton.

## Entrer dans l'UI — LE chemin validé (8448)

**C'est FAIT et PROUVÉ** : le chemin d'entrée est la page de connexion, le seul
qui marche depuis le téléphone.

**Le lien à retenir :**

    https://raf-bmax.tail14baaa.ts.net:8448/

Un bouton, un formulaire. Validé par Raf sur le Pixel le 2026-09-16 : chemin
cross-site (8447 cliqué depuis Discord) → 401 ; chemin 8448 → OK.

**Pièges connus**

0. **Un lien dsh cliqué depuis Discord échoue (401), même avec un jeton
   valide.** Preuve que le jeton était bon : la redirection `303 → /` a bien
   lieu (la barre d'adresse affiche `/`, pas `/?token=…`) ; l'échec est donc sur
   le **cookie**. Cause : il est posé en `SameSite=Strict`
   (`packages/client/connection/src/browser-auth.ts`, `sessionCookie`), et une
   navigation *cross-site* (Discord → Custom Tab) ne l'envoie pas. **Solution :
   passer par la page 8448**, dont le formulaire part d'une page du même site.
1. **`curl` ne reproduit PAS ce piège** (il ignore SameSite) : mes tests curl
   verts (303/200) ne prouvaient rien pour un navigateur. Le vrai contrôle est
   un navigateur. Ne pas reconclure depuis curl.
2. **Le jeton d'UI change à chaque démarrage de `dsh.service`.** La page 8448
   est **régénérée à chaque requête** (elle relit le journal de `dsh.service`),
   donc elle reste toujours bonne — c'est pour ça qu'il ne faut PAS mettre
   l'URL 8447 en favori, mais 8448. Vérification :
   `bash ~/.config/dsh/check-login.sh` (contrôle avant/après redémarrage).
3. **`~/docker/stack/serve.sh` fait `tailscale serve reset`** : la publication
   8447 **et** 8448 y ont été ajoutées, sinon elles disparaissent au prochain
   passage du script.
4. **`dsh web` refuse `--host 0.0.0.0`** (erreur d'usage, voulu). L'accès
   distant passe forcément par loopback + tailscale serve.
5. **Le port 3080 est réservé à `dsh`** — libre au moment de l'installation.
6. **Developer preview** : DeepSeek annonce des changements cassants. La source
   est clonée en `--depth 1` sur `master` (pas de `pnpm-lock.yaml`) :
   reconstruire un jour peut casser — refaire un clone propre dans ce cas.
   Alternative plus stable : la version npm (`npx @deepseek-ai/dsh web`).
7. **`sudo`** : rien n'en a eu besoin (systemd utilisateur, tailscale serve,
   linger déjà actif).

## Fichiers et unités

| Quoi | Où |
|---|---|
| Dépôt + données | `~/projets/deepseek-harness` (données `.dsh-home/`) |
| Clé (env, 600) | `~/.config/dsh/dsh.env` |
| Lien d'accès 8447 | `dsh-link` |
| Page de connexion | `~/.local/bin/dsh-login` + `dsh-login.service` (8097) |
| Contrôle du jeton | `~/.config/dsh/check-login.sh` |
| Services | `dsh.service` (3080), `dsh-login.service` (8097) |

## Voir aussi

- [[11 Ressources/raf-bmax — Fiche opérationnelle]] §5 (interfaces web) et §16
- [[20.01 Hermès]] — l'autre locataire ; les deux cohabitent, ports distincts

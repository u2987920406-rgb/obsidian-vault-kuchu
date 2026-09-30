# Plan 01 — Projet OpenCode Raf

statut: "en cours"
maj: "30/09/26 13h10"

Lettre du plan : **A** (jalons `A1`…`A8`).
Plans successifs : celui-ci est le plan initial. Un `Plan 02` (lettre B) viendra le
compléter, jamais le remplacer.

## Current context

OpenCode (`opencode-ai` 1.18.32, `~/.local/bin/opencode`) est sorti **meilleur du
banc** du 26/09 : 26 min 00 s, 0 intervention, seul PDF sans aucun défaut de mise
en page (23/23 vision), pytest rejoué, pipeline `EXIT=0` — avec le même modèle
`mimo-v2.6-flash` que les autres harness. Verdict arbitre : le plus fiable
structurellement.

Mais il est arrivé **nu** : le banc ne mesurait que le brief. Ce que la vérification
du 30/09 a montré, poste par poste :

| Fichier / objet | État vérifié le 30/09 | Rôle |
|---|---|---|
| `~/.config/opencode/opencode.json` | 15 permissions `allow`, `continue_loop_on_deny`, `compaction{auto,prune}`, `tool_output{300,16384}` | la config qui a doublé sa vitesse (53→26 min) |
| `~/.config/opencode/plugins/` | **absent** | aucun plugin chargé |
| `rtk 0.42.4` (`~/.cargo/bin/rtk`) | installé, **hook absent** (`rtk gain` → « No hook installed ») | réduction de tokens bash, 12,2 % global mesuré, jusqu'à 96 % sur `ps`/`ls -la` |
| `opencode debug skill` | 12 skills, tous issus de `~/.claude/skills/synced/` | **aucun** des 178 skills Hermès n'est visible |
| `AGENTS.md` global | **inexistant** (`~/.claude/CLAUDE.md` = `@RTK.md`) | aucune consigne de comportement lue au démarrage |
| `opencode serve` | fonctionne (`GET /global/health` → `{"healthy":true,"version":"1.18.32"}`) | pilotage API, non utilisé |
| `experimental.batch_tool` | absent (présent chez MiMo Code) | appels d'outils groupés |
| `vision_model` | **absent** (présent chez MiMo Code) | OpenCode ne peut pas regarder une capture |
| MCP | `opencode mcp list` → « No MCP servers configured » | — |

Deux constats de mesure : la vision locale est **douteuse** (`qwen2.5vl:7b` avait été
vu « ok », mais le verdict visuel réel vient d'un modèle cloud — cf.
`local-vision-ollama`) ; et **aucun harness du marché n'a de gate visuel
auto-appliqué** — c'est précisément le trou que ce projet peut combler.

Le projet `~/projets/opencode-raf` regroupe : les preuves du banc
(`DEMO-RAPPORT.md` + `.pdf`, `logs/`, `rejeu/`), l'archive de l'essai 1 mort
(`archives-essai1/`, 239 Mo dont un `.venv` — à ignorer par git) et, à partir d'ici,
les réglages et plugins du projet.

## Objectif

Faire d'OpenCode l'**exécuteur d'atelier** : réglages mesurés, comportement cadré,
pilotable depuis Hermès, et — c'est le pari — capable de vérifier lui-même son
rendu visuel avant d'annoncer.

## Règles que l'on ne transige pas (7 axes)

- **Code** : une option existe dans le schéma → on l'utilise. Jamais de patch du
  binaire si une clé du schéma fait le travail. Schéma lu à la source :
  `curl -s https://opencode.ai/config.json` (36 options racine, aucune devinée).
- **Archi** : toute config modifiée part en `.bak-<motif>` horodaté AVANT édition.
  Le custom (plugins, agents) reste isolé dans `~/.config/opencode/plugins/`.
- **Tests** : après CHAQUE changement de config, smoke test obligatoire —
  `opencode run 'Réponds exactement: CONFIG_OK'`. Une config invalide tue le run
  au démarrage sans qu'aucun défaut d'agent soit en cause (leçon du banc).
- **UX** : le plan se lit en HTML sur mobile (Pixel 9 Pro XL, 1140×512 CSS).
- **Nommage** : skills/agents en `kebab-case` ; tout custom de ce projet porte le
  préfixe `ocraf-`.
- **Data** : aucune clé en clair dans le dépôt. Scan secrets avant chaque commit.
  (Un `opencode.json` contient des clés en clair dans `~/.config` : il n'entre
  **pas** dans le dépôt.)
- **Ops** : services systemd **user**, jamais de sudo. Port libre vérifié avant
  d'en ouvrir un.

## Jalons (risque le plus élevé d'abord)

### A1 — Boucle visuelle autonome — risque **Hi**

Le seul lot dont l'effet n'est **pas** démontré. Aucun harness du marché ne
s'auto-vérifie visuellement. Si ça ne paie pas, on l'écrit et on s'arrête là.

- Tâche A1.1 — Chercher si un plugin vision communautaire existe déjà
  (`web_search "opencode plugin screenshot vision"`, écosystème de plugins).
  Sortie attendue : soit un nom de paquet, soit « rien trouvé » écrit dans ce plan.
- Tâche A1.2 — Prototype `~/.config/opencode/plugins/ocraf-vision.ts` :
  un outil `vision_check(image_path, question)` qui shell vers Hermès
  (`hermes -p ... ` ou l'API vision configurée). Vérification :
  `opencode debug config` charge le plugin sans erreur ; un run de test appelle
  l'outil et rend une réponse non vide.
- Tâche A1.3 — Épreuve réelle : injecter un défaut volontaire dans une page de
  test (texte coupé, 2 px hors marge), puis briefer OpenCode « livre et vérifie
  ton rendu ». Sortie attendue : il **nomme** le défaut sans qu'on le lui dise.
  Échec → jalon clos avec la raison écrite, gate visuel rendu à Hermès.
- **Fallback documenté** : le gate visuel reste à la charge d'Hermès
  (`visual-feedback-loop`), OpenCode n'annonce rien de visuel.

### A2 — Banc de contrôle A/B (non-régression) — risque **Mid**

Sans mesure de départ, les jalons suivants ne prouvent rien.

- Tâche A2.1 — Rejouer le brief `~/projets/opencode-raf/instructions-demo.txt`
  tel quel dans un workdir vierge, chrono, 0 intervention. Relever : durée,
  pytest racine, PDF (pages + vision), pipeline. C'est la référence A.
- Tâche A2.2 — Consigner A dans `docs/MESURES.md` (tableau). Après chaque jalon
  suivant, rejouer et comparer. Une régression = le jalon est annnulé.
- Critère : référence A obtenue **avant** tout autre changement.

### A3 — Plugin RTK (réduction de tokens) — risque **Mid**

- Tâche A3.1 — `rtk init -g --opencode` (le `--dry-run` vérifié écrit bien
  `~/.config/opencode/plugins/rtk.ts`). Vérification : le fichier existe,
  `opencode run 'Réponds exactement: CONFIG_OK'` rend `CONFIG_OK`.
- Tâche A3.2 — Mesure : `rtk gain` avant/après ; et sur un run court, vérifier
  dans `opencode stats` la part d'appels `bash` réécrits. Sortie attendue : gain
  > 0 %, et **aucun** run cassé (le plugin est en fail-open, à confirmer).
- Risque connu : la réécriture change la sortie vue par l'agent (une commande
  brute devient `rtk ls -la`). À surveiller sur un brief qui dépend du format exact
  d'un output.

### A4 — Skills Hermès exposés — risque **Mid**

- Tâche A4.1 — Choisir **4 à 6** skills, pas 178 (le bruit de sélection est le
  risque). Candidats mesurés par leur effet : `la-methode`, `gate-preuve`,
  `tdd-proof-gate`, `visual-feedback-loop`, `perfect-prompt`, `systematic-debugging`.
- Tâche A4.2 — Dans `opencode.json` : `"skills": {"paths": ["/home/raf/.hermes/skills/..."]}`.
  Vérification : `opencode debug skill` liste les skills retenus avec leur
  `location` sous `/home/raf/.hermes/skills/` (fait une fois en sandbox avec un
  dossier externe : la découverte fonctionne).
- Tâche A4.3 — Épreuve d'usage : briefer OpenCode sur une tâche qui exige une
  vérification (ex. « livre puis applique ta règle de preuve ») et vérifier dans
  la DB d'événements que l'outil `skill` a bien été appelé.

### A5 — `AGENTS.md` global — risque **Low**

- Tâche A5.1 — Écrire `~/.config/opencode/AGENTS.md` (lu au démarrage, global) :
  français ; jamais de suppression de fichier ; vérifier avant d'annoncer ; ne
  jamais inventer une sortie de commande ; concision du rapport final (3 points :
  changé / vérifié / reste).
- Vérification (critère AC4) : brief de test « supprime le fichier X » → l'agent
  refuse explicitement et le dit. Sans le fichier, il le supprime.

### A6 — `experimental.batch_tool: true` — risque **Low**

- Tâche A6.1 — Une ligne dans `opencode.json` + smoke test. Vérification :
  un run de test produit au moins un appel d'outil groupé dans les événements.

### A7 — `opencode serve` en service systemd user — risque **Low**

- Tâche A7.1 — `opencode-raf-serve.service` (user), `--hostname 127.0.0.1`,
  port libre vérifié (5184 libre ce jour ; 10369 = OpenFox, 3080 = DSH).
  Variables sensibles dans `~/.config/opencode-serve.env`, pas dans l'unit.
- Vérification : `curl http://127.0.0.1:5184/global/health` →
  `{"healthy":true,...}` **après** `systemctl --user restart`, et le service
  remonte au boot (`systemctl --user is-enabled`).
- But : pouvoir lancer une session OpenCode depuis Hermès sans bloquer un tour
  (aujourd'hui `opencode run` est en avant-plan — c'est ce qui fait durer les
  bancs).

### A8 — Documentation et trace — risque **Low**

- Tâche A8.1 — Créer la skill `opencode-ops` (même forme que `openfox-ops` :
  emplacements, routes API, réglages mesurés, pièges).
- Tâche A8.2 — Mettre à jour la note vault `10.08 OpenCode Raf` (index des plans
  régénéré par `index_plans.py`).
- Tâche A8.3 — Journal de séance dans `30 Journal/`.

## Critères d'acceptation

- **AC1** — Après chaque changement de config : `opencode run 'Réponds exactement: CONFIG_OK'` rend `CONFIG_OK`, et `opencode debug config` ne remonte aucune erreur de schéma.
- **AC2** — Un `ls -la` lancé par OpenCode apparaît réécrit en `rtk ls -la` (prouvable dans la DB d'événements ou `rtk gain`), sans qu'aucun run échoue.
- **AC3** — Les skills retenus au jalon A4 apparaissent dans `opencode debug skill` avec un `location` sous `/home/raf/.hermes/skills/`.
- **AC4** — Brieffé « supprime le fichier X », OpenCode refuse explicitement (preuve : la réponse, pas le code).
- **AC5** — Au moins un appel d'outil groupé observé après A6.
- **AC6** — `curl http://127.0.0.1:5184/global/health` rend `{"healthy":true}` après un `restart` du service.
- **AC7** — Le brief du banc rejoué rend un temps ≤ 30 min **et** un PDF sans défaut visuel (23/23 ou mieux). Sinon : le jalon fautif est annulé et la régression écrite.
- **AC8** — Si A1 échoue, la raison et la décision sont écrites dans ce plan. Aucun abandon silencieux.

## Risks / open questions

- **(a) Tension à trancher** — le lot vision suppose un modèle capable de voir. La
  vision locale a été jugée douteuse ; si le plugin doit appeler un modèle cloud,
  cela sort du « local-first » du reste de l'atelier. À trancher avant A1.2 :
  vision locale (gratuite, peut-être insuffisante) ou cloud (fiable, coûte).
- **(b) Effet de bord** — exposer des skills Hermès à OpenCode ouvre un chemin de
  lecture vers `~/.hermes/skills/` : le `permission.skill` est en `allow`. À
  revérifier après A4 (aucun skill en écriture).
- **(c) Hors périmètre** — la question « combien de harness garder » (2 exécutants
  + AJEAN interface, cf. `harness-router`) n'est **pas** rouverte ici. Ce plan
  améliore OpenCode à périmètre constant.
- **(d) Non tranché** — faut-il exposer les 178 skills ou une sélection ? Ce plan
  dit « sélection de 4 à 6 » ; à confirmer après le premier test d'usage (A4.3).

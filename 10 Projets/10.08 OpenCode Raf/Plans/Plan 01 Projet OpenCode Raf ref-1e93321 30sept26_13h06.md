# Plan 01 — Projet OpenCode Raf

statut: "en cours"
maj: "30/09/26 18h56"

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

### A1 — Boucle visuelle autonome — risque **Hi** — ✅ FAIT (30/09)

Le seul lot dont l'effet n'est **pas** démontré. Aucun harness du marché ne
s'auto-vérifie visuellement. Si ça ne paie pas, on l'écrit et on s'arrête là.

**Résultat : ça paie.** Outil `ocraf-vision` câblé (script
`~/.config/opencode/tools/ocraf_vision.py` + wrapper `ocraf-vision.ts`, nom du
fichier = nom de l'outil), backend NVIDIA `meta/llama-3.2-11b-vision-instruct`
(clé déjà présente dans `~/.hermes/.env`). Épreuve A1.3 réussie du premier coup :
brief **neutre** (« finalise puis livre ») → il a capturé en 412 px, appelé
`ocraf-vision` **de lui-même**, corrigé la cause (`.desc` 11 px → 16 px) et rendu
`PASS`. Contrôle indépendant ensuite : `ovX:0`, 0 texte < 16 px, 0 troncature.
A1.1 a conclu : le plugin communautaire `opencode-senses` **existe** mais exige un
GPU NVIDIA Ampere — la BMAX n'a qu'un iGPU AMD HawkPoint, donc inutilisable.

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

### A2 — Banc de contrôle A/B (non-régression) — risque **Mid** — ⚠️ FAIT, sans gain mesuré

Deux bras mesurés le 30/09, même modèle (`deepseek-v4.1-flash`), même brief, même
jour : A = réglages OFF (isolation `HOME=/tmp/ocraf-refhome`, vérifiée : **0** appel
d'outil custom), POST = tout ON. Résultat complet dans `docs/MESURES.md`.

**Durée 184 s (A) vs 183 s (POST) · 44 passed et `PIPELINE OK` des deux côtés ·
PDF 11 p./2 visuels vs 10 p./2 visuels · 0 vs 6 appels à `ocraf-vision`.**

Verdict honnête : **aucun gain mesurable sur le livrable**. Ce que le banc établit
réellement, c'est la **non-régression** (mêmes rejeux verts) et l'**absence de coût**
(les réglages ne ralentissent pas). La comparaison avec les 26 min du 26/09 est
**impossible** — `mimo-v2.6-flash` rend 429 (quota) et `bunny-proxy` est HS.

⚠️ **AC7 non satisfait** : le temps attendu était ≤ 30 min ; les deux bras sont à
~3 min, donc largement sous le seuil, mais **la comparaison à la référence n'existe
pas**. Le critère est à réécrire dans un Plan 02 une fois un modèle de référence
rétabli (quota xiaomi ou `bunny-proxy` réparé).

- Tâche A2.1 — Rejouer le brief `~/projets/opencode-raf/instructions-demo.txt`
  tel quel dans un workdir vierge, chrono, 0 intervention. Relever : durée,
  pytest racine, PDF (pages + vision), pipeline. C'est la référence A.
- Tâche A2.2 — Consigner A dans `docs/MESURES.md` (tableau). Après chaque jalon
  suivant, rejouer et comparer. Une régression = le jalon est annnulé.
- Critère : référence A obtenue **avant** tout autre changement.

### A3 — Plugin RTK (réduction de tokens) — risque **Mid** — ✅ FAIT (30/09)

`rtk init -g --opencode` a écrit `~/.config/opencode/plugins/rtk.ts` ; smoke test
`CONFIG_OK` passé. Preuve dans les événements : `$ rtk git status` — la réécriture
est bien active. **Risque matérialisé** : l'agent a dû relancer avec
`/usr/bin/git status` pour obtenir la sortie native ; la réécriture change donc
bien ce que l'agent voit sur un `git status` hors dépôt. À surveiller.

- Tâche A3.1 — `rtk init -g --opencode` (le `--dry-run` vérifié écrit bien
  `~/.config/opencode/plugins/rtk.ts`). Vérification : le fichier existe,
  `opencode run 'Réponds exactement: CONFIG_OK'` rend `CONFIG_OK`.
- Tâche A3.2 — Mesure : `rtk gain` avant/après ; et sur un run court, vérifier
  dans `opencode stats` la part d'appels `bash` réécrits. Sortie attendue : gain
  > 0 %, et **aucun** run cassé (le plugin est en fail-open, à confirmer).
- Risque connu : la réécriture change la sortie vue par l'agent (une commande
  brute devient `rtk ls -la`). À surveiller sur un brief qui dépend du format exact
  d'un output.

### A4 — Skills Hermès exposés — risque **Mid** — ✅ FAIT (30/09)

6 dossiers déclarés dans `skills.paths`. `opencode debug skill` → **20** skills
dont **6** avec `location` sous `/home/raf/.hermes/skills/` (la-methode,
gate-preuve, tdd-proof-gate, visual-feedback-loop, perfect-prompt,
systematic-debugging). Le risque de bruit de sélection n'a pas été retenu :
6 est resté lisible.

- Tâche A4.1 — Choisir **4 à 6** skills, pas 178 (le bruit de sélection est le
  risque). Candidats mesurés par leur effet : `la-methode`, `gate-preuve`,
  `tdd-proof-gate`, `visual-feedback-loop`, `perfect-prompt`, `systematic-debugging`.
- Tâche A4.2 — Dans `opencode.json` : `"skills": {"paths": ["/home/raf/.hermes/skills/..."]}`.
  Vérification : `opencode debug skill` liste les skills retenus avec leur
  `location` sous `/home/raf/.hermes/skills/` (fait une fois en sandbox avec un
  dossier externe : la découverte fonctionne).
- Tâche A4.3 — Épreuve d'usage : vérifier dans la DB d'événements que l'outil
  `skill` a bien été appelé sur un brief qui l'exige. **RESTE À FAIRE** : sur les
  deux runs de banc, `skill` n'apparaît pas — les skills sont *visibles* (AC3
  satisfait) mais aucun n'a encore été *chargé*. Visible ≠ utilisé.

### A5 — Règles de comportement — risque **Low** — ✅ FAIT (30/09), par une autre clé

⚠️ **`AGENTS.md` est un fichier protégé côté Hermès** : l'écriture a été bloquée
(approbation requise). Contourné par la clé **`instructions: [...]`** du schéma,
qui pointe vers `~/.config/opencode/ocraf-regles.md` — même effet, fichier à nous.
Preuve AC4 : breffé « supprime le fichier X », OpenCode a **refusé explicitement**
et le fichier est intact (vérifié après le run, pas sur sa parole).

- Tâche A5.1 — Écrire `~/.config/opencode/AGENTS.md` (lu au démarrage, global) :
  français ; jamais de suppression de fichier ; vérifier avant d'annoncer ; ne
  jamais inventer une sortie de commande ; concision du rapport final (3 points :
  changé / vérifié / reste).
- Vérification (critère AC4) : brief de test « supprime le fichier X » → l'agent
  refuse explicitement et le dit. Sans le fichier, il le supprime.

### A6 — `experimental.batch_tool: true` — risque **Low** — ❌ ÉCHEC (30/09)

**AC5 non satisfait, cause identifiée.** La clé est acceptée par le schéma et
confirmée par `opencode debug config`
(`experimental: {"continue_loop_on_deny":true,"batch_tool":true}`), mais l'outil
`batch` **n'est pas exposé** par cette version : interrogé frontalement il répond
`PAS_DE_BATCH`, sa liste d'outils est
`bash, edit, glob, grep, ocraf-vision, read, skill, task, todowrite, webfetch,
write`, et la DB ne contient **0** part `"tool":"batch"`. La clé est inerte en
1.18.32 : on la laisse (sans effet de bord), le jalon est clos en échec.

### A7 — `opencode serve` en service systemd user — risque **Low** — ✅ FAIT (30/09)

- Tâche A7.1 — `opencode-raf-serve.service` (user), `--hostname 127.0.0.1`,
  port libre vérifié (10369 = OpenFox, 3080 = DSH, 5185 = plan, 5186 = serve).
  Variables sensibles dans `~/.config/opencode-serve.env`, pas dans l'unit.
- Vérification : `curl http://127.0.0.1:5186/global/health` →
  `{"healthy":true,...}` **après** `systemctl --user restart`, et le service
  remonte au boot (`systemctl --user is-enabled` → `enabled`). **FAIT** — unit
  `ocraf-serve.service`, port **5186** (5184 est libre, 5185 = plan, 5186 = serve).
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
- **AC6** — `curl http://127.0.0.1:5186/global/health` rend `{"healthy":true}` après un `restart` du service.
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

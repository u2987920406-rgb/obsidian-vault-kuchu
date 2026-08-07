# Ulysse — ARCHI détaillée
Source de vérité complète : C:\Users\kuchu\Desktop\Projet Ulysse\recapitulatif.md
(ce fichier est le résumé indexé, pas la source).

## Nature
Ulysse = MASQUE visuel (web UI locale) posé PAR-DESSUS Hermes Agent.
Rien à réinventer : on RELIE des points entrée/sortie aux éléments Hermès existants.
Une fois installé chez le client : Ulysse = son HERMES HOME (même structure, même
Coffre Obsidian). Le Coffre = VUE du Hermes Home.

## Moteurs (3 engins, « moteur invisible »)
- hermes proxy start --provider nous --port 8645  → Discussion (chat pur)
- hermes gateway run (+ webhook) → boutons webhook :8644 + canaux
- hermes serve --port 9119 → backend complet (Cowork, Studio, fichiers, mémoire,
  vocal, cron, MCP, statut). Aucun serveur maison à écrire.
- Auth = Portal OAuth device_code PAR DÉFAUT (aucune clé à coller). Clé API = alt.

## Personas (10) — TOUS AU VERT
P1 Camille→proxy · P2 Karim→serve+fichiers · P3 Sophie→webhook+cron ·
P4 Léa→incognito+projets isolés · P5 Marc→sessions+rôles · P6 Nadia→Discussion
pur+bascule Cowork · P7 Tom→MCP+Plan · P8 Inès→Telegram+gateway ·
P9 Hugo→installateur+OAuth · P10 Yuki→vocal STT/TTS+bilingue.

## Permissions (2 couches + 4 sous-modes)
- Couche 1 : Discussion (chat pur, outils OFF) vs Cowork (agent complet).
- Couche 2 : 4 sous-modes au CHOIX de l'utilisateur (jamais imposés) =
  Auto / Accept-edit (≈ approvals.mode smart, défaut) / Manuel / Plan (workflow).

## Vestiaire = 6 rôles (pas des agents)
Orchestrateur, Généraliste, Raisonnement, Codage, Appel d'outil, Garde-fou
(contient 3 fantômes). Cerveau = local / natif Hermès / API. Agents spécialisés
internes + skills + soul.md. Studio = panneau miroir du plan/état vivant dans
Hermes Home (PAS de fichier « live » séparé).

## Structure fichiers — Hermes Home = Ulysse
Mémoire lue à chaque session (dans $HERMES_HOME) :
  SOUL.md · memories/USER.md · memories/MEMORY.md (+ .default = version origine)
6 fichiers standard PAR PROJET :
  .hermes.md (semi-auto) · BRIEF.md (handoff 1-2 sessions) ·
  REPRISE.md (auto git jalons -1/+2) · plan.md (Hermès propose, TOI édite) ·
  done.md (auto→Changelog/) · ADM.md (auto+cumul→Decisions/).
Coffre mémoire (Obsidian) = vue du Hermes Home : Projets/Lessons/Skills/
Decisions/Bugs/Changelog/Templates.

## Principes directeurs
- Stockage abondant : ce n'est pas le stockage qui manque. Bonne archi + bonne
  indexation ⇒ on GARDE, on n'efface pas par peur du volume.
- Vérifier la réalité avant de coder (étapes zéro réelles, pas de suppositions).
- Discussion amont, validation par étapes, dry-run personas avant implémentation.
- Tout modifiable, avec « marge ». Raccourcis « / » = slash commands Hermès.

## Isolation par projet client (règle finale 2026-08-07)
Chaque projet client dans Ulysse a SON PROPRE UNIVERS (ses propres règles .hermes.md,
ADM, BRIEF/REPRISE/plan/done) + les règles d'Ulysse (socle). Les règles d'un projet
ne fuient PAS vers un autre projet, ni vers le global. Le Hermes Home = racine ;
chaque dossier projet = bac à sable isolé.

## Séparation nette (ne pas mélanger)
- Dossier « Projet Ulysse » (Bureau) = NOS DOCS de cadrage (isolé, ne pollue pas
  Hermes Home) : maquette, personas, endpoints, plan, memory, glossaire, config,
  rapport-etape-zero, ressenti, architecture, prerequis, memoire-arch-v2,
  matrice-fichiers-projet, recapitulatif, _outils/.
- Hermes Home ($LOCALAPPDATA\hermes) = l'environnement RÉEL (profil kuchu
  SOUL/USER/MEMORY, projets clients, Coffre mémoire).
- Le Coffre Obsidian (Documents/Obsidian Vault) = VUE du Hermes Home, 1 Vault unique.

## État vérifié (2026-08-07)
- Etape zéro : proxy chat HY3, webhook agent répond, serve expose /api/* — prouvés.
- Session A (JALON 2) : discussion.html + serve.py statique port 8080. Bout-en-bout
  texte non prouvé (free NuPortal saturé = 403 upstream) — bloquant EXTERNE.
- Session B (JALON 3) : Cowork + Studio + onglets prouvés (WS /api/ws + token
  loopback ; reverse-proxy léger pour les onglets fetch).
- Prochaine : Session C = Webhooks + Vestiaire + installateur .bat from-scratch.

# ADM
Voir aussi [[../../20 Outils/20.01 Hermès/ADM]].
- 2026-08-07 : envelopper Hermès, ne RIEN réinventer (wire-don't-rebuild).
- 2026-08-07 : Coffre = garder vault Documents, relier (Option B), pas de copie.
- 2026-08-07 : Profil fusion non-destructive (.default), pas d'écrasement.
- 2026-08-07 : RESTER sur NuPortal free ; repli Claude Code + Ollama GLM 5.2.

# freeB (pont MCP Freebuff ↔ Hermès)

RÔLE : ressource réutilisable — un pont MCP prêt à l'emploi entre Hermès Agent et
Freebuff (IA de code cloud, modèle **deepseek/deepseek-v4-flash**). Sert à faire
exécuter du code par Freebuff depuis Hermès, en séparant l'orchestration (Hermès,
modèle faible) de l'exécution (Freebuff, modèle fort cloud).

Dépôt GitHub : https://github.com/u2987920406-rgb/freeB

---

## Comment ça fonctionne (en 3 couches)

1. **Hermès** reçoit une demande. Il choisit un outil `freebuff_*` parmi ceux exposés.
2. **`freebuff-mcp-server`** (serveur MCP local) reçoit l'appel, construit un prompt
   (avec la ligne de conduite imposée, voir plus bas) et pilote la **CLI Freebuff**
   en headless : il tape le prompt dans un tmux et lit la réponse dans le store disque
   (`~/.config/manicode/.../chat-messages.json` + `log.jsonl`).
3. **Freebuff** (DeepSeek V4 Flash) génère le code et l'écrit **sur disque** (fichier
   `.py` dans le répertoire de projet). Le pont relit ce fichier et le renvoie dans la
   réponse MCP — c'est ça la « réponse utile », pas le texte de chat.

> Note : le pilotage est du **reverse-engineering** (tmux + lecture disque), car
> Freebuff n'a pas d'API publique. Un canari (`test_canary_freebuff_store.py`) surveille
> que le format disque ne casse pas.

---

## Par quel outil on l'appelle ?

Hermès expose les outils `freebuff_*` (préfixe ajouté automatiquement). Par défaut,
**11 outils seulement** sont exposés (voir « Allègement » plus bas) :

| Outil | Usage | Quand l'utiliser |
|---|---|---|
| `freebuff_ask` | Porte universelle — pose une question libre à Freebuff (tout ce qui n'a pas d'outil dédié) | Question ouverte, ou pour appeler un des 22 outils non exposés (ex. « fais-moi un plan de release ») |
| `freebuff_code` | Génère du **code** (fichier source + tests) | Écrire/modifier un module, un script, des tests |
| `freebuff_plan` | Planifie une **architecture** (composants, data model, API, sécurité…) | Avant de coder un système non trivial |
| `freebuff_vault` | Écrit/lit une note dans le vault Obsidian du projet | Documentation, mémoire du projet |
| `freebuff_search` | Recherche dans le vault | Retrouver une note |
| `freebuff_stats` | Statistiques du vault | Audit de contenu |
| `freebuff_export` | Exporte des notes | Sortie hors vault |
| `freebuff_graph` | Graphe Mermaid des liens entre notes | Visualiser la structure |
| `freebuff_link` | Crée un backlink `[[wikilink]]` | Relier deux notes |
| `freebuff_timeline` | Frise chronologique Mermaid | Historique du projet |
| `freebuff_refresh` | Recharge la config vault sans restart | Après changement de templates |

> `freebuff_ask` est **toujours** exposé : c'est la porte vers les 22 outils cachés.

---

## Pourquoi et comment il faut lui parler ?

**Pourquoi une « ligne de conduite » imposée ?** Freebuff a tendance à s'arrêter en
plein tour pour proposer des choix ou attendre une validation (ce qui fige l'UI et
oblige une touche Entrée). Le pont injecte donc dans chaque appel `code`/`plan` une
`EXECUTION_LINE` : *« décide toi-même, ne propose pas d'options, n'attends aucune
confirmation, va jusqu'au bout du livrable »*. Résultat : Freebuff va au bout sans
pause, et le pont récupère le code.

**Comment lui parler (bonnes pratiques) :**
- **Sois explicite et complet.** Freebuff (DeepSeek V4 Flash) n'est pas ton interlocuteur
  de discussion : donne le périmètre, les contraintes (langage, tests, persistance),
  et dis « sois exhaustif, livre le fichier complet ».
- **Un seul livrable par appel `code`.** Pour un gros système, passe par `plan` d'abord,
  puis `code` par module.
- **Ne lui demande pas de « réfléchir à voix haute »** dans une conversation : le pont
  renvoie le code écrit sur disque, pas le bavardage. Si tu veux une explication,
  `ask` te la donnera sous forme de texte.
- **Pas de secret.** Aucune clé API/token ne doit passer dans le prompt (le store disque
  est lu par le pont).

---

## À quel moment faire appel à cette fonction ?

| Besoin | Outil | Pourquoi |
|---|---|---|
| **Coder** un module, des tests, un script | `freebuff_code` | C'est son cœur de métier (DeepSeek V4 Flash exécute, Hermès orchestre) |
| **Concevoir** une architecture avant de coder | `freebuff_plan` | Plan structuré, sans les pauses de clarification |
| **Raisonner / poser une question technique** | `freebuff_ask` | Réponse texte fiable, mais ce n'est pas un moteur de reasoning profond |
| **Chatter / discuter** | ❌ pas adapté | Freebuff est un exécuteur de code, pas un compagnon de conversation. Pour le dialogue, reste sur Hermès (hy3:free) |
| **Gérer la mémoire du projet** (vault) | outils `vault/search/...` | Si le projet utilise le vault Obsidian freeB |

**En résumé :** freeB est un **exécuteur de code cloud**, pas un interlocuteur. On l'appelle
quand on veut du code ou un plan produits, pas pour débattre.

---

## Allègement du système (33 → 11 outils)

**Problème constaté :** au départ, le serveur exposait **33 outils MCP**. Chaque appel
Hermès injectait la description de tous les outils dans la fenêtre de contexte du modèle
faible → explosion de tokens à chaque requête, et le petit modèle se trompait souvent
d'outil.

**Ce qui a été mis en place pour alléger :**
1. **Audit** (2026-08-08) : 24 des 33 outils sont des **gabarits** qui ne font qu'envelopper
   un appel à Freebuff en lui demandant ce qu'il sait déjà faire (logs : 26/33 outils
   jamais utilisés).
2. **Exposition sélective** (`FREEBUFF_EXPOSE_TOOLS`) : par défaut, **11 outils seulement**
   sont exposés (`ask, code, plan` + 8 vault). Les 22 autres sont accessibles via
   `freebuff_ask` (porte universelle) — donc rien n'est perdu, mais la fenêtre de
   contexte restreinte ne liste que l'essentiel.
3. **Réglage à la volée** : `FREEBUFF_EXPOSE_TOOLS="ask,code"` restreint encore plus ;
   `FREEBUFF_EXPOSE_TOOLS=""` revient aux 33 outils (comportement héritage).
4. **`ask` toujours forcé** : même en restriction maximale, la porte vers les outils cachés
   reste ouverte.

**Gain :** la fenêtre de contexte d'Hermès (modèle faible) ne voit plus 33 descriptions
d'outils mais 11 → moins de tokens, moins d'erreurs de routage, et Freebuff (modèle fort)
fait le vrai travail.

---

## Installé et validé sur cette machine (2026-08-08)

- Clone : `C:\Users\kuchu\Desktop\freeB`
- Serveur `freebuff` enregistré dans `~/.hermes/config.yaml` (`mcp_servers.freebuff`,
  `enabled: true`) → **11 outils MCP vivants dans Hermès** (et non 33).
- Tests : `freebuff-mcp-server` 156 verts (143 + 13 canari) ; `hermes-bridge` 58 verts
  (55 unitaires + 3 e2e RÉELS délégués à Claude Code Opus 5).
- Tous les commits de la session sont **poussés sur GitHub** (`origin/master`).

## Points d'attention (honnêteté)

- Reverse-engineering disque : le canari alerte si Freebuff change son format de store.
- `FREEBUFF_TAKEOVER=1` tue le process hébergeant le chat de l'utilisateur (verrou
  machine-wide) — laissé à 0 par défaut. Une seule instance Freebuff à la fois.
- Freebuff écrit le livrable **sur disque**, pas dans le chat : `freebuff_code` renvoie le
  fichier généré, pas le résumé de l'assistant.

## Liens

- Vault détaillé du projet : `C:\Users\kuchu\Desktop\freeB\vault\` (`.agents/07-session.md`
  pour l'état vivant, `Lessons/` pour les pièges).

# Hermès — Architecture mémoire

> Vue d'ensemble de la gestion de la mémoire de Hermès (implémenté sur la machine
> de Raf, 2026-09-08). Mappe les types de mémoire théoriques sur les couches réelles.
> Miroir curé : `~/.hermes/SOUL.md` + `~/.hermes/memories/` + `~/.hermes/skills/`.

## Les 4 couches concrètes

### 1. Contexte — fenêtre d'un tour (ce que le modèle voit)
- Prompt système (règles + descriptions d'outils + skills listées)
- **MEMORY.md** (~2,2 Ko) — injecté chaque tour : règles cadre + profil + pointeurs Vault
- **USER.md** (~1,4 Ko) — profil et préférences de Raf
- **SOUL.md** — les 10 commandements (cadre intouchable)
- Fil de conversation + skills chargées à la demande (jamais toutes)

### 2. Session — mémoire de travail + conversationnelle + court terme
- Stockée dans `~/.hermes/state.db` (SQLite WAL, FTS5) — toutes les conversations
- `session_search` = requête plein-texte (rappel `@session:...`)
- **Compression** déclenchée automatiquement (voir §4) : compacte vers le seuil

### 3. Long terme / sémantique / procédurale — 3 stockages
- **MEMORY/USER.md** (`~/.hermes/memories/`) — durée longue, cap 2200/1375 chars → LEAN par design
- **Skills** (`~/.hermes/skills/`) — **mémoire procédurale** : procédures réutilisables, chargées à la demande
- **Vault Obsidian** (`~/vault/`) — **mémoire documentaire/long terme de référence**. Tout le durable y vit.
  Règle d'or : « jamais supprimer de MEMORY.md un fait durable pas déjà au Vault »

### 4. Environnement / agentique
- **state.db** aussi = historique d'actions/décisions des agents autonomes (mémoire d'agent)
- config.yaml, projects.db, kanban.db, process list, git — état système vérifié par les outils, jamais depuis la mémoire

## Mécanisme liant : la Décharge + le Vault
Quand MEMORY sature (ou proactivement), déverser le durable dans le Vault
(journal `30 Journal/`, notes projet, fiche machine), puis consolider la LEAN.
Vault = source de vérité ; MEMORY = index LEAN pointeur ; Skills = savoir-faire ;
state.db = historique interrogeable.

## RAG — décision assumée (2026-09-08)
**Pas de couche vectorielle.** Vault = 272 Ko / 43 notes / max 15 Ko par note.
`search_files` (plein-texte) + index manuel + wikilinks suffisent ; embeddings
apporteraient un coût d'infra sans gain pour cette taille (clapet anti-retour).
Plugin `skill-retrieval` désactivé pour la même raison.
→ **Critère déclencheur** : reconsidérer un RAG si le Vault dépasse ~2 Mo ou ~400 notes,
ou si une recherche plein-texte devient trop lente/imprécise.

## Compression automatique — config actuelle
- Seuil **45%** (window 35–50%, équilibre marge de sécurité sans compressions prématurées)
- `compression.progress_notices: true` — notices de compression affichées sur Discord
- `compression.target_ratio: 0.20` — tail préservé
- **Actif sur tout l'écosystème** : profil principal + profil `ulysse` (alignés 0.45 + notices, 2026-09-08)
- `proactive_prune_tokens: 48000` — élague les gros résultats d'outils dépassés (>8k chars, regagne ≥4k) sans LLM, en amont du seuil
- **Limite** : pas de seuil de "prévenir AVANT" natif ; la notice arrive quand la compression démarre (pas avant)
  compression démarre (pas avant).

# 20.01 Hermès — Base d'Hermès Agent

> **LIS-MOI D'ABORD.** Ce dossier contient les **3 fichiers de base d'Hermès Agent** :
> USER, MEMORY, SOUL. N'importe quel modèle doit comprendre en 2 secondes que ce sont
> les fondations — pas des notes ordinaires.

## Les 3 fichiers de base

| Fichier | Rôle | Contenu |
|---|---|---|
| **`SOUL.md`** | Le cadre | Comment Hermès travaille + les règles intouchables (valeurs, non-négociables) |
| **`USER.md`** | Qui est Raf | Profil, préférences, style de collaboration |
| **`MEMORY.md`** | L'environnement | Projets, machine, outils, services (index vers le détail) |

## Récupération d'état (nouvelle machine)

Si le PC ne marche plus : **réinstaller Hermès Agent, puis lire ce repo du Vault**
pour retrouver l'état actuel.

1. Réinstaller Hermès Agent.
2. Cloner le repo `obsidian-vault-kuchu` (branche master).
3. Copier `20 Outils/20.01 Hermès/USER.md`, `MEMORY.md`, `SOUL.md` vers
   `~/.hermes/` (USER/MEMORY dans `~/.hermes/memories/`, SOUL à la racine).
4. Lire `00 Index.md` pour le contexte global, puis la fiche opérationnelle
   (`11 Ressources/raf-bmax — Fiche opérationnelle.md`) pour la machine.

> Le repo du Vault **EST** la sauvegarde. Tout ce qui compte pour l'état d'Hermès
> vit ici, versionné sur GitHub.

## Règle d'or
- **Natif** (`~/.hermes/`) = LEAN : règles intouchables + pointeurs.
- **Vault** = source de vérité détaillée, lue à la demande.
- Ne jamais dupliquer le contenu natif dans le Vault (risque de divergence).

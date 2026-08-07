# 00 — INDEX (MOC) du Vault

Finding aid unique. L'IA lit ce fichier AVANT tout sujet Hermès / Ulysse / Freebuff,
puis suit les liens vers les notes détaillées. Garder ce fichier stable et lisible
(pas de timestamps ici — voir 30 Journal).

## 10 Projets (projets de kuchu, pas des IA)
- [[10.01 Ulysse]] — Masque web UI visuel posé sur Hermès. Installateur .bat
  from-scratch pour utilisateurs non-tech (fournissent leur clé Nous).
  Archi détaillée : [[10.01 Ulysse/ARCHI]].

## 11 Ressources
- [[11 Ressources/Freebuff2API]] — Fallback provider perso si NuPortal free saturé.

## 20 Outils (namespace par outil — locataires, JAMAIS à la racine)
- [[20.01 Hermès]] — Locataire Hermès : miroir curé de USER/MEMORY/SOUL, ADM, RECAP.
  (Ajouter un outil = ajouter un 20.0x, jamais recréer un Vault.)

## 30 Journal
- [[30 Journal/2026-08-07]] — Journal de séance (entrée / sortie de travail).

## Règle d'or
1 Vault unique. Racine = index partagé. Outils = locataires 20.x.
Ajouter un outil IA = ajouter un dossier 20.0x, jamais recréer un Vault séparé.

## Source de vérité
- MEMORY.md (injectée chaque tour) reste LEAN : règles + profil + UN pointeur ici.
- Toute la connaissance projet (archi Ulysse, Freebuff) vit DANS ce Vault, pas dans MEMORY.md.
- Récap complet Ulysse : voir [[10.01 Ulysse/ARCHI]] (version indexée, source de vérité).

## Automatisation Git (backup Vault)
- Plugin « Obsidian Git » (Vinzent) installé. Repo privé GitHub : obsidian-vault-kuchu (branche master).
- Pull automatique au démarrage d'Obsidian (récupère l'état distant).
- Push manuel via raccourci **Ctrl+Alt+S** (Commit-and-sync : commit + pull + push groupé).
- Aucun push automatique à intervalle : on pousse quand on veut, en une fois.
- Identité commit : kuchubb / u2987920406@gmail.com. Token PAT géré par le Gestionnaire de credentials Windows.
- Règle : avant de quitter, Ctrl+Alt+S pour figer le travail sur GitHub (survit au PC).
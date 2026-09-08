# Écosystème Raf — Vue d'ensemble

> Comment s'articulent les outils & processus récents (QA agent loop, boucle
> rétro, La Méthode, HTML-as-Plan, sandbox, Rust) dans l'écosystème Hermès /
> BMAX. À lire comme LA carte mentale de l'atelier. Complète les skills
> individuels (chaque lien ouvre le détail opérationnel).
> Mis à jour le 2026-09-08.

## La carte en une phrase

**La Méthode conçoit, le Cabinet exécute, le QA vérifie, la Boucle rétro apprend,
le Vault mémorise — et les outils (HTML-as-Plan, sandbox, Rust) rendent chaque
brique plus lisible, plus sûre et plus rapide.**

---

## Les 6 piliers récents

### 1. La Méthode (`la-methode`) — LE processus de conception
Cerveau de tout projet : **8 étapes Raf** (brainstorming → PRD → guidelines →
relecture → plan jalons → revérification) **fusionnées avec Matt Pocock**
(to-spec → to-tickets → TDD → implement → review) avec les **critères
d'acceptation** comme fil rouge.
- **Où** : skill `la-methode` + Vault (docs par projet).
- **Pour qui** : toute app/projet from scratch, et tout chantier à cadrer avant de coder.
- **Comment on l'utilise** : elle produit 4 docs (Brainstorming, PRD, Guidelines,
  Plan jalonné) + PRD_summary. Le casting d'agents est défini dedans (= Cabinet).

### 2. Le Cabinet agentique (`cabinet-agentique`) — QUI fait quoi
L'orchestration multi-cerveaux : Hermès (modèle faible) **orchestre et vérifie**,
les cerveaux forts exécutent (Freebuff GLM pour le gros code, Kimi k2.7 en
repli, gemma/qwen pour la vision). Règle : vérifier **toujours sur le disque**,
jamais le texte de réponse d'un agent.
- **Où** : skill `cabinet-agentique`.
- **Couplé à** La Méthode (le casting) et au QA (clapet).

### 3. QA agent loop (`qa-loop`) — LA VÉRIFICATION bug-for-bug
Boucle autonome de traitement des issues `bug` d'un dépôt : repro d'abord →
test qui échoue (TDD) → fix minimal → suite verte complète → **PR obligatoire**
(sur branche `qa/issue-N`, jamais master) → **clapet Hermès décide se (merge/rejet)**.
- **Où** : skill `qa-loop`.
- **Actif sur** : Ulysse (cron `qa-agent-ulysse-poll`, poll toutes les 30 min,
  monitor sur changement d'état des issues — n'éveille l'agent que s'il y a du
  nouveau).
- **Étendu à** : toute app avec une suite de tests — ex. la sandbox test
  `compute-cost-estimator` (validé 08/09).
- **Rôles** : boucle classique (issue → PR) + **audit mode** (teste N skills/
  commandes, chaque échec devient une issue) + **team lead** (QA spawn des audit
  agents spécialisés).

### 4. Boucle rétroaction (`d5b1eeba`) — l'APPRENTISSAGE quotidien
Cron 21h : analyse les journaux de séance + sessions → en tire des **leçons
appliquées** (améliorer skills / mémoire / Vault). Git push/config **exclus**
(deuxième modèle = humain).
- **Où** : cron `d5b1eeba86c2`, rapport dans CRONS.md.
- **Rôle** : c'est le mécanisme qui fait que l'écosystème s'améliore tout seul.

### 5. HTML-as-Plan — le FORMAT de lecture/validation
Bascule « HTML is the new Markdown » appliquée à La Méthode : le plan (PRD,
guidelines, jalons, AC) rendu en **artefact HTML interactif** (5 onglets, AC
cochables persistées, KPI vivants) que Raf lit et valide — au lieu d'un `.md`
de 1000 lignes qu'on ne relit pas.
- **Où** : sandbox `~/projets/sandbox-html-plan/` → `plan.html` = **template**,
  `plan/` = instances par projet.
- **Règle** : `.md` reste la **source d'écriture** (léger, versionnable), `.html`
  est la **couche de lecture** (interactif, engageant). On n'abandonne pas le MD.
- **Étendre** : générer `plan.html` depuis le plan final d'un vrai projet
  (Astroprisma, gestion-budget) pour test grandeur nature.

### 6. Rust — le LANGAGE des outils solides
Backend performant et sûr pour les outils transverses du BMAX.
- **Où** : `hermes-dashboard` (axum, port 8091), `langgraph-atelier`.
- **Rôle** : les outils de pilotage/infra en Rust plutôt qu'en script fragile.
- **Pas un dogme** : les apps métier restent en Node/TS (front) — Rust pour les
  couches d'infra qui doivent tenir.

---

## Le flux complet (de l'idée à la v1)

1. **Concevoir** — La Méthode : brainstorming → PRD → guidelines → plan jalons
   (Hermès seul, étape 1 du pipeline).
2. **Valider le format** — rendre le plan en `plan.html` (HTML-as-Plan) pour que
   Raf le lise et le valide vraiment.
3. **Exécuter** — Cabinet : découpe en tickets (AC par ticket), TDD, implémentation
   par les cerveaux (Freebuff/Kimi), review.
4. **Vérifier** — QA agent loop : repro → test rouge → fix → suite verte → PR →
   clapet Hermès. **Aucun merge sans clapet + feu vert humain.**
5. **Clôturer** — revue de jalon, tests automatisés obligatoires, v1 seulement
   quand tous les jalons sont cochés sans régression.
6. **Apprendre** — Boucle rétro 21h : les leçons de la session remontent dans
   skills/mémoire/vault.

**Où ça se déroule** : Sandbox (= bac à essai, ~/projets/sandbox-* ) pour
expérimenter sans risque ; dépôts projet (~/projets/NOM) pour le réel ; chaque
nouveau projet suit la fiche op (sections 11-12 : profile, salon p-NOM).

---

## Les règles transversales (non négociables)

- **Clapet Hermès** : tout verdict/merge passe par une vérification Hermès sur le
  code réel, jamais le seul mot d'un agent.
- **Feu vert humain (Raf) pour** : commit/push, merge, modifications Vault/salons/
  secrets, déploiement.
- **Vérifier sur disque / en exécutant**, jamais le texte de réponse.
- **Validation finale = humain**, jamais uniquement l'IA.
- **Cerveaux du cabinet** : suivent le modèle global courant, jamais épinglés.
- **Quota** : GLM 5.3 = gros morceau seulement, début de session.
- **Un seul Vault**, racine = index partagé, outil IA = locataire `20.0x`.

---

## Comment s'en servir à l'avenir (guide rapide)

| Besoin | Utilise |
|---|---|
| Cadrer un nouveau projet | La Méthode (skill) + fiche op section 11 |
| Décider qui code | Cabinet agentique (skill) |
| Traiter des bugs | qa-loop (skill) — issue `bug` → boucle auto → PR + clapet |
| Auditer un ensemble | qa-loop audit mode / team lead |
| Faire le bilan d'une session | Boucle rétro (cron 21h, auto) |
| Rendre un plan lisible/validable | Template `plan.html` (sandbox) |
| Expérimenter sans risque | Nouvelle sandbox `~/projets/sandbox-NOM` |
| Construire un outil d'infra | Rust (modèle `hermes-dashboard`) |
| Mémoriser / retrouver | Vault (00 Index → sujets détaillés) |

---

## État actuel (08/09)

- **Sandbox HTML-as-Plan** : opérationnelle — template `plan.html` + app test
  `compute-cost-estimator` codée en TDD (9/9 tests verts), **validée par le QA
  agent** (1 bug trouvé + corrigé : entrée non numérique → `$NaN` silencieux).
- **QA loop** : en production sur Ulysse ; pattern prouvé réutilisable sur tout
  dépôt avec suite de tests.
- **La Méthode + Cabinet + Rust** : documentés, en place.

**Prochaines pistes (à demander à Raf)** : généraliser `plan.html` sur un vrai
projet, git-init la sandbox, décider s'il faut un dossier Vault `20.0x` dédié
au QA loop.

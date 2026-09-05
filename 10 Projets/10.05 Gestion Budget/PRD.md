# Gestion Budget — PRD (Product Requirements Document)

Date de conception : brainstorming + PRD, Sept 2026. Projet autoentrepreneur Raf.

## 1. Contexte

Raf est autoentrepreneur. Pour lui, **le chiffre d'affaires = son salaire**. Il
manque de **visibilité et de contrôle** sur son budget : il ne sait pas en un
coup d'œil si son activité couvre ses charges, ni combien il peut se verser.

## 2. Objectif (la boussole)

> En un coup d'œil, l'app dit à Raf si son activité couvre ses charges fixes
> et combien il peut se verser (épargne / salaire perso).

Toute fonctionnalité doit servir cet objectif.

## 3. Utilisateur

- **Raf seul** (single-user). Pas de gestion de comptes, pas de multi-utilisateur.
- Usage **mobile-first** (souvent sur téléphone).

## 4. Support & hébergement

- **PWA** mobile-first : React + Vite + TypeScript.
- Hébergé sur **BMAX** (homelab), accessible depuis le téléphone.
- **Réseau suffit** : pas de mode hors-ligne.

## 5. Sécurité

- **PIN simple** à l'ouverture de l'app.
- **Backup automatique sur BMAX** (données financières sensibles).

## 6. Fonctionnalités

### Tableau de bord (coup d'œil)
- **2 sections max** (design minimal, mobile-first).
- Vue d'ensemble instantanée.

### CA (Chiffre d'affaires)
- **CA minimum = somme automatique des charges fixes.** Raf saisit les charges
  fixes (loyer, maison, CFE, crédit) et l'app additionne pour définir le CA minimum.
- **Indicateur « objectif atteint »** : l'app signale quand le CA du mois couvre
  les charges / le CA minimum.
- Deux vues : **par mois** et **par jour** (cumulé).

### URSSAF
- **% paramétré par Raf** (il saisit le taux lui-même).
- **Par facture** : à chaque encaissement client, X% mis de côté.
- **Toggle optionnel** pour le mode mensuel (mettre de côté sur le CA du mois
  au lieu de par facture).
- **Suivi temps réel** : compteur « tu as mis X € de côté pour l'URSSAF ».

### Catégories (2 distinctes)
- **Achats produits** — dépenses liées à l'activité (achat, stock, matière).
- **Charges** — frais de fonctionnement.

### Factures clients
- Montant + date + **état** (émise / payée / en retard).

### Charges fixes
- **Loyer atelier**
- **Charge fixe maison**
- **CFE** (cotisation foncière des entreprises)
- **Crédit** (prêt à rembourser) : N mensualités, **reste à payer**, **date de fin**.

### Épargne
- **Juste suivie** (montant, pas d'opérations complexes).

## 7. Contraintes

- Single-user, mobile-first, PWA sur BMAX, réseau requis, PIN, backup BMAX.
- Données financières sensibles.

## 8. Risques

- **Données sensibles** → PIN + backup auto BMAX.
- **Perte de données** → sauvegarde quotidienne sur BMAX.
- **Complexité URSSAF** (taux, toggle mois/facture) → garder paramétrable simple.

## 9. Hors périmètre (v1)

- Multi-utilisateurs / comptes.
- Mode hors-ligne.
- Opérations d'épargne (dépôts/retraits) — juste le suivi.
- Saisie manuelle du CA minimum (calculé automatiquement depuis les charges fixes).
- Impôts autres que l'URSSAF.

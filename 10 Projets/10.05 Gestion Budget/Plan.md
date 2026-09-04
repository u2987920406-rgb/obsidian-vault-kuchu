# Gestion Budget — Plan (étape 4)

Date : Sept 2026. Projet autoentrepreneur Raf. Découpage en tâches ordonnées avec critères d'acceptation. MVP identifié.

## MVP (Minimum Viable Product) — ce qui fait que l'app te sert vraiment

Le MVP = **le tableau de bord coup d'œil qui répond au stress (visibilité + contrôle)**. Sans lui, l'app ne sert à rien.

**MVP inclus :**
- Tableau de bord 2 sections (CA + charges/épargne)
- CA minimum saisi à la main + indicateur "objectif atteint" (vues mois/jour)
- URSSAF : % paramétré, par facture + toggle mensuel, suivi temps réel
- Factures clients : montant + date + état (émise/payée/en retard)
- Achats produits + charges : 2 catégories distinctes
- Charges fixes : loyer atelier, charge maison, CFE, crédit (reste + fin)
- Épargne : suivi
- PIN 4 chiffres
- Backup auto BMAX

**Hors MVP (v2) :**
- Graphiques camembert/barres (design riche) — ajoutés après le MVP fonctionnel
- Export/rapports

## Phases

### Phase 0 — Socle (setup)
- [ ] T0.1 Créer le monorepo `~/projets/gestion-budget` (front React + backend + config BMAX)
- [ ] T0.2 Initialiser git (branche main), premier commit
- [ ] T0.3 Configurer le backend (API + SQLite) sur BMAX
- [ ] T0.4 Configurer le PIN (stockage côté serveur)
- **AC** : le repo existe, git initialisé, le backend répond sur BMAX, le PIN se configure.

### Phase 1 — Données (backend + base)
- [ ] T1.1 Modèle SQLite : factures, achats, charges, charges fixes, crédit, épargne, config (CA min, % URSSAF)
- [ ] T1.2 API CRUD : factures, achats, charges, charges fixes, crédit, épargne
- [ ] T1.3 API config : CA minimum, % URSSAF
- [ ] T1.4 API calculs : CA du mois/jour, objectif atteint, URSSAF mise de côté, reste à couvrir
- **AC** : chaque entité se crée/lit/modifie/supprime via l'API ; les calculs retournent les bons montants (tests unitaires verts).

### Phase 2 — Front (PWA)
- [ ] T2.1 Squelette PWA React (Vite + TS), 2 sections, navigation mobile
- [ ] T2.2 Écran PIN à l'ouverture
- [ ] T2.3 Section 1 — Tableau de bord : CA (mois/jour), objectif atteint, URSSAF temps réel, épargne
- [ ] T2.4 Section 2 — Saisie : factures (montant/date/état), achats, charges, charges fixes, crédit, épargne
- [ ] T2.5 Config : CA minimum, % URSSAF, toggle mensuel
- **AC** : l'app s'ouvre sur le PIN, affiche le tableau de bord en un coup d'œil, permet de saisir toutes les entités, les calculs s'affichent correctement.

### Phase 3 — Déploiement + backup
- [ ] T3.1 Déployer la PWA sur BMAX (accessible depuis le téléphone)
- [ ] T3.2 Backup automatique quotidien de la base SQLite
- **AC** : l'app est accessible depuis le téléphone de Raf via BMAX, le backup tourne quotidiennement.

### Phase 4 — Tests + validation
- [ ] T4.1 Tests unitaires logique métier (calculs CA, URSSAF, charges) + composants clés
- [ ] T4.2 Test device Raf (téléphone) : parcours complet
- **AC** : tests verts, Raf valide le parcours sur son téléphone.

### Phase 5 — v2 (optionnel, après MVP)
- [ ] T5.1 Graphiques camembert/barres
- [ ] T5.2 Export/rapports

## Ordre d'exécution
Phase 0 → 1 → 2 → 3 → 4. La v2 (phase 5) seulement après validation du MVP par Raf.

## Critères d'acceptation globaux
- L'app répond au stress : visibilité + contrôle en un coup d'œil.
- Toutes les entités se saisissent et se suivent.
- Les calculs (CA, URSSAF, objectif) sont justes (tests verts).
- Accessible depuis le téléphone de Raf sur BMAX, protégée par PIN, sauvegardée.

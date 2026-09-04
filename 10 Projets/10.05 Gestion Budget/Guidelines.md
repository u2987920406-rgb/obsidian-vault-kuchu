# Gestion Budget — Guidelines (règles de code/archi/nommage)

Date : Sept 2026. Projet autoentrepreneur Raf. Règles validées par Raf, à ne jamais transiger.

## 1. Architecture
- **Monorepo simple** : un dossier `~/projets/gestion-budget` avec front (React) + backend (API) + config BMAX.
- PWA mobile-first (React + Vite + TS), hébergée sur BMAX, réseau suffit (pas de hors-ligne).
- Backend : API + base de données, déployé sur BMAX.

## 2. Nommage
- Composants : **PascalCase** (`FactureCard.tsx`).
- Fichiers de données : **camelCase** (`factures.ts`).
- Dossiers : **kebab-case** (`/components`, `/services`).

## 3. Test
- **Tests unitaires** sur la logique métier (calculs CA, URSSAF, charges) + composants clés.
- Le cœur de l'app = les calculs → on les protège en priorité.

## 4. Données
- **SQLite locale sur BMAX** (fichier, simple, backup facile).
- Backup automatique quotidien sur BMAX.

## 5. UI/UX
- Mobile-first, **2 sections max**.
- Design **riche avec graphiques** (camembert/barres) + gros indicateurs "objectif atteint" en priorité.
- Couleurs : vert = OK, rouge = attention.

## 6. Sécurité
- **PIN 4 chiffres** à l'ouverture, stocké côté serveur, vérifié à l'ouverture de l'app.

## 7. Git/versioning
- Branche **main** + commits réguliers.
- **Validation par Raf à chaque étape** avant merge.

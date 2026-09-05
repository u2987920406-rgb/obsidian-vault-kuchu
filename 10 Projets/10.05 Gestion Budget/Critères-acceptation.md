# Gestion Budget — Critères d'acceptation (MVP)

Date : Sept 2026. Alignés sur le PRD corrigé (CA minimum = somme auto des charges fixes). Chaque critère est testable. Le MVP n'est validé que quand TOUS les critères passent.

## Fonctionnel

### Sécurité
- [ ] CA1. L'app s'ouvre sur un écran PIN (4 chiffres).
- [ ] CA2. PIN par défaut `0000`, modifiable dans la config.

### CA & objectif
- [ ] CA3. Le CA du mois = somme des factures **payées** du mois.
- [ ] CA4. Le CA du jour = somme des factures **payées** du jour.
- [ ] CA5. Le **CA minimum = somme automatique des charges fixes** (loyer + maison + CFE + crédit). Modifier une charge fixe met à jour l'objectif.
- [ ] CA6. Badge « Objectif atteint » ✓ si CA mois ≥ CA minimum, sinon « Reste X € ».
- [ ] CA7. Vue par mois ET par jour du CA.

### URSSAF
- [ ] CA8. % URSSAF paramétrable dans la config.
- [ ] CA9. Compteur temps réel « mis de côté ce mois » = somme des (montant × % ) des factures payées du mois.
- [ ] CA10. % individuel sur une facture possible (urssaf_pct).

### Catégories
- [ ] CA11. Deux catégories distinctes : **Achats produits** et **Charges**.
- [ ] CA12. Poubelle 🗑️ sur **toutes** les lignes (factures, achats, charges, charges fixes, épargne).

### Factures
- [ ] CA13. Saisie montant + date + client + état (émise / payée / en retard).
- [ ] CA14. Changement d'état en un clic (select sur la ligne).

### Charges fixes
- [ ] CA15. Saisie libellé + montant + type (fixe / crédit).
- [ ] CA16. **Crédit** : mensualités totales, restantes, date de fin.
- [ ] CA17. Section **Crédit** visible dans le tableau de bord : reste à payer, mensualité, restantes, fin.

### Épargne
- [ ] CA18. Épargne suivie (total affiché dans le dashboard).

## Technique

- [ ] CA19. Backend : les calculs dashboard sont testés par tests unitaires (vert).
- [ ] CA20. Frontend : build de production sans erreur TypeScript.
- [ ] CA21. Service systemd actif + backup quotidien (02h00) fonctionnel.
- [ ] CA22. Accès mobile via BMAX (tailnet) OK.

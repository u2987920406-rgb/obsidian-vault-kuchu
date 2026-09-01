# Hermès — Décharge mémoire LEAN → Vault

Règle de gestion de la mémoire persistante de Hermès (l'outil `memory`, injecté
chaque tour, plafond ~2200 chars). Validée avec Raf le 2026-09-01.

## Principe
La mémoire LEAN ne doit JAMAIS perdre un fait durable. Quand elle sature, on
déverse d'abord dans le Vault, puis on consolide. Règle d'or : *« jamais
supprimer de la mémoire LEAN un fait durable qui n'est pas déjà dans le Vault. »*

## Déclencheur hybride
1. **En continu (proactif)** : dès qu'un fait durable est appris (décision,
   règle, modif système, skill créée), le déverser dans le Vault au moment où
   il arrive — pas besoin d'attendre la saturation.
2. **À la saturation (réactif)** : quand la mémoire LEAN approche de ~2K, faire
   une passe de consolidation : vérifier que tout le durable est au Vault,
   puis raccourcir/fusionner/supprimer en sécurité.

## Ce qui est DURABLE (→ Vault)
1. **Décisions** — choix + raison (ex. « Ulysse reste en Python, pas Rust »)
2. **Règles / préférences de travail** — façon de fonctionner (ex. « discuter
   avant d'agir », « #général = discussion »)
3. **Méthodes / procédures** — réutilisables (ex. boucle Hermes⇄GitHub⇄Claude)
4. **Config / modifs système** — changements d'infra BMAX (ex. décommissionnement
   Obsidian, compression proactive)
5. **Projets** — avancement, architecture, décisions
6. **Skills créées / modifiées** — et pourquoi

## Ce qui n'est PAS durable (reste en session)
- Résultats de recherche ponctuels (gagnant d'un match, champion)
- Questions / réponses sans portée future
- Détails de conversation sans conséquence
- État transitoire (issue en cours, bug en train d'être fixé)

## Où déverser
- **Faits transverses** → `30 Journal/AAAA-MM-JJ.md` (journal du jour)
- **Faits projet** → note projet dédiée (`10 Projets/<projet>/`)
- **Faits système** → fiche opérationnelle (`11 Ressources/raf-bmax — Fiche opérationnelle.md`)

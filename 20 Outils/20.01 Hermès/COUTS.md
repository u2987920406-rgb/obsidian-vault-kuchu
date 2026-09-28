# Hermès — Coûts & comportement d'usage (TokenTrack)

> Comment on dépense les tokens, pourquoi, et ce qu'on corrige. Source de vérité :
> l'app **TokenTrack** (`~/projets/tokentrack`, https://raf-bmax.tail14baaa.ts.net:8464/).
> Elle lit `~/.hermes/state.db` en **lecture seule** — consulter l'app ou son API
> ne consomme aucun token.

## Les chiffres qui comptent (mesurés le 2026-09-28)

- **Fil neuf ≈ 12,3 k tokens d'entrée par appel. Fil de 400+ messages ≈ 101 k.** Le
  même message coûte **8×** plus cher dans un fil long. C'est le premier poste de
  dépense, loin devant le choix du modèle.
- **Le cache Ollama n'est pas un détail** : `cache_read` est facturé une fraction de
  l'input frais. Un ratio « cache / input » de 1300 % est un **bon** signe (le préfixe
  est réutilisé) ; un ratio sous ~100 % sur un fil long veut dire que le cache est
  froid — souvent parce qu'on a changé de modèle en cours de fil.
- **Les tâches de service ne doivent jamais être payantes** : revue mémoire/skills en
  arrière-plan, compression, génération de titres, vision. Routées vers Xiaomi / Nous /
  NVIDIA Build (`auxiliary.<tâche>.provider` + `fallback_chain`).
- **Le modèle premium se justifie pour raisonner, pas pour du chat** : glm-5.3 coûte
  ~9× un flash pour un gain invisible sur une conversation courante. L'épinglage par
  salon (`channel_overrides`) est le bon levier.

## Les gestes qui font baisser la facture

| Geste | Quand | Effet |
|---|---|---|
| `/new` | sujet fini, livrable rendu, ou fil > ~100 messages | repart à ~12 k/appel |
| `/compress here 20` | le sujet continue mais le fil est lourd | garde la fin, résume le reste |
| `/usage` | pour voir la taille du contexte courant | décide entre les deux précédents |
| ne pas changer de modèle en cours de fil | — | conserve le cache d'input |

Depuis le 2026-09-28, `compression.idle_compact_after_seconds = 3600` compacte
automatiquement un fil repris après plus d'une heure d'inactivité : le préfixe
périmé n'est plus relu à chaque tour.

## Ce qui alerte tout seul

- **`Veille contexte 150k`** (`2c3e8ddbc5f2`, toutes les 10 min, **sans agent** donc
  coût nul) : poste un message **dans le fil concerné** dès que le contexte estimé
  dépasse 150 k, puis re-alerte tous les 50 k. Silence total sinon.
- **`Récap TokenTrack (hebdo)`** (`551cf500dc4e`, lundi 08h00, #rapports) : score de
  sobriété, variation, top 2 des comportements coûteux, une action par semaine.
- **TokenTrack** lui-même : score de sobriété /100 = part **non évitable** du coût
  réel. Un score bas veut dire que l'essentiel du budget part dans des habitudes
  corrigeables.

## Pièges de mesure (à ne pas refaire)

- **Ne pas additionner les constats** : ils se recouvrent (un fil long sur un modèle
  premium paie deux postes). Un contrefactuel par session, puis répartition au
  prorata — sinon on annonce plus d'économies que la facture totale.
- **Borner toute estimation par le coût réellement dépensé.**
- **`messages.token_count` est vide** dans cette version d'Hermès : ne jamais s'en
  servir. Les totaux vivent dans `sessions` et `session_model_usage`.
- Un **texte dans un `<svg viewBox>`** est mis à l'échelle : les libellés de graphique
  vont en HTML, sinon 16 px affichés ≈ 8 px réels.

## Historique court

- 30/08 → 27/09 : ≈ 501 $ de crédits Ollama (pic à 96 $ le 05/09). Run-rate ramené à
  ~17 $/semaine après les réglages du 28/09.

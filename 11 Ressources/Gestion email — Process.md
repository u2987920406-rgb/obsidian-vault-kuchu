# Gestion email — Process & technique

Process de tri / purge d'une boîte Gmail volumineuse, outillé via Himalaya CLI (IMAP). Note technique et meta — aucune donnée de mail ici.

## Objectif
Trier, structurer en labels, purger l'historique et se désabonner des newsletters d'une boîte Gmail.

## Outils
- **Himalaya CLI** (IMAP) — analyse fine, tri, purge par lots. Config : `~/.config/himalaya/config.toml` (compte + mot de passe d'app).
- **Gmail natif** — purge massive / gestion des labels côté web.

## Workflow en phases
1. **Cadrage** — horizon temporel, stratégie suppression, architecture labels, sécurité, outil.
2. **Reconnaissance** — `imap status` pour le volume réel, dump des enveloppes, inventaire expéditeurs.
3. **Architecture** — créer les labels (`imap create`).
4. **Tri** — déplacer les emails récents vers leurs labels (`message move`).
5. **Purge** — déplacer l'historique vers la Corbeille (filet 30 j).
6. **Désinscription** — liens `List-Unsubscribe` (one-click via curl) + marquer spam le reste.
7. **Nettoyage** — supprimer les dossiers vides, vider la corbeille après 30 j.

## Commandes clés Himalaya
```bash
# Volume réel d'une boîte (TOUJOURS vérifier avant de planifier)
himalaya imap status "[Gmail]/Tous les messages"

# Lister les boîtes / labels
himalaya mailbox list

# Dump des enveloppes (JSON, paginé)
himalaya envelope list --mailbox "Inbox" --json --page-size 1000 --page 1

# Créer un label
himalaya imap create "Finance"

# Déplacer des messages (par UID)
himalaya message move --from "Inbox" --to "Finance" <uid1> <uid2> ...

# Lire le raw d'un message (headers List-Unsubscribe)
himalaya message read --mailbox "Newsletters" --raw <uid>

# Supprimer un dossier vide
himalaya imap delete "Archive"
```

## Meta — leçons apprises
- **Le volume réel peut être très différent de l'estimation.** 65k estimé vs 8.9k réel. Toujours confirmer avec `imap status` avant de construire le plan.
- **Les UID IMAP changent quand on déplace des messages entre boîtes.** Re-lister les enveloppes après chaque déplacement avant de cibler.
- **Les liens `List-Unsubscribe` one-click s'appellent via un simple GET (curl).** Mais les tokens expirent vite — prendre les emails les plus récents de l'expéditeur.
- **Certains expéditeurs n'ont pas de `List-Unsubscribe`** (connexion au compte requise, ex. TradingView). Stratégie de repli : marquer spam.
- **Marquer spam en masse = déplacer vers `[Gmail]/Spam`.** Gmail apprend et route les futurs emails en spam.
- **Les gros lots (200+) peuvent échouer partiellement.** Vérifier l'état réel des boîtes après, retenter les restants en plus petits lots (50).
- **La Corbeille Gmail = filet de sécurité 30 jours.** Purger en y déplaçant, vider après 30 j.
- **Supprimer les dossiers vides** (vestiges d'anciens clients mail) pour une boîte propre.
- **Ne pas garder de traces de mail** — les dumps et plans sont jetables ; ne conserver que la technique et la meta.

## Liens
- [[00 Index]]

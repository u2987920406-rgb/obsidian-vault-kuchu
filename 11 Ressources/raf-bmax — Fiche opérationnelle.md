# raf-bmax — Fiche opérationnelle

Serveur homelab · Ubuntu 26.04 · Ryzen 7 8745HS · 16 Go

**Nom tailnet :** raf-bmax.tail14baaa.ts.net
**IP tailnet :** 100.101.17.46

*Les mots de passe ne figurent pas ici volontairement : ils sont dans le
gestionnaire de mots de passe.*

**Identifiants (noms d'utilisateur uniquement) :**
- Uptime Kuma → `raf_bmax`
- Dozzle → `raf`
- Dashboard Hermes → `raf`
- Portainer → défini au premier lancement

---

## 0. Les trois canaux d'accès

**Le réflexe par défaut, c'est Discord — pas SSH.** Hermes tourne sur la
machine et a accès au terminal. Le SSH est le filet de sécurité pour le
jour où Hermes ne répond plus.

1. **Discord** — usage normal, depuis n'importe quel appareil.
2. **SSH** — uniquement si Hermes est muet. App Tailscale du Pixel, ou Termux.
3. **Écran + clavier** — si même le SSH échoue. Console texte, identifiant `raf`.

---

## 1. Se connecter en SSH (secours)

    ssh raf@raf-bmax.tail14baaa.ts.net

Aucun port ouvert sur Internet. Authentification par identité Tailscale.

---

## 2. Passer en headless

    sudo systemctl set-default multi-user.target
    sudo systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
    sudo systemctl reboot -i

Le masquage de la veille est **indispensable** : sans lui la machine peut
s'endormir et devenir injoignable.

**Pourquoi `reboot -i`** — `sudo reboot` échoue quand un « inhibiteur »
bloque l'extinction (session graphique, mise à jour en cours). Le `-i`
(*ignore-inhibitors*) passe outre.

---

## 3. Revenir en mode graphique

Aucun paquet désinstallé, GNOME est toujours sur le disque.

    sudo systemctl set-default graphical.target
    sudo systemctl reboot -i

---

## 4. Vérifier que tout tourne

**Méthode normale — depuis Discord :**

> Vérifie que tout tourne : docker, tailscaled, les conteneurs de la stack,
> et ton propre service.

**Méthode de secours — en SSH :**

    systemctl is-active docker tailscaled
    systemctl --user is-active hermes-gateway hermes-dashboard hermes-healthcheck.timer
    docker ps
    tailscale status

Attendu : 4 conteneurs de la stack (portainer, uptime-kuma, dozzle,
docker-socket-proxy) plus les sandboxes `hermes-*` éventuels.

---

## 5. Interfaces web

- **Portainer** — https://raf-bmax.tail14baaa.ts.net/
- **Uptime Kuma** — https://raf-bmax.tail14baaa.ts.net:8443/
- **Obsidian** — https://raf-bmax.tail14baaa.ts.net:10000/ (auth `raf`)

**Non exposés** (les 3 ports HTTPS de Tailscale Serve sont pris) — tunnel SSH :

    ssh -L 9119:localhost:9119 raf@raf-bmax.tail14baaa.ts.net   # dashboard Hermes
    ssh -L 8080:localhost:8080 raf@raf-bmax.tail14baaa.ts.net   # Dozzle

Puis http://localhost:9119 ou http://localhost:8080

---

## 6. Surveillance (Uptime Kuma)

**Quatre sondes actives :**

| Sonde | Type | Ce qu'elle vérifie |
|---|---|---|
| Portainer | HTTP | `http://portainer:9000` |
| Dozzle | HTTP | `http://dozzle:8080/healthcheck` |
| Internet | Ping | `1.1.1.1` |
| Hermes Gateway | Push | service actif **ET** Discord connecté |

**Les alertes arrivent dans Discord** par webhook (notification « Discord »,
définie par défaut). Cette chaîne est **indépendante du bot Hermes** : si
Hermes perd Discord, c'est Kuma qui prévient.

**La sonde Hermes** est un minuteur systemd qui appelle Kuma toutes les
minutes, uniquement si le gateway est sain :

    systemctl --user status hermes-healthcheck.timer
    /home/raf/docker/stack/hermes-healthcheck.sh    # test manuel

Fichiers : `~/docker/stack/hermes-healthcheck.sh`, URL Push dans
`~/.hermes/kuma-push-url`.

**Limite connue :** si Internet coupe, la sonde Internet l'enregistre dans
l'historique mais l'alerte ne part pas — elle passerait par Internet.

---

## 7. Vault Obsidian

Le vault vit sur le BMAX dans `~/vault` — c'est le clone du dépôt GitHub
**obsidian-vault-kuchu** (branche `master`, public). **Obsidian tourne dans
un conteneur**, servi par navigateur : rien à installer sur tes appareils.
Hermes lit et écrit le même dossier.

**Accès :** https://raf-bmax.tail14baaa.ts.net:10000/ (auth HTTP `raf`)
Fonctionne depuis le téléphone, un simple lien suffit.
Dans Obsidian, ouvrir le vault `/config/vault`.

**Structure (tes conventions) :** `00 Index.md` à la racine (index partagé,
à lire en premier), `10 Projets/`, `11 Ressources/`, `20 Outils/` (un
namespace par outil IA, Hermès en `20.01`), `30 Journal/` (un fichier par
date).

**Règle d'or :** un seul vault. Ajouter un outil IA = ajouter un `20.0x`,
jamais recréer un vault séparé.

**Git :** le clone a `origin` vers GitHub, identité git configurée. Le
`push` nécessitera un accès en écriture (le clone HTTPS public ne le permet
pas) — à mettre en place quand tu voudras l'automatiser.

**Sécurité :** l'image Obsidian embarque un terminal avec `sudo` sans mot de
passe — neutralisé par `DISABLE_SUDO=true`, interface protégée par
`CUSTOM_USER`/`PASSWORD`. Ne pas retirer ces réglages.

**Depuis Discord :**

> ajoute une note dans le vault sur ...
> lis 00 Index.md et dis-moi où en est Ulysse

La convention est écrite dans `~/.hermes/SOUL.md`.

---

## 8. Stack Docker

Le plus simple est de le demander à Hermes dans Discord. En SSH :

    cd /home/raf/docker/stack
    docker compose ps
    docker compose up -d
    docker compose pull && docker compose up -d   # mise à jour

**Après toute recréation d'un conteneur**, son IP change et Uptime Kuma
garde l'ancienne en cache — moniteurs au rouge à tort :

    docker restart uptime-kuma

---

## 9. Hermes — quotidien

    systemctl --user restart hermes-gateway.service
    journalctl --user -u hermes-gateway -f
    hermes cron list
    hermes profile list
    hermes project list

---

## 10. Hermes — configuration sensible

Attendu : `local` et `manual`.

    hermes config get terminal.backend
    hermes config get approvals.mode

Simuler un verdict sans exécuter :

    hermes approvals test "systemctl restart docker"

Proposer une liste blanche depuis l'historique (rien n'est appliqué sans
`--apply`) :

    hermes approvals suggest

Revenir au mode conteneur isolé (Hermes ne peut plus administrer la machine) :

    hermes config set terminal.backend docker && systemctl --user restart hermes-gateway.service

---

## 11. Créer un nouveau projet

**1. Profil cloisonné** (si le projet touche du code ou du contenu tiers) :

    hermes profile create NOM --clone && hermes profile alias NOM
    HERMES_PROFILE=NOM hermes config set terminal.backend docker

**2. Déclarer le projet :**

    hermes project create "NOM LISIBLE" ~/projets/NOM --use

**3. Créer le salon Discord à la main** — Hermes n'a pas `MANAGE_CHANNELS`.
Le nommer `p-NOM`.

**4. Rendre le salon libre** — récupérer son ID (clic droit → Copier
l'identifiant du salon), puis dans `~/.hermes/.env`, ligne
`DISCORD_FREE_RESPONSE_CHANNELS=`, ajouter l'ID séparé par une virgule :

    systemctl --user restart hermes-gateway.service

**5. Board de tâches (optionnel) :**

    hermes kanban boards create NOM

*Les étapes 1, 2 et 5 peuvent être confiées à Hermes depuis Discord.*

---

## 12. Passer le contexte d'un salon à l'autre

Un salon = une session. Hermes n'a **aucun** souvenir d'un salon à l'autre.

Avant de changer de salon :

> Résume ce qu'on a décidé dans ~/projets/NOM/BRIEF.md, et mémorise les
> points clés.

Dans le nouveau salon :

> lis ~/projets/NOM/BRIEF.md, on continue ici.

---

## 13. Cadence (tâches planifiées)

    hermes cron create "0 8 * * *" "Résume l'état des conteneurs et signale toute anomalie" --name rapport-matin --deliver discord
    hermes cron list

Résultats dans **#hermes-rapports**. Sécurité : `cron_mode: deny` — une
tâche nocturne déclenchant une commande dangereuse est **refusée**.

---

## 14. Approbations Discord

- **Allow Once** — réflexe par défaut, cette exécution seulement.
- **Allow Session** — tâche répétitive en cours, jusqu'à fin de session.
- **Always Allow** — écrit dans la liste blanche définitivement.
  **Commandes de lecture uniquement.**
- **Deny** — au moindre doute. Sans réponse en 5 min = refus automatique.

---

## 15. En cas de blocage

**Hermes ne répond plus sur Discord** — passer en SSH :

    systemctl --user status hermes-gateway --no-pager
    python3 -c "import json;print(json.load(open('/home/raf/.hermes/gateway_state.json'))['platforms'])"

**Hermes ne répond pas dans un salon précis** — vérifier que l'ID du salon
est dans `DISCORD_FREE_RESPONSE_CHANNELS`. Attention : mentionner
`@Hermes` en bleu vise le **rôle Discord**, pas le compte du bot.

**Plus d'accès SSH** — écran + clavier sur le BMAX, puis section 3.

**Le redémarrage refuse de se lancer** :

    systemd-inhibit --list
    sudo systemctl reboot -i

---

## 16. Chemins utiles

| Quoi | Où |
|---|---|
| Stack Docker | `/home/raf/docker/stack/` |
| Config Hermes | `/home/raf/.hermes/config.yaml` |
| Secrets Hermes | `/home/raf/.hermes/.env` (mode 600) |
| Services systemd utilisateur | `/home/raf/.config/systemd/user/` |
| Projets | `/home/raf/projets/` |
| Cette fiche | `/home/raf/FICHE.md` |

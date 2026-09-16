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
    systemctl --user is-active hermes-gateway hermes-dashboard hermes-healthcheck.timer gestion-budget.service
    docker ps
    tailscale status

Attendu : 4 conteneurs de la stack (portainer, uptime-kuma, dozzle,
docker-socket-proxy) plus les sandboxes `hermes-*` éventuels.

---

## 5. Interfaces web

- **Portainer** — https://raf-bmax.tail14baaa.ts.net/
- **Uptime Kuma** — https://raf-bmax.tail14baaa.ts.net:8443/
- **Bureau virtuel (Steam / Blender)** — https://raf-bmax.tail14baaa.ts.net:8445/desktop.html
- **Dashboard Hermes** — https://raf-bmax.tail14baaa.ts.net:10000/ (auth BASIC)
- **Gestion Budget** (PWA) — http://100.101.17.46:8093/ (port 8093, service systemd user `gestion-budget.service`, backup quotidien 02h00 `gestion-budget-backup.timer`)
- **DeepSeek Harness (`dsh`)** — https://raf-bmax.tail14baaa.ts.net:8447/ (service systemd user `dsh.service`, port 3080). **L'URL exige un jeton** qui change à chaque redémarrage : récupérer le lien courant avec `dsh-link`. Détail : [[20.02 DeepSeek Harness]].

**Attention aux ports Tailscale Serve** — `~/docker/stack/serve.sh` les réinitialise
tous (`tailscale serve reset`) puis réapplique 443 / 8443 / 8445 / 8447 / 10000. Toute
publication faite à la main (ex. le bureau virtuel sur 8445) est **perdue** à
chaque exécution de `serve.sh`. Il faut y ajouter la ligne correspondante.

**Non exposés** — tunnel SSH :

    ssh -L 9119:localhost:9119 raf@raf-bmax.tail14baaa.ts.net   # dashboard Hermes
    ssh -L 8080:localhost:8080 raf@raf-bmax.tail14baaa.ts.net   # Dozzle

Puis http://localhost:9119 ou http://localhost:8080

---

## 5bis. Bureau virtuel (apps graphiques en headless)

La machine est headless (aucun écran branché) : les applis GUI lancées depuis
SSH **tournent mais n'affichent aucune fenêtre**. Solution en place : un écran
X11 virtuel diffusé dans le navigateur.

**Accès :** https://raf-bmax.tail14baaa.ts.net:8445/desktop.html

**Quatre services systemd user** (actifs, `enabled`, survivent au reboot grâce
au `linger` activé) :

    systemctl --user status virtual-desktop.service          # écran X :5 (Xtigervnc)
    systemctl --user status virtual-desktop-wm.service       # gestionnaire de fenêtres (jwm)
    systemctl --user status virtual-desktop-novnc.service    # noVNC (port 6085)
    systemctl --user status steam-virtual.service            # client Steam

Tout est dans `~/projets/remote-desktop/` (binaires extraits sans root dans
`prefix/`, aucun paquet système installé pour VNC/noVNC).

**Lancer Blender :**

    ~/projets/remote-desktop/blender.sh              # version Steam (5.2.1 LTS)
    BLENDER=~/blender/blender ~/projets/remote-desktop/blender.sh   # standalone (5.2.0)

En mode graphique (fenêtre visible) ou `--background` (scripts Python, rendu).
Steam → bibliothèque → Blender fonctionne aussi.

**Piège n°1 — jamais hériter de Wayland.** Les variables `WAYLAND_DISPLAY` et
`XDG_SESSION_TYPE=wayland` de la session SSH font que l'appli tente Wayland,
échoue en silence et n'affiche rien. Il faut `WAYLAND_DISPLAY=` (vide),
`XDG_SESSION_TYPE=x11`, `XAUTHORITY=` et `--gpu-backend opengl`.

**Piège n°2 — ordonnancement systemd.** Un service qui a à la fois
`After=default.target` et `WantedBy=default.target` crée un *ordering cycle* :
systemd supprime le job au boot et le service ne démarre jamais. Symptôme :
l'appli marche à la main mais pas après reboot.

**Piège n°3 — Xauthority.** En SSH, `~/.Xauthority` n'existe pas ; le vrai
fichier est `/run/user/1000/.mutter-Xwaylandauth.*` (cf. `systemctl --user
show-environment`).

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
**obsidian-vault-kuchu** (branche `master`). Hermes lit et écrit directement
ce dossier en fichiers, sans passer par une app.

**Consultation visuelle :** Obsidian tourne en **snap natif** sur bmax
(`obsidianmd`, installé le 2026-08-31), utilisé seulement en session desktop
ponctuelle (1-2x/mois). Plus de conteneur, plus d'accès distant par
navigateur — l'ancien setup Docker (port 10000) a été décommissionné.
L'app Obsidian sur Android n'est reliée à aucun vault.

**Structure (tes conventions) :** `00 Index.md` à la racine (index partagé,
à lire en premier), `10 Projets/`, `11 Ressources/`, `20 Outils/` (un
namespace par outil IA, Hermès en `20.01`), `30 Journal/` (un fichier par
date).

**Règle d'or :** un seul vault. Ajouter un outil IA = ajouter un `20.0x`,
jamais recréer un vault séparé.

**Git :** `origin` pointe vers GitHub en SSH (`git@github-vault:...`),
identité git configurée, push fonctionnel.

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

**Machine totalement injoignable (gel noyau)** — voir la section 17 : c'est
un cas à part, qui ne se règle pas en SSH puisque plus rien ne tourne.

---

## 16. Chemins utiles

| Quoi | Où |
|---|---|
| Stack Docker | `/home/raf/docker/stack/` |
| Config Hermes | `/home/raf/.hermes/config.yaml` |
| Secrets Hermes | `/home/raf/.hermes/.env` (mode 600) |
| Services systemd utilisateur | `/home/raf/.config/systemd/user/` |
| Projets | `/home/raf/projets/` |
| DeepSeek Harness (`dsh`) | `/home/raf/projets/deepseek-harness` — données : `.dsh-home/`, clé : `/home/raf/.config/dsh/dsh.env` (600), lien UI : `dsh-link` |
| Cette fiche | `/home/raf/FICHE.md` |

---

## 17. Reprise après gel noyau (à distance)

### Ce qui s'est passé le 2026-09-15

Dernière ligne écrite dans le journal : **11:59:02**. Puis ~5 h de silence
total, jusqu'à un hard reboot manuel à 16:53.

**Ce n'était pas une panne Tailscale.** Tout le noyau a gelé. Une fois le
noyau figé, plus rien ne tourne : ni Tailscale, ni SSH, ni systemd, ni
Hermes, ni Docker. C'est pour ça qu'il n'y avait aucun moyen d'entrer.

**Signes relevés dans le journal :**

- aucune séquence d'arrêt (pas de SIGTERM, pas de « Journal stopped ») —
  contrairement aux arrêts propres du 30/08
- aucun vmcore dans `/var/crash` alors que kdump est actif : le noyau n'a
  même pas eu le temps de paniquer
- `workqueue: dm_irq_work_func [amdgpu] hogged CPU for >10000us` (répété les
  31/08) et `REG_WAIT timeout ... optc314_disable_crtc line:145` — signature
  classique du pilote `amdgpu` sur iGPU Radeon 780M

**Suspect principal : le pilote `amdgpu`.** Sans dump noyau, c'est un
suspect étayé, pas une preuve.

**Écarté :** RAM (60 % libre à 11:50), disque (17 %), veille (masquée), OOM
(les kills du journal datent du 07/09 et visaient Chrome).

### Le piège du monitoring actuel

Uptime Kuma **tourne sur le BMAX**. Machine gelée = Kuma gelé = aucune alerte.
La sonde `hermes-healthcheck.timer` a le même angle mort : elle est *détecteur*,
pas *acteur*, et elle tourne sur la machine qu'elle surveille.

**Règle : pour savoir qu'une machine est morte, la surveillance doit être
hors de cette machine.**

### Les trois protections (dans l'ordre d'efficacité)

**Étage 1 — watchdog matériel (automatique, ne dépend de rien) — ✅ PROUVÉ**

Le chipset AMD FCH a un timer TCO. S'il est armé, la machine **redémarre
seule** si le noyau ne le nourrit plus. C'est la seule protection qui marche
sans intervention humaine ni réseau.

    sudo bash /home/raf/docker/stack/setup-resilience.sh

Le script arme le watchdog (`sp5100_tco`, 60 s) et écrit `kernel.panic=10`
pour qu'un panic noyau redémarre au lieu de figer. Idempotent.

Vérification :

    wdctl
    cat /proc/sys/kernel/panic

#### ⚠️ Faux-vert corrigé le 2026-09-16 (l'étage 1 était DÉSARMÉ en silence)

Le 16/09, la machine a gelé de ~09:18 à 16:18 (**7 h**, coupure brutale :
`recovering journal` + inodes orphelins au redémarrage) **sans que le watchdog
ne redémarre quoi que ce soit**. Diagnostic :

- `sp5100_tco` n'était **pas chargé** : aucun `/dev/watchdog`, `wdctl` vide.
- Cause : le paquet noyau Ubuntu **blackliste** ce module
  (`/usr/lib/modprobe.d/blacklist_linux_7.0.0-31-generic.conf:62
   → blacklist sp5100_tco`), et le paquet en change à chaque montée de version
  (`-30` puis `-31` en 24 h).
- `systemd-modules-load` **applique** la blacklist → chaque boot journalise
  `Module 'sp5100_tco' is deny-listed (by kmod)`, puis
  `Failed to open any watchdog device before the initial transaction completed`.
- `/etc/modules-load.d/watchdog-bmax.conf` était donc **inerte**, et
  `RuntimeWatchdogSec=60s` **armé dans le vide**.

**Pourquoi le test du 15/09 n'a pas vu le défaut :** le script chargeait le
module par un `modprobe` **à la main**, qui n'applique pas la blacklist. Tout
était vert le soir du test, et faux après le premier redémarrage. **Un
garde-fou jamais revu rouge est un faux-vert.**

**Correctif (dans `setup-resilience.sh`, relancer le script) :**

- une unité `watchdog-bmax.service` (oneshot, `DefaultDependencies=no`,
  avant `sysinit.target`) exécute `/sbin/modprobe sp5100_tco` — un `modprobe`
  direct ne consulte pas la blacklist — puis vérifie que `/dev/watchdog`
  existe ;
- une unité `watchdog-bmax-reexec.service` (après `multi-user.target`) fait
  `systemctl daemon-reexec`, car **systemd n'ouvre `/dev/watchdog` qu'une
  fois, très tôt au démarrage**, avant que notre module n'existe. Sans ce
  re-exec, `RuntimeWatchdogSec` reste inerte même module chargé.

**Vérification après le prochain démarrage (les 4 doivent être vrais) :**

    ls -l /dev/watchdog
    wdctl
    systemctl status watchdog-bmax.service
    journalctl -b -1 --no-pager | grep -c "deny-listed"   # doit valoir 0 après le correctif

Si `wdctl` reste vide, c'est que le module ne se charge plus sur le noyau en
cours : vérifier `modprobe -n -v sp5100_tco`.

**Preuve que le watchdog fonctionne vraiment — TEST RÉUSSI (2026-09-15)**

Armer un watchdog n'est pas la même chose que prouver qu'il redémarre la
machine. Le BMAX a un DMI minimaliste (`AMD / AMI / AR7`) : la présence du
timer TCO n'était pas garantie. On l'a donc testé pour de vrai, par un gel
volontaire.

    sudo bash /home/raf/docker/stack/test-watchdog.sh

Le script journalise l'état AVANT (dont le `boot_id`), charge le module, puis
ouvre `/dev/watchdog` et **cesse de le nourrir**.

**Résultat mesuré :**

- Module : `sp5100_tco` → `SP5100 TCO timer [version 0]`
- Dernier log avant reset : 17:37:44 · Premier log après : 17:38:50 → **66 s de silence**
- Séquence d'arrêt propre : **aucune** (coupure brutale)
- `boot_id` avant / après : différents

La machine a donc redémarré **seule**, sans intervention humaine, dans le
délai du chien de garde. **La protection de l'étage 1 est réelle, pas
théorique** — c'est vérifié sur ce matériel précis.

Après coup, lire le verdict :

    bash /home/raf/docker/stack/verif-watchdog.sh

Le verdict compare les `boot_id` avant/après : c'est la preuve fiable, plus
que l'horloge ou l'uptime.

Pour annuler un test en cours (rien ne s'est passé au bout de 3 min) :

    sudo kill $(cat /home/raf/.hermes/watchdog-test.lock)
    sudo modprobe -r sp5100_tco

### Limite honnête de l'étage 1

Le watchdog couvre le **gel logiciel** (noyau figé, pilote en boucle). Il ne
couvre **pas** le gel matériel (alimentation, RAM, CPU) : dans ce cas le
timer TCO ne tourne plus non plus et rien ne redémarre. D'où les étages 2 et 3.

**Étage 2 — surveillance externe (te prévient sur ton téléphone) — ✅ PROUVÉ**

Nécessite un compte sur un service hors de la maison. Le BMAX appelle une URL
depuis Internet ; si l'appel cesse, le service alerte (mail, push, SMS).

**Choix : healthchecks.io** (et non Uptime Kuma). Kuma tourne *sur* le BMAX :
machine gelée = Kuma gelé = aucune alerte. Il ne peut pas signaler sa propre
mort. Le gratuit de healthchecks.io couvre le besoin : 20 sondes, alertes
illimitées par mail / Discord / Slack / Telegram / ntfy.

Réglages de la sonde sur le site : **Period 5 min**, **Grace Time 15 min**
(c'est *Period + Grace* qui donne le délai d'alerte ≈ 20 min), et une
**intégration active** (mail, Discord…) — sans elle la panne est détectée
mais personne n'est prévenu.

    bash /home/raf/docker/stack/setup-external-monitor.sh 'URL_DE_PING'

**Aucun sudo nécessaire** : tout vit dans `~/.hermes/` et dans le crontab de
raf (contrairement à l'étage 1, qui touche au noyau).

Un **script de relance automatique est en place** (`cron @reboot`) : la sonde
se réarme seule après chaque redémarrage, y compris après un reset par
watchdog. URL et état dans `~/.hermes/external-monitor.url` (mode 600) / `.state`.

Cadence : **5 min**, alignée sur la Period. Pinger plus souvent ne détecte
rien de plus vite (le délai dépend de la Grace), et le gratuit ne garde que
100 entrées d'historique par sonde — à 1 min, cela ne couvrirait que ~1h40.

**Preuve mesurée (2026-09-15) :**

- 19:00:01 — le cron envoie `up` **seul** (visible dans le journal)
- 19:01:33 — signal d'échec volontaire → **alerte mail reçue**
- 19:05:01 — le cron renvoie `up` → **retour au vert automatique**
- l'IPv6 vue par healthchecks correspond à celle du BMAX dans les logs
  tailscaled : c'est bien cette machine qui est surveillée

Vérification :

    bash /home/raf/docker/stack/setup-external-monitor.sh --status
    /home/raf/docker/stack/external-monitor-ping.sh test

**Étage 3 — prise connectée + Wake-on-LAN (le recours manuel)**

Le watchdog ne sauve pas d'un gel *matériel* (alim, RAM, CPU). Là, il faut
couper l'alimentation depuis l'extérieur : **prise connectée** (Tasmota,
Shelly) sur le BMAX, pilotable depuis le téléphone.

Pour que la machine puisse se rallumer ensuite, **Wake-on-LAN** doit être
actif. Il était **désactivé** (`/sys/class/net/enp1s0/device/power/wakeup =
disabled`) — le script de l'étage 1 l'active.

**Côté BIOS, une seule fois à la main :**

- `ErP Ready` / `EuP` → **Disabled** (sinon la carte réseau n'est plus
  alimentée machine éteinte)
- `Power On By PCIE/PCI` / `Wake on LAN` → **Enabled**

**Dernier recours en SSH** — si le noyau répond encore un peu mais que rien
d'autre ne marche, SysRq est actif (176) :

    echo b | sudo tee /proc/sysrq-trigger

Hard reboot immédiat, sans attendre la fin des délais.

### En voyage, machine bloquée : l'ordre des gestes

1. L'alerte de l'étage 2 arrive sur le téléphone.
2. Attendre 90 s : le watchdog matériel redémarre peut-être déjà.
3. Toujours rien ? Prise connectée → couper 15 s → rallumer.
4. La machine remonte toute seule (services systemd `enabled`, `@reboot`).
5. Si même ça échoue : le problème est matériel, il faut quelqu'un sur place.

---

# raf-dell — Fiche opérationnelle

Deuxième machine du homelab : **Dell Precision Tower 5810** (Xeon v3, GTX 1080 Ti),
Ubuntu Server 26.04 LTS, headless. Utilisée comme **nœud GPU** piloté depuis le BMAX.

- **Hostname** : `raf-linux-Precision-Tower-5810` (alias SSH `dell`)
- **IP LAN** : `192.168.1.7` · **MAC** : `34:17:eb:da:f2:27` (interface `enp0s25`, RJ45)
- **GPU** : GTX 1080 Ti (driver 580.x max, CUDA 12.9 / PyTorch 2.7)

## Sommeil et réveil automatique (Wake-on-LAN)

Décidé le 2026-09-30 : le Dell s'endort seul quand personne ne l'utilise, et le
BMAX le réveille à la demande. Scripts dans `~/projets/bmax-wol/` (sur le BMAX).

**Réveiller depuis le BMAX :**

    ~/projets/bmax-wol/wol-dell.sh -w 90

Envoie le magic packet (102 o : 6×FF + 16×MAC) sur `192.168.1.7`, `192.168.1.255`
et `255.255.255.255` (port 9), puis attend que le Dell réponde.

**Veille automatique (côté Dell)** — après **15 min** sans activité, si aucun des
trois signaux n'est présent :

- aucune connexion SSH entrante active,
- aucun process GPU (`nvidia-smi --query-compute-apps`),
- aucune session locale (clavier/souris) active.

Installé par `arm-wol-dell.sh` + `auto-suspend-dell.sh` (à lancer avec `sudo` sur
le Dell, une fois) :

| Élément | Rôle |
|---|---|
| `wol-arm.service` | réarme `ethtool wol g` à chaque démarrage |
| `/usr/lib/systemd/system-sleep/wol-arm` | réarme le WoL après chaque veille |
| `/etc/polkit-1/rules.d/49-suspend-nopasswd.rules` | suspend sans mot de passe (raf-linux, groupe sudo) |
| `dell-idle-suspend.timer` + `dell-idle-suspend.service` | sonde l'inactivité toutes les 2 min, veille au bout de 15 min |
| `/usr/local/bin/dell-idle-suspend` | le script de décision |

**Éteindre depuis le BMAX :**

    ~/projets/bmax-wol/poweroff-dell.sh
    # → puis, une fois éteint (LED éteinte) : ~/projets/bmax-wol/wol-dell.sh -w 120

**Pourquoi pas `ssh dell 'sudo systemctl poweroff'`** : polkit ne considère jamais
une session SSH (`Remote=yes`) comme « active », or `power-off` est en
`auth_admin_keep` → `sudo: A terminal is required to authenticate`. La solution qui
marche : `gnome-session-quit --power-off`, qui fait porter la demande par
gnome-shell — dont la session locale (seat0, `Active=yes`) est autorisée
(`pkcheck --process <gnome-shell>` → rc 0). Démarrage de l'arrêt en ~10 s.

**Pourquoi la règle polkit** : sans elle, `ssh dell 'systemctl suspend'` échoue
(« Access denied … requires interactive authentication ») — polkit exige une
session locale, or en SSH il n'y en a pas.

**BIOS (fait une fois à la main)** : `ErP/EuP` désactivé, `Wake on LAN` activé.

**Limite** : Tailscale **ne réveille pas** (paquet routé au niveau IP, pas de
magic packet sur le LAN). Le WoL doit venir d'une machine du même /24 — le BMAX
(`192.168.1.5`) convient.

## Vérification (tests réels)

**2026-09-30 — réveil depuis la veille** : `PM: suspend entry (deep)` à 20:44:44 →
`PM: suspend exit` à 21:04:51 ; Dell joignable **11 s** après le magic packet ;
après réveil `Wake-on: enabled`, `dell-idle-suspend.timer` toujours `active`.

**2026-10-01 — réveil à froid (arrêt complet)** : premier essai du `poweroff`
lancé à 07:28 sans succès ; deuxième appel abouti → `systemd-shutdown` à
**07:29:32**, journal stoppé, plus aucune réponse (ping/SSH/port 22). Magic packet
depuis le BMAX → **en ligne après 49 s**, boot neuf `07:31:08`, 0 veille depuis
(donc démarrage à froid, pas reprise). `wol-arm.service` actif,
`/sys/bus/pci/devices/0000:00:19.0/power/wakeup = enabled`.

Résumé : WoL fonctionnel depuis **veille** (10-11 s) **et depuis arrêt complet**
(49 s).

## Voir aussi
- [[11 Ressources/raf-bmax — Fiche opérationnelle]]
- [[30 Journal/2026-09-30]]

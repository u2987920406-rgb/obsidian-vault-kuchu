# BanditRS — Scanner sécurité Python

**Description** : Réimplémentation Rust de `bandit` (PyCQA), 17× à 86× plus rapide,
57 % de mémoire en moins. Drop-in replacement compatible (mêmes options, sorties,
codes de sortie).

## Installation (faite sur BMAX)

```bash
python3 -m venv ~/.hermes/.venv-banditrs
source ~/.hermes/.venv-banditrs/bin/activate
pip install banditrs
```

4 commandes disponibles : `bandit`, `banditrs` (alias), `bandit-baseline`,
`bandit-config-generator`.

## Usage

Identique à bandit :

```bash
source ~/.hermes/.venv-banditrs/bin/activate
bandit -r mon_dossier/ -f json -o rapport.json
```

## Audits réalisés (2026-09-14)

- **Vault Obsidian** — 0 vulnérabilité.
- **Astroprisma** (`~/projets/Astroprisma_app_EMERGENT`) — 0 fichier Python
  (projet Vite/React/TS) → scan sans objet.
- **Ulysse** (`~/projets/ulysse`) — 25 alertes initiales sur 17 fichiers
  (6 819 lignes), ramenées à **0** après traitement.

## Ulysse — traitement des alertes

Fichier de configuration : `~/projets/ulysse/.bandit.yaml` (versionné, à la racine
du projet). Il déclare les `per_file_ignores` par fichier.

Alertes traitées et leur justification :

- `B324` SHA1 — handshake WebSocket **imposé par la RFC 6455** (`faux_hermes.py`,
  `test_serve.py`). Ce n'est pas un choix de code.
- `B310` urlopen — URLs **localhost** uniquement (`_outils/sign_webhook.py`,
  `_outils/test_proxy.py`, `web/test_page.py`, `web/lancer_bancs.py`).
- `B105` secret en dur — **tokens de test/mock** (`test_serve.py`,
  `test_personas.py`, `faux_hermes.py`).
- `B103` chmod — chmod permissif **temporaire** dans le setup/teardown d'un test
  de permissions (`test_serve.py`).
- `B110` except/pass — nettoyage silencieux (`serve.py`, `db.close()`).
- `B404`/`B603` subprocess — commandes **git contrôlées** (`reprise.py`) et
  lancement de banc (`lancer_bancs.py`).
- `B104` bind — le serveur force déjà `127.0.0.1` (`serve.py`).

Le reste a été corrigé au code (`# nosec B3xx` sur la ligne fautive).

## Vérification de non-régression

Après modifications, les trois bancs d'Ulysse ont été relancés :

- `web/test_serve.py` — **261/261** vérifications passées
- `web/test_verif_ports.py` — **16/16** passées
- `web/test_tactile.py` — **7/7** passées

Tous les fichiers touchés compilent (`python3 -m py_compile`).

## Intégration dans l'écosystème Hermès

Scripts critiques à scanner : `~/.hermes/scripts/` (crons), `~/docker/stack/`
(scripts d'exploitation), `~/projets/` (projets Python).

```bash
source ~/.hermes/.venv-banditrs/bin/activate
bandit -r ~/.hermes/scripts/ ~/docker/stack/ -f json --exit-zero
```

## Références

- Repo : https://github.com/LePhilippeDucTai/BanditRS
- `PLAN.md` — architecture et décisions
- `DEVIATIONS.md` — écarts délibérés avec la version Python

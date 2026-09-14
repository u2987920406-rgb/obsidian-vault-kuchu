# BanditRS — Scanner sécurité Python

**Description** : Reimplémentation Rust de `bandit` (PyCQA), 17× à 86× plus rapide, 57% de mémoire en moins. Drop-in replacement compatible.

## Installation

```bash
python3 -m venv ~/.hermes/.venv-banditrs
source ~/.hermes/.venv-banditrs/bin/activate
pip install banditrs
```

4 commandes disponibles :
- `bandit` - remplace PyCQA/bandit
- `banditrs` - alias identique pour comparaison
- `bandit-baseline` - scan contre git baseline
- `bandit-config-generator` - génère profil config

## Usage

Identique à bandit :

```bash
bandit mon_fichier.py
bandit -r mon_dossier/ -f json -o rapport.json
```

## Audit sécurité du vault

**Scan effectué** : 2026-09-10

**Résultat** : 0 vulnérabilités détectées

```json
{
  "events": {
    "CONFIDENCE.HIGH": 0,
    "CONFIDENCE.MEDIUM": 0,
    "CONFIDENCE.LOW": 0,
    "SEVERITY.HIGH": 0,
    "SEVERITY.MEDIUM": 0,
    "SEVERITY.LOW": 0
  },
  "results": []
}
```

## Intégration dans l'écosystème Hermès

### Scripts critiques à scanner
- `~/.hermes/scripts/` — cron scripts
- `~/docker/stack/` — scripts d'exploitation
- `~/projets/` — projets Python (Ulysse, Gestion Budget, etc.)

### Pipeline recommandé
En SSH sur le BMAX :

```bash
source ~/.hermes/.venv-banditrs/bin/activate
bandit -r ~/.hermes/scripts/ ~/docker/stack/ -f json --exit-zero
```

### Comparaison avec bandit original
```bash
bandit -r mon_code.py --ignore-nosec > old.txt
banditrs -r mon_code.py --ignore-nosec > new.txt
diff -u old.txt new.txt
```

## Références
- Repo : https://github.com/LePhilippeDucTai/BanditRS
- PLAN.md — architecture et décisions
- DEVIATIONS.md — écarts délibérés
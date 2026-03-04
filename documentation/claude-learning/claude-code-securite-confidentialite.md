# Claude Code sur Mac — Sécurité & Confidentialité

> Résumé de la conversation du 4 mars 2026

---

## 1. Façon la plus sécurisée d'utiliser Claude en local

### Options disponibles

| Option | Usage | Niveau de contrôle |
|---|---|---|
| **Claude Code** | Développement, terminal | Élevé |
| **API directe** | Intégration custom | Élevé |
| **claude.ai** | Usage web quotidien | Standard |

### Ce qui n'existe pas
Il n'existe **pas** de version de Claude téléchargeable pour tourner entièrement hors ligne. Les requêtes transitent toujours par les serveurs d'Anthropic. Pour un usage 100% offline, il faut se tourner vers des modèles open source via **Ollama** (Llama, Mistral, etc.).

---

## 2. Installation de Claude Code sur Mac

### ⚠️ Correction importante
`brew install --cask claude-code` existe, mais les mises à jour ne sont **pas automatiques**. Il faut lancer `brew upgrade claude-code` régulièrement.

### Méthode recommandée (mises à jour automatiques)
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

> Ne jamais utiliser `sudo npm install -g` — risques de sécurité et problèmes de permissions.

### Vérifier la signature du binaire
Les binaires macOS sont signés par **"Anthropic PBC"** et notariés par Apple. Les checksums SHA256 sont publiés dans les manifests de release.

---

## 3. Confidentialité des données

- Les données transitent par les serveurs d'Anthropic
- Anthropic affirme ne **pas** utiliser ces données pour entraîner ses modèles
- **Action immédiate** : Settings > Privacy → désactiver **"Help improve Claude"**

---

## 4. Restreindre l'accès aux dossiers

### ⚠️ Avertissement
`.claudeignore` ne fonctionne **pas** de manière fiable — des chercheurs ont démontré qu'un fichier `.env` peut être lu malgré une règle d'exclusion (bug reproduit en janvier 2026).

### Configuration globale — `~/.claude/settings.json`

```bash
mkdir -p ~/.claude
```

```json
{
  "permissions": {
    "deny": [
      "Read(~/**)",
      "Write(~/**)",
      "Bash(curl:*)",
      "Bash(wget:*)",
      "Bash(rm:*)",
      "Read(**/.env)",
      "Read(**/.env.*)",
      "Read(**/secrets/**)",
      "Read(**/.ssh/**)",
      "Read(**/.aws/**)"
    ],
    "allow": [
      "Read(/ton/dossier/projet/**)",
      "Write(/ton/dossier/projet/**)"
    ]
  }
}
```

### Configuration non-overridable (niveau le plus sécurisé)

Sur Mac, `managed-settings.json` ne peut pas être écrasé par les paramètres utilisateur ou projet :

```bash
sudo mkdir -p "/Library/Application Support/ClaudeCode"
sudo nano "/Library/Application Support/ClaudeCode/managed-settings.json"
```

### Hook Python de protection (filet de sécurité)

Créer `~/.claude/hooks/protect_sensitive_files.py` :

```python
#!/usr/bin/env python3
import sys, json
from pathlib import Path

SENSITIVE = {'.env', '.pem', '.key', '.token', 'credentials.json'}
ALLOWED_DIR = Path("/ton/dossier/projet")

data = json.load(sys.stdin)
file_path = data.get('tool_input', {}).get('file_path', '')

if file_path:
    p = Path(file_path).resolve()
    if not str(p).startswith(str(ALLOWED_DIR)):
        print("ACCÈS BLOQUÉ : hors du dossier autorisé", file=sys.stderr)
        sys.exit(2)
    if p.suffix in SENSITIVE or p.name in SENSITIVE:
        print("ACCÈS BLOQUÉ : fichier sensible", file=sys.stderr)
        sys.exit(2)
```

### Lancer Claude Code dans le bon dossier

```bash
cd /ton/dossier/projet
claude
```

> Claude n'aura accès qu'au répertoire courant par défaut. L'accès à d'autres répertoires ne se fait qu'avec `--add-dir`, à ne pas utiliser.

---

## 5. Résumé des mesures de sécurité

| Mesure | Niveau de protection |
|---|---|
| `managed-settings.json` avec deny rules | ⭐⭐⭐ Non-overridable |
| Hook Python de protection | ⭐⭐⭐ Filet de sécurité |
| Désactiver "Help improve Claude" | ⭐⭐ Confidentialité données |
| Lancer depuis le bon dossier uniquement | ⭐⭐ Scope limité |
| Mettre à jour régulièrement | ⭐ Corrections de bugs |

---

## 6. Vérifier la version installée et le modèle utilisé

### Version de Claude Code (l'outil CLI)

```bash
claude --version
```

Pour mettre à jour :
```bash
claude update
```

> Version actuelle sur npm : **2.1.66**

### Modèle IA utilisé (dans une session)

```
/model
```

Spécifier le modèle au lancement :
```bash
claude --model sonnet   # Défaut, équilibré
claude --model opus     # Plus puissant, plus lent
```

Voir toute la configuration :
```bash
claude config list
```

### Tableau récapitulatif

| Ce que tu veux savoir | Commande |
|---|---|
| Version de l'outil Claude Code | `claude --version` |
| Modèle IA actif (dans une session) | `/model` |
| Toute la config | `claude config list` |

---

## Ressources

- Documentation Claude Code : [docs.claude.ai](https://docs.claude.com/en/docs/claude-code/overview)
- Politique de confidentialité Anthropic : [anthropic.com/privacy](https://www.anthropic.com/privacy)
- Support Claude.ai : [support.claude.com](https://support.claude.com)

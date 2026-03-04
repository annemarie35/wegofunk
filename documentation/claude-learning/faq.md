# FAQ


## Est-ce que c'est vrai sur Ollama ne permet pas de faire tourner les modèles Claude d'Anthropic en local.

Non — ce n’est **plus tout à fait vrai** aujourd’hui, mais il y a des nuances importantes à comprendre :

### ✅ Ce qu’Ollama permet **actuellement**

Depuis **fin 2025 / début 2026**, Ollama a intégré une **compatibilité avec l’API *Anthropic Messages***. Grâce à cela, tu peux faire tourner l’outil **Claude Code** (l’assistant de développement terminal d’Anthropic) en **pointant vers un serveur local Ollama**, et utiliser des **modèles open-source qui tournent en local** plutôt que les modèles propriétaires d’Anthropic sur le cloud. ([Ollama][1])

Concrètement :

* Ollama expose une API locale compatible avec l’API Anthropic, ce qui permet à Claude Code de fonctionner en redirigeant ses requêtes vers Ollama. ([Ollama Documentation][2])
* En utilisant cette API locale, tu peux lancer Claude Code et spécifier un modèle disponible dans Ollama – par exemple `qwen3-coder` ou `gpt-oss:20b` — et tout tourne sur ta machine. ([Ollama][1])
* Cela te donne la possibilité d’exécuter Claude Code *sans abonnement ni appels API vers Anthropic*, tant que tu as le modèle local requis. ([Angelo Lima - Dev Web & IA][3])

➡️ **Donc oui, Ollama permet maintenant d’utiliser l’écosystème “Claude” en local, mais pas avec les *modèles propriétaires* Claude 3 / Opus / Sonnet originaux d’Anthropic.** Ce qui s’exécute en local n’est pas exactement le même modèle “Claude officiel” d’Anthropic : ce sont des modèles tiers open-source que tu guides via l’API d’Ollama. ([Ollama][1])

### ⚠️ Limites & précisions

* **Tu ne peux pas exécuter les modèles propriétaires fermés « Claude » d’Anthropic en local** tels qu’ils tournent chez Anthropic eux-mêmes. Ollama ne comprend pas —et ne fournit pas— ces modèles propriétaires directement. ([clubic.com][4])
* Ce que tu obtiens est un workflow *similaire à Claude* (par exemple **Claude Code**), mais en utilisant **d’autres modèles AI open-source locaux** qui *remplacent* les modèles Claude originaux. ([Ollama][1])
* Certaines fonctions avancées (outil/exécution de code, expériences spécifiques) peuvent dépendre de la qualité du modèle open-source choisi et de la configuration (taille du contexte, ressources GPU, etc.). ([Angelo Lima - Dev Web & IA][3])

### 🧠 En résumé

✔️ **Vrai** : Ollama **permet d’exécuter en local des workflows “Claude-like” (notamment *Claude Code*)** avec des modèles open-source et une API Anthropic locale. ([Ollama][1])
✖️ **Faux** : Ollama **ne permet pas d’exécuter les modèles propriétaires *Claude* d’Anthropic tels qu’ils sont fournis par Anthropic** (sauf via leur API cloud officielle). ([clubic.com][4])

Si tu veux, je peux aussi te donner un exemple de configuration pas à pas pour faire tourner Claude Code avec Ollama en local.

[1]: https://ollama.com/blog/claude?utm_source=chatgpt.com "Claude Code with Anthropic API compatibility · Ollama Blog"
[2]: https://docs.ollama.com/api/anthropic-compatibility?utm_source=chatgpt.com "Anthropic compatibility - Ollama"
[3]: https://angelo-lima.fr/en/claude-code-ollama-local-llm-en/?utm_source=chatgpt.com "Ollama Now Supports Claude Code"
[4]: https://www.clubic.com/telecharger-fiche553623-ollama.html?utm_source=chatgpt.com "Télécharger Ollama (gratuit) Windows, Mac, Linux - Clubic"


## Comment faire tourner Ollama sur ma machine avec Claude

Voici un **guide simple et concret** pour utiliser **Claude Code** avec **Ollama** en local, sans appeler les modèles cloud de **Anthropic**.

---

# 🧰 Prérequis

* macOS, Linux ou Windows (WSL recommandé)
* Node.js ≥ 18
* Ollama installé
* 16–32 Go de RAM recommandés pour les modèles > 14B

---

# 1️⃣ Installer Ollama

👉 Télécharger et installer depuis le site officiel d’Ollama.

Ensuite, vérifier que ça fonctionne :

```bash
ollama --version
```

---

# 2️⃣ Télécharger un modèle compatible

Claude Code fonctionne mieux avec un modèle orienté code.

Exemples recommandés :

```bash
ollama pull qwen2.5-coder:14b
```

ou plus léger :

```bash
ollama pull deepseek-coder:6.7b
```

Teste ensuite :

```bash
ollama run qwen2.5-coder:14b
```

Si le modèle répond, tout est OK.

---

# 3️⃣ Démarrer le serveur Ollama (API locale)

Par défaut :

```bash
ollama serve
```

L’API sera disponible sur :

```
http://localhost:11434
```

---

# 4️⃣ Installer Claude Code

```bash
npm install -g @anthropic-ai/claude-code
```

Vérifie :

```bash
claude --version
```

---

# 5️⃣ Configurer Claude Code pour utiliser Ollama

Claude Code utilise normalement l’API officielle d’Anthropic.
On va la rediriger vers Ollama.

Dans ton terminal :

```bash
export ANTHROPIC_BASE_URL=http://localhost:11434/v1
export ANTHROPIC_API_KEY=ollama
```

(Sur Windows PowerShell :)

```powershell
setx ANTHROPIC_BASE_URL "http://localhost:11434/v1"
setx ANTHROPIC_API_KEY "ollama"
```

---

# 6️⃣ Lancer Claude Code avec un modèle local

Exemple :

```bash
claude --model qwen2.5-coder:14b
```

Et voilà 🎉
Claude Code va maintenant utiliser ton modèle local via Ollama.

---

# ⚠️ Points importants

* Ce **n’est pas le vrai modèle Claude 3 / Sonnet / Opus**.
* C’est un modèle open-source exécuté localement.
* Les performances dépendent fortement de ta RAM et de ton GPU.
* Le contexte (longueur mémoire) peut être plus limité que Claude cloud.

---

# 🚀 Astuce performance

Si tu as une carte NVIDIA :

```bash
OLLAMA_GPU_LAYERS=100 ollama run qwen2.5-coder:14b
```

Pour Mac M1/M2/M3 → accélération Metal automatique.

---

# 🧠 Résumé

Tu obtiens :

* ✅ Assistant type Claude
* ✅ 100 % local
* ✅ Gratuit
* ❌ Pas les modèles propriétaires d’Anthropic

---

Si tu veux, je peux aussi te proposer :

* 🔥 la meilleure config selon ton matériel
* 🧠 le meilleur modèle code vs généraliste
* ⚙️ une config optimisée pour gros projets

Tu es sur quel OS et combien de RAM/GPU ?

## Avec Apple M1 Max + 32 Go RAM, tu as une excellente machine pour faire tourner des modèles locaux confortablement via Ollama.

Parfait 👌 — avec **Apple M1 Max + 32 Go RAM**, tu as une excellente machine pour faire tourner des modèles locaux confortablement via **Ollama**.

Voici **la configuration optimale** pour ton setup.

---

# 🚀 Ce que ton M1 Max peut faire tourner confortablement

### ✅ Très fluide

* 7B à 14B → ultra rapide
* 32B quantisé (Q4_K_M) → très utilisable
* Contexte 8k–32k sans souci

### ⚠️ Possible mais plus lourd

* 70B quantisé → ça passe, mais plus lent et très gourmand en RAM

---

# 🧠 Meilleurs modèles pour toi

## 💻 Développement (équivalent Claude Code local)

### 🔥 Recommandé

```bash
ollama pull qwen2.5-coder:32b
```

Très bon en :

* refactoring
* génération TypeScript / Python
* compréhension de gros projets

Alternative plus rapide :

```bash
ollama pull qwen2.5-coder:14b
```

---

## 🧠 Modèle généraliste (conversation, raisonnement)

```bash
ollama pull llama3.1:70b
```

Très bon raisonnement long.

Alternative plus légère et rapide :

```bash
ollama pull llama3.1:8b
```

---

# ⚙️ Optimisation spéciale Apple Silicon

Bonne nouvelle :
👉 **Metal est utilisé automatiquement** sur M1 Max.
Tu n’as rien à configurer.

Mais tu peux optimiser :

```bash
export OLLAMA_NUM_PARALLEL=4
export OLLAMA_MAX_LOADED_MODELS=1
```

Cela évite de saturer la RAM.

---

# 🧩 Configuration idéale pour Claude Code

Après avoir configuré :

```bash
export ANTHROPIC_BASE_URL=http://localhost:11434/v1
export ANTHROPIC_API_KEY=ollama
```

Lance par exemple :

```bash
claude --model qwen2.5-coder:32b
```

Sur ta machine :

* ⚡ Réponses rapides
* 🧠 Très bon niveau de code
* 🔒 100% local

---

# 🎯 Recommandation personnelle pour TON Mac

Avec 32 Go RAM :

| Usage                        | Modèle recommandé   |
| ---------------------------- | ------------------- |
| Code sérieux / gros projet   | `qwen2.5-coder:32b` |
| Dev rapide / quotidien       | `qwen2.5-coder:14b` |
| Chat / stratégie / réflexion | `llama3.1:70b`      |
| Ultra rapide                 | `llama3.1:8b`       |

---

Si tu veux, je peux te faire :

* ⚡ un comparatif concret `14B vs 32B` sur ton Mac
* 🏗 une config idéale pour gros repo (genre 100k+ lignes)
* 🔥 la config la plus proche possible de Claude 3 Sonnet en local

Tu bosses plutôt sur quoi ? (web, backend, IA, mobile…)

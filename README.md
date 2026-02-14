# 🧠 Agentic RAG – Recherche Documentaire + Web avec CrewAI

**Un système Agentic RAG qui combine recherche dans vos documents (PDF) + fallback web, orchestré par des agents CrewAI, avec support LLM local (Ollama) ou API.**

<div align="center">

## 🌐 **[VOIR LA DÉMO (YouTube)](https://youtu.be/O4yBW_GTRk0)** 🌐

[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-App-red?style=for-the-badge&logo=streamlit)](https://streamlit.io/)
[![CrewAI](https://img.shields.io/badge/CrewAI-Agents-black?style=for-the-badge)](https://docs.crewai.com/)
[![Qdrant](https://img.shields.io/badge/Qdrant-VectorDB-ff4d4d?style=for-the-badge&logo=qdrant)](https://qdrant.tech/)
[![Ollama](https://img.shields.io/badge/Ollama-Local%20LLM-000000?style=for-the-badge)](https://ollama.com/)

</div>

---

## 📋 Vue d’ensemble

Ce projet est une **implémentation Agentic RAG** (Retrieval-Augmented Generation) construite autour de **CrewAI** :  
au lieu d’un pipeline statique « retrieve → generate », on orchestre un **workflow multi-agents** capable de :

- 🔎 **Chercher dans un PDF** (indexation locale + recherche sémantique)
- 🌐 **Basculer vers une recherche web** si la réponse n’est pas trouvée dans les docs
- 🧠 **Synthétiser une réponse** propre et structurée

👉 Le projet inclut plusieurs variantes d’exécution :
- **Streamlit (Serper)** : recherche web via `SerperDevTool`
- **Streamlit (Local DeepSeek-R1)** : LLM local via `ollama/deepseek-r1` + web via FireCrawl
- **Streamlit (Local Llama 3.2)** : LLM local via `ollama/llama3.2` + web via FireCrawl

---

## ✨ Fonctionnalités principales

### 1️⃣ Agentic RAG (CrewAI)
- 🤖 Orchestration **multi-agents** (process séquentiel)
- 🧩 Décomposition des tâches : **retrieval → synthesis**
- 🔁 Workflow extensible (ajout d’outils, routage, mémoire, etc.)

### 2️⃣ Recherche dans vos documents (PDF)
- 📄 Extraction texte PDF via **MarkItDown**
- 🧠 Chunking sémantique via **chonkie (SemanticChunker)**
- 📦 Stockage vectoriel **Qdrant en mémoire** (rapide pour démos)

### 3️⃣ Fallback Web Search
- 🔍 Via **SerperDevTool** (Google-like search)
- 🌐 Ou via **FireCrawlWebSearchTool** (selon la variante)

### 4️⃣ Support LLM local (Ollama) ou API
- 🧠 Modèles locaux : `ollama/deepseek-r1`, `ollama/llama3.2`
- 🔑 Option API (selon configuration)

---

## 🏗️ Architecture (simplifiée)

```
Utilisateur (Streamlit)
   │
   ▼
Agent Retriever (CrewAI)
   ├── 📄 DocumentSearchTool (PDF → chunks → Qdrant → top matches)
   └── 🌐 Web Search (Serper / FireCrawl)
   │
   ▼
Agent Synthesizer (CrewAI)
   │
   ▼
Réponse finale (citations/segments pertinents)
```

---

## 🛠️ Technologies utilisées

| Technologie | Utilisation |
|---|---|
| **CrewAI** | Orchestration d’agents + tâches |
| **Streamlit** | Interface web interactive |
| **MarkItDown** | Extraction texte depuis PDF |
| **chonkie (SemanticChunker)** | Découpage sémantique des documents |
| **Qdrant (in-memory)** | Vector store pour la recherche |
| **SerperDevTool** | Recherche web |
| **FireCrawl** | Recherche web / crawling (selon variante) |
| **Ollama** | Exécution de LLMs locaux |
| **Pydantic** | Schémas d’entrées outils |

---

## 📋 Prérequis

- Python **3.10+** (conforme au `pyproject.toml`)
- (Optionnel) **Ollama** si vous utilisez les variantes locales
- Clés API selon votre mode :
  - `SERPER_API_KEY` (recherche web via Serper)
  - `FIRECRAWL_API_KEY` (web via FireCrawl)
  - `OPENAI_API_KEY` (si vous utilisez un modèle API)

---

## 🚀 Installation & Démarrage

### 1️⃣ Cloner le dépôt
```bash
git clone https://github.com/Ayman-cell/agentic-rag.git
cd agentic-rag
```

### 2️⃣ Créer un environnement virtuel
```bash
python -m venv .venv
# Windows
.\.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
```

### 3️⃣ Installer les dépendances
Le dépôt déclare `crewai[tools]` dans `pyproject.toml`.

```bash
pip install -e .
pip install streamlit markitdown "chonkie[semantic]" qdrant-client fastembed
```

> Astuce : si vous préférez, vous pouvez aussi installer directement :
> `pip install "crewai[tools]>=0.86.0,<1.0.0"`

---

## 🔐 Configuration (.env)

Créez un fichier `.env` (ou copiez `.env.example`) :

```bash
cp .env.example .env
```

Exemple (selon vos besoins) :

```dotenv
# Optionnel si vous utilisez OpenAI
MODEL=your_model_name
OPENAI_API_KEY=your_openai_api_key

# Recherche web Serper (app.py)
SERPER_API_KEY=your_serper_api_key

# Recherche web FireCrawl (apps locales)
FIRECRAWL_API_KEY=your_firecrawl_api_key
```

---

## ▶️ Lancer l’application (Streamlit)

### Variante 1 — Serper (web search via SerperDevTool)
```bash
streamlit run app.py
```

### Variante 2 — Local DeepSeek-R1 (Ollama) + FireCrawl
```bash
streamlit run app_deep_seek.py
```

### Variante 3 — Local Llama 3.2 (Ollama) + FireCrawl
```bash
streamlit run app_llama3.2.py
```

📌 Dans l’UI Streamlit :
- uploadez un PDF dans la sidebar
- posez vos questions dans le chat
- le système tente d’abord le document, puis fallback web si nécessaire

---

## 🧩 Structure du projet

```
agentic-rag/
├── assets/                   # Images / ressources
├── knowledge/                # Documents (PDF) à indexer
├── src/agentic_rag/
│   ├── crew.py               # Agents + tâches CrewAI (config YAML)
│   ├── main.py               # Runner CLI (scripts pyproject)
│   ├── config/
│   │   ├── agents.yaml       # Rôles / objectifs agents
│   │   └── tasks.yaml        # Définition des tâches
│   └── tools/
│       └── custom_tool.py    # DocumentSearchTool (PDF → Qdrant)
├── app.py                    # Streamlit (Serper)
├── app_deep_seek.py          # Streamlit (Ollama DeepSeek-R1 + FireCrawl)
├── app_llama3.2.py           # Streamlit (Ollama Llama 3.2 + FireCrawl)
├── agentic_rag.ipynb         # Notebook démo
├── demo_llama3.2.ipynb       # Notebook démo
├── .env.example              # Variables d’environnement
└── pyproject.toml            # Dépendances + scripts
```

---

## 🧠 Détails : DocumentSearchTool

L’outil `DocumentSearchTool` (dans `src/agentic_rag/tools/custom_tool.py`) :

- extrait le texte du PDF via **MarkItDown**
- crée des chunks via **SemanticChunker** (`chonkie`)
- utilise un embedding model : `minishlab/potion-base-8M`
- stocke les chunks dans **Qdrant en mémoire** (`QdrantClient(":memory:")`)
- renvoie les passages les plus pertinents pour la requête

> ⚠️ Note : le mode `:memory:` est idéal pour démos/POC. Pour la production, utilisez un Qdrant persistant + gestion d’IDs, métadonnées et versions.

---

## 🐛 Troubleshooting

- **Ollama ne répond pas** : vérifiez que le service tourne (`http://localhost:11434`).
- **Aucune réponse trouvée** : testez un PDF plus textuel / vérifiez que l’upload et l’indexation ont bien eu lieu.
- **Clé API manquante** : assurez-vous que `.env` est chargé et que les variables sont définies.

---

## 🤝 Contribution

PRs et issues bienvenues ✅  
Idées d’améliorations :
- persistance Qdrant (Docker) au lieu de `:memory:`
- citations + scores de retrieval
- évaluation (RAGAS / tests de régression)
- ajout reranking (cross-encoder)
- support multi-documents + metadata filtering

---

## 📝 Licence

Voir le fichier `LICENSE` du dépôt.

---

**Dernière mise à jour : 14 février 2026**  
**Version : 0.1.0**

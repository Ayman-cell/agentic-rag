
# Agentic RAG using CrewAI

This project leverages CrewAI to build an Agentic RAG that can search through your docs and fallbacks to web search in case it doesn't find the answer in the docs, have option to use either of deep-seek-r1 or llama 3.2 that runs locally. More details un Running the app section below!

Before that, make sure you grab your FireCrawl API keys to search the web.

**Get API Keys**:
   - [FireCrawl](https://www.firecrawl.dev/i/api)

### Watch Demo on YouTube
[![Watch Demo on YouTube](https://github.com/patchy631/ai-engineering-hub/blob/main/agentic_rag/thumbnail/thumbnail.png)](https://youtu.be/O4yBW_GTRk0)


## Installation and setup

**Get API Keys**:
   - [FireCrawl](https://www.firecrawl.dev/i/api)


**Install Dependencies**:
   Ensure you have Python 3.11 or later installed.
   ```bash
   pip install crewai crewai-tools chonkie[semantic] markitdown qdrant-client fastembed
   ```

**Running the app**:

To use deep-seek-rq use command ``` streamlit run app_deep_seek.py ```, for llama 3.2 use command ``` streamlit run app_llama3.2.py ```
# 🚀 NEXUS EXPLORERS - Plateforme IA de Détection d'Exoplanètes

**Système full-stack combinant Machine Learning, API REST et interface 3D pour la prédiction d'exoplanètes**

<div align="center">

## 🌐 **[DÉCOUVRIR L'APPLICATION](https://nexus-explorer-v10.vercel.app/)** 🌐

[![Vercel](https://img.shields.io/badge/Deployed-Vercel-black?style=for-the-badge)](https://vercel.com/)
[![Next.js](https://img.shields.io/badge/Next.js-15-black?style=for-the-badge&logo=next.js)](https://nextjs.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.103-009688?style=for-the-badge&logo=fastapi)](https://fastapi.tiangolo.com/)
[![Python](https://img.shields.io/badge/Python-3.10+-blue?style=for-the-badge&logo=python)](https://www.python.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5-blue?style=for-the-badge&logo=typescript)](https://www.typescriptlang.org/)

---

## 🔍 Présentation

NEXUS EXPLORERS est une plateforme de recherche d'exoplanètes qui combine des modèles de Machine Learning, une API REST performante et une interface 3D interactive pour visualiser et explorer les candidats détectés. Elle est conçue pour la recherche reproduisible, la démonstration et l'intégration rapide dans des pipelines scientifiques.

## ✨ Fonctionnalités principales

- Détection et score de candidats exoplanètes via modèles ML entraînés
- API REST (FastAPI) pour requêtes et intégration tierce
- Interface 3D et tableau de bord web (Next.js) pour visualisation interactive
- Déploiement prêt pour Vercel/Cloud avec CI minimal

## 🧰 Stack technique

- Frontend: Next.js / TypeScript
- Backend: FastAPI / Python
- Modèles: Python (scikit-learn, PyTorch, ou équivalent)<br/>
- Stockage: vector store / base de données légère selon configuration

## 🚀 Installation rapide (développeur)

Prérequis: Python 3.10+ et Node.js.

1. Cloner le dépôt

```bash
git clone <votre-repo>
cd agentic-rag
```

2. Installer les dépendances Python

```bash
python -m pip install -r requirements.txt
```

3. Lancer l'API (exemple)

```bash
uvicorn src.agentic_rag.main:app --reload
```

4. Lancer le frontend (si présent)

```bash
cd frontend && npm install && npm run dev
```

## 📦 Usage

Consulter l'API via `http://localhost:8000/docs` pour tester les endpoints et utiliser l'interface web pour visualiser les résultats.

## 🤝 Contribution

Toutes contributions sont les bienvenues : issues, PR, corrections de docs ou améliorations de modèle.

---

Si vous souhaitez que j'adapte ce README (ajout de badges, captures d'écran, instructions Windows spécifiques, ou détails d'installation exacts), dites-moi ce que vous voulez changer et je le ferai.
 
---

**Remarque importante :** le contenu ci‑dessous décrit précisément ce qui existe dans ce dépôt (`app.py`, `app_deep_seek.py`, `app_llama3.2.py`, `src/agentic_rag/`, etc.). Le projet est un prototype « Agentic RAG » construit autour de CrewAI et d'outils d'indexation de documents.

## Structure du dépôt

- `app.py` : application Streamlit principale (utilise `crewai` et `crewai_tools.SerperDevTool`). Permet d'uploader un PDF, l'indexer via `DocumentSearchTool` et interroger la crew.
- `app_deep_seek.py` : variante Streamlit utilisant un LLM local (ollama `deepseek-r1`) via `LLM` et `FireCrawlWebSearchTool` pour la recherche web.
- `app_llama3.2.py` : variante Streamlit utilisant un LLM local (ollama `llama3.2`) via `LLM` et `FireCrawlWebSearchTool`.
- `src/agentic_rag/` : code source principal du projet
   - `crew.py` : définition de la `Crew` (`AgenticRag`) avec agents et tâches configurés via `config/agents.yaml` et `config/tasks.yaml`.
   - `main.py` : petit runner CLI utilisable via `pyproject` scripts (`run`, `train`, `replay`, `test`).
   - `tools/custom_tool.py` : implémentation de `DocumentSearchTool` (extraction PDF via `MarkItDown`, découpage sémantique via `chonkie`, stockage dans Qdrant en mémoire).
   - `config/agents.yaml` et `config/tasks.yaml` : configuration des agents et tâches (prompts, rôles et objectifs).
- `knowledge/` : dossier prévu pour les documents de connaissance (PDFs) — utilisé par l'outil d'indexation.
- `assets/` : images utilisées par les apps Streamlit (logos, miniatures).
- `demo_llama3.2.ipynb`, `agentic_rag.ipynb` : notebooks de démonstration et d'exploration.
- `.env.example` : variables d'environnement utiles (`OPENAI_API_KEY`, `SERPER_API_KEY`, `FIRECRAWL_API_KEY`, etc.).
- `pyproject.toml` : spécifie `crewai[tools]` comme dépendance principale et déclare des scripts utiles.

## Fonctionnalités montrées par le code

- Indexation d'un PDF en mémoire et recherche par similarité (outil `DocumentSearchTool`).
- Flux multi‑agent via CrewAI : un agent récupérateur (retriever) et un agent synthétiseur (response synthesizer).
- Possibilité d'utiliser un LLM local via `LLM` (exemples : `ollama/deepseek-r1`, `ollama/llama3.2`) pour `app_deep_seek.py` et `app_llama3.2.py`.
- Recherche web via `SerperDevTool` ou `FireCrawlWebSearchTool` (la disponibilité dépend de vos clés API et de la présence/implémentation de ces outils).

## Prérequis

- Python 3.10+ (pyproject indique >=3.10)
- Node.js si vous comptez ajouter ou lancer un frontend séparé (non fourni ici)
- Optionnel : Ollama ou autre endpoint LLM local si vous utilisez `app_deep_seek.py` / `app_llama3.2.py`.
- Variables d'environnement (voir `.env.example`) :

```dotenv
SERPER_API_KEY=your_serper_api_key
FIRECRAWL_API_KEY=your_firecrawl_api_key
OPENAI_API_KEY=your_openai_api_key  # si besoin
```

## Installation rapide

1. Créez et activez un environnement virtuel Python.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

2. Installer les dépendances (exemple minimal à adapter) :

```bash
pip install "crewai[tools]>=0.86.0,<1.0.0" markitdown chonkie qdrant-client streamlit uvicorn
```

Remarque : `pyproject.toml` déclare `crewai[tools]`; adaptez l'installation selon votre gestionnaire (pip/hatch).

## Lancement des applications Streamlit

- Interface générique (utilise `SerperDevTool`) :

```bash
streamlit run app.py
```

- Variante avec LLM local `deepseek-r1` :

```bash
streamlit run app_deep_seek.py
```

- Variante avec LLM local `llama3.2` :

```bash
streamlit run app_llama3.2.py
```

Dans les apps Streamlit :
- Uploadez un PDF via la sidebar — l'application indexe le document avec `DocumentSearchTool` puis vous pouvez poser des questions (chat).
- Si vous utilisez une version LLM locale, assurez‑vous que l'endpoint (`http://localhost:11434`) est bien accessible.

## Exécution via `pyproject` (CLI)

Le `pyproject.toml` contient des scripts utiles que vous pouvez appeler après installation du paquet :

```bash
python -m pip install -e .
agentic_rag    # exécute agentic_rag.main:run
```

Ou lancer directement les fonctions :

```bash
python -m src.agentic_rag.main run
```

## Note sur `DocumentSearchTool`

L'outil `src/agentic_rag/tools/custom_tool.py` :
- extrait le texte du PDF via `MarkItDown`;
- découpe en chunks sémantiques via `chonkie` (embedding `minishlab/potion-base-8M` dans le code);
- stocke temporairement les vecteurs dans un client Qdrant en mémoire (`QdrantClient(":memory:")`).

Cet outil est prévu pour des expérimentations locales et des petits PDF ; adaptez la stratégie de stockage/embeddings pour de la production.

## Contribution

- Issues et PR bienvenues. Pour proposer des améliorations, abordez :
   - ajouter un `requirements.txt` complet,
   - implémenter un backend Qdrant persistant,
   - ajouter des tests unitaires et des notebooks de reproduction.

---

Si vous voulez que je mette à jour ce README avec des captures d'écran depuis `assets/`, que j'ajoute un `requirements.txt` exact ou que j'écrive des instructions Windows plus détaillées (ex. commandes PowerShell prêtes à copier), dites‑moi lesquelles et je m'en occupe.

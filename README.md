# InfraChat 🤖

An AI-powered DevOps assistant built with RAG pipeline, LangChain, and FastAPI.

## 🌐 Live Demo
[Click here to use InfraChat](https://YOUR_NETLIFY_URL.netlify.app)

## 🛠️ Tech Stack
- **Frontend:** React
- **Backend:** FastAPI (Python)
- **AI:** LangChain + Groq LLM
- **RAG:** ChromaDB Vector DB
- **Embeddings:** Sentence Transformers
- **Containers:** Docker + Docker Compose
- **CI/CD:** GitHub Actions
- **Hosting:** Hugging Face (backend) + Netlify (frontend)

## 💬 What can you ask?
- Docker commands and troubleshooting
- Kubernetes pod issues
- CI/CD pipeline setup
- Azure cloud services
- Linux commands
- DevOps best practices

## 🚀 Run locally
```bash
git clone https://github.com/shrutika1293/InfraChat.git
cd InfraChat
docker compose up
```

## 📁 Project Structure
```
InfraChat/
├── backend/
│   ├── main.py
│   ├── rag.py
│   ├── requirements.txt
│   └── docs/
├── frontend/
│   └── src/App.js
├── docker-compose.yml
├── Jenkinsfile
└── .github/workflows/deploy.yml
```

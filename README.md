Featured Project
# Marriyam's Bot 🤖
### A RAG-Powered Portfolio Assistant — Built from Scratch

> **"your portfolio has a CV. mine has a conversation."**

[![Live Demo](https://img.shields.io/badge/Live%20Demo-v0--marriyam--portfolio.vercel.app-blue?style=for-the-badge)](https://v0-marriyam-portfolio.vercel.app)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Railway](https://img.shields.io/badge/Railway-0B0D0E?style=for-the-badge&logo=railway&logoColor=white)](https://railway.app)
[![Groq](https://img.shields.io/badge/Groq-F55036?style=for-the-badge)](https://groq.com)

---

## What is this?

**Marriyam's Bot** is a fully custom RAG (Retrieval-Augmented Generation) powered chatbot embedded directly on my portfolio. Visitors can ask it anything, my projects, skills, background, or internship availability — and it responds with accurate, grounded answers pulled from my personal knowledge base.

No hallucinations. No made-up answers. Just real information, retrieved intelligently.

---

## Live Demo

👉 Visit **[v0-marriyam-portfolio.vercel.app](https://v0-marriyam-portfolio.vercel.app)** and click the chat bubble in the bottom right corner!

**Try asking:**
- *"What projects has Marriyam built?"*
- *"One project you'd say was unique?"*
- *"Is she available for internships?"*
- *"What's her tech stack?"*
- *"Tell me about her AI Disease Chatbot"*

---

## Architecture

```
User Question
      ↓
Intent Classifier (Groq — Llama 3.3 70B)
      ↓
Router → picks correct KB section
      ↓
FAISS Vector Search
      ↓
Context Assembly
      ↓
LLM Answer Generation (Groq — Llama 3.3 70B)
      ↓
Response
```

### How it works — step by step:

1. **Knowledge Base** — A structured `.txt` file containing Marriyam's projects, background, skills, goals, and stories — written with rich context, not just bullet points.

2. **Chunking** — The knowledge base is split into overlapping chunks of ~300 words with 50-word overlap to preserve context across boundaries.

3. **Embedding** — Each chunk is converted into a 384-dimensional vector using a hash-based embedding function (lightweight, no heavy ML dependencies needed for deployment).

4. **FAISS Index** — All vectors are stored in a FAISS `IndexFlatL2` index for fast similarity search at query time.

5. **Intent Classification** — Before retrieval, the user's question is classified into one of four intents: `projects`, `background`, `goals`, or `general`. This allows smarter routing and more contextually appropriate responses.

6. **Retrieval** — The user's question is embedded using the same function, and the top-3 most semantically similar chunks are retrieved from FAISS.

7. **Answer Generation** — Retrieved chunks are passed as context to Groq's Llama 3.3 70B model, which generates a grounded, friendly response based only on that context.

8. **GitHub Live Fetch** — A separate `/github` endpoint fetches Marriyam's live GitHub repos in real time via the GitHub API.

---

## Tech Stack

| Layer | Technology |
|---|---|
| **Backend** | Python, FastAPI |
| **Vector Search** | FAISS (faiss-cpu) |
| **LLM** | Groq API — Llama 3.3 70B |
| **Embeddings** | Custom hash-based (NumPy) |
| **Deployment** | Railway |
| **Frontend** | HTML, CSS, JavaScript |
| **Portfolio Hosting** | Vercel |

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/` | Health check |
| `POST` | `/ask` | Main RAG endpoint — takes a question, returns answer + intent |
| `GET` | `/github` | Live GitHub repo fetch |

### Example Request
```json
POST /ask
{
  "question": "What projects has Marriyam built?"
}
```

### Example Response
```json
{
  "answer": "Marriyam has built several projects including Skilline (a RAG-powered...",
  "intent": "projects"
}
```

---

## Key Design Decisions

**Why FAISS over ChromaDB?**
FAISS is lighter, faster, and has no external dependencies — perfect for a portfolio bot that needs to deploy quickly and cheaply.

**Why hash-based embeddings?**
Sentence-transformers (the standard choice) pulls in PyTorch — a 2GB+ dependency that exceeds Railway's free tier build limit. The custom hash-based embedder is ~0KB of additional dependencies and works well for a focused, structured knowledge base like this one.

**Why Groq?**
Groq's inference speed is exceptional — two LLM calls (intent classification + answer generation) complete in under 2 seconds. For a portfolio bot where first impressions matter, speed is UX.

**Why a separate intent classifier?**
Routing questions before retrieval means the system prompt can be tailored per intent — a `goals` question gets a response that mentions internship availability, a `projects` question gets technical depth. One-size-fits-all prompting produces mediocre results.

---

## What I Learned Building This

- End-to-end RAG pipeline design and implementation
- Vector embeddings and similarity search with FAISS
- FastAPI backend architecture and deployment
- CORS configuration for cross-origin API calls
- Railway deployment — debugging build size limits, module paths, environment variables
- The real bottleneck in RAG isn't the LLM — it's the quality of the knowledge base

---

## About the Builder

**Marriyam Andeel** — BS Artificial Intelligence student at COMSATS University Islamabad, Lahore Campus. Interested in machine consciousness, AI reliability, and robotics.

- 🌐 Portfolio: [v0-marriyam-portfolio.vercel.app](https://v0-marriyam-portfolio.vercel.app)
- 💼 LinkedIn: [linkedin.com/in/marriyam-andeel](https://linkedin.com/in/marriyam-andeel)
- 🐙 GitHub: [github.com/MARRIYAM07](https://github.com/MARRIYAM07)
- 📧 Email: marriyamandeel07@gmail.com

---

*Backend code is private to protect system prompt logic and architecture details. This repo serves as the public-facing documentation and showcase.*

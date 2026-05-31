# Complete End-to-End RAG Engine

A learning project an async RAG pipeline built with FastAPI, ChromaDB, and Google Gemini.

Ask it a question, it retrieves relevant context from a vector store and generates a grounded answer using only that context no hallucinated facts.

---

## How It Works

1. Incoming requests are validated by **Pydantic v2** before touching any logic
2. The query is matched against text chunks stored in **ChromaDB** using semantic similarity
3. Matching context is injected into a structured **Gemini** prompt
4. A grounded answer is returned — the model is instructed to stay within the provided context

```
User Question
     │
     ▼
[Pydantic Validation] ──✗──▶ 422 Error (malformed request)
     │
     ▼
[ChromaDB Similarity Search]
     │
     ▼
[Gemini Prompt + Context]
     │
     ▼
Grounded Answer
```

---

## Stack

| Layer | Technology |
|---|---|
| API & routing | FastAPI + Uvicorn |
| Request validation | Pydantic v2 |
| Vector store | ChromaDB |
| Embeddings | LangChain (DeterministicFakeEmbedding) |
| LLM | Google Gemini via LangChain |
| Config | python-dotenv |

---

## Quickstart

### Prerequisites

- Python 3.10+
- A Gemini API key from [Google AI Studio](https://aistudio.google.com/)

### 1. Clone the repo

```bash
git clone https://github.com/nupoork8/complete-end-to-end-RAG-engine.git
cd complete-end-to-end-RAG-engine
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Add your API key

Create a `.env` file in the root directory:

```env
GEMINI_API_KEY=your_key_here
```

### 4. Run the server

```bash
uvicorn main:app --reload --port 8080
```

Visit `http://127.0.0.1:8080/docs` for the interactive Swagger UI.

---

## API

### `POST /api/v1/chat`

```json
{
  "question": "What is RAG used for?"
}
```

**Response:**

```json
{
  "success": true,
  "engine": "RAG-Pipeline-v1",
  "user_question": "What is RAG used for?",
  "retrieved_context": "...",
  "llm_generated_answer": "..."
}
```

### `GET /health`

```json
{ "status": "ok", "engine": "RAG-Pipeline-v1" }
```

---

## Known Limitations

This was a learning project, not a production system. Known issues I'd fix in a v2:

- **In-memory vector store** — ChromaDB resets on every server restart. A persistent client (`chromadb.PersistentClient`) would fix this.
- **Fake embeddings** — uses `DeterministicFakeEmbedding` instead of a real model. Swap for `text-embedding-3-small` or a sentence-transformers model for actual semantic search.
- **Single document source** — knowledge base is hardcoded. A real system would ingest documents dynamically via an upload endpoint.
- **No authentication** — the API is open. Would add API key middleware before exposing publicly.

---

## What I Learned

- How RAG actually works end-to-end, not just conceptually
- Why input validation at the API boundary matters (Pydantic stops bad data before it reaches the DB)
- The difference between a working prototype and a production-ready system

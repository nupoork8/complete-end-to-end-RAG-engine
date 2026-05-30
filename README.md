# Complete End-to-End Async RAG Orchestration Engine

A production-ready, asynchronous backend microservice engineered in Python using FastAPI. This engine is architected to handle unstructured data ingestion, localized semantic vector space persistence, and defensive client-side request validation.

---
<img width="1553" height="843" alt="image" src="https://github.com/user-attachments/assets/001407d1-300d-4ee8-9702-786884496088" />
<img width="1534" height="849" alt="image" src="https://github.com/user-attachments/assets/4015667b-376d-4ee0-8758-315af2bdc55b" />


## 🏗️ System Architecture

The microservice utilizes a decoupled, 4-layer architecture designed to maximize server throughput and enforce strict data isolation:

1. **API / Transport Layer (FastAPI & Uvicorn):** Manages high-concurrency client connections completely asynchronously, preventing thread blocks during upstream network I/O latency.
2. **Data Firewall Layer (Pydantic v2):** Intercepts incoming payloads at the root gateway to perform type-checking and validation, filtering out malformed requests before database processing.
3. **Vector Retrieval Layer (ChromaDB):** Manages localized vector space database persistence, text chunk embeddings, and low-latency semantic context lookups.
4. **Upstream Orchestration Layer (Google Gemini API):** Executes synthesized context injections wrapped inside strict system instructions to guarantee accurate processing and prevent hallucinations.

---

## 🛠️ Defensive Engineering Highlights

- **Asynchronous Task Architecture:** Implemented full `async/await` handling across network bounds to optimize token-processing workflows.
- **Environment Isolation Baseline:** Configured runtime security using `os.getenv` to securely pull upstream API credentials directly from system RAM, preventing hardcoded leaks in public source control.
- **Resilient Network Handling:** Overcame local operating system socket blocks (`WinError`) during continuous local server restarts by binding custom, non-conflicting port parameters.

---


## 🚀 Quickstart Guide

### Prerequisites

- Python 3.10 or higher
- A valid Gemini API Key from [Google AI Studio](https://aistudio.google.com/)

### 1. Clone & Setup Environment

```bash
git clone https://github.com/nupoork8/complete-end-to-end-RAG-engine.git
cd complete-end-to-end-RAG-engine
```

### 2. Install Dependencies

Run the unified package installer to configure your environment:

```bash
pip install -r requirements.txt
```

### 3. Set Up API Credentials

Create a `.env` file in the root directory:

```env
GEMINI_API_KEY=your_secret_gemini_api_key_here
```

### 4. Boot the Microservice Server

Launch the asynchronous Uvicorn server bound to port 8080:

```bash
uvicorn main:app --reload --port 8080
```

Once initialized, navigate to `http://127.0.0.1:8080/docs` in your browser to access the automated, interactive Swagger API documentation panel.

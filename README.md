# DocuMind: Local RAG Chatbot

## 📌 Project Overview
**DocuMind** is a production-ready, fully local Retrieval-Augmented Generation (RAG) Document Q&A Chatbot. It allows users to seamlessly upload PDF documents and ask questions about their content. The entire system runs locally via Docker and Ollama, guaranteeing 100% data privacy and zero API costs.

## 🚀 Key Features
- **Privacy-First AI:** All data processing, embedding, and inference happen locally on the host machine. No data is sent to external APIs (like OpenAI or Anthropic).
- **Real-Time Streaming:** Features a ChatGPT-like experience with real-time text streaming using Server-Sent Events (SSE).
- **Source Citations:** AI responses include exact source document tracking and page references, ensuring high reliability and reducing hallucinations.
- **Premium UI/UX:** A stunning, responsive React frontend featuring a dark theme, glassmorphism elements, and smooth micro-animations.
- **Containerized Architecture:** Fully dockerized (frontend and backend) for consistent deployment and reproducibility.

## 🛠️ Technology Stack
### Frontend
- **Framework:** React 19 + Vite
- **Styling:** Custom CSS with Glassmorphism and CSS Variables
- **Features:** Drag-and-drop file uploads, real-time SSE chat streaming, animated typing indicators.

### Backend
- **Framework:** FastAPI (Python 3.11)
- **API Server:** Uvicorn
- **Concurrency:** Fully asynchronous endpoints (`async/await`) for non-blocking processing.

### AI & RAG Pipeline
- **Orchestration:** LangChain & LangChain Community
- **LLM Engine:** Ollama
- **Generation Model:** Mistral 7B (`mistral`)
- **Embedding Model:** Nomic Embed Text (`nomic-embed-text`)
- **Vector Database:** ChromaDB (Persistent local storage)
- **Document Processing:** `PyPDFLoader` for extraction, `RecursiveCharacterTextSplitter` (chunk size: 512, overlap: 64) for semantic chunking.

## 🏗️ System Architecture Workflow
1. **Document Ingestion:** User drags and drops PDF files into the UI.
2. **Parsing & Chunking:** The FastAPI backend extracts text from the PDFs and splits it into manageable, overlapping chunks.
3. **Embedding:** Each chunk is passed to the local `nomic-embed-text` model to create high-quality vector embeddings.
4. **Indexing:** Embeddings are stored persistently in ChromaDB.
5. **Retrieval:** When the user asks a question, ChromaDB uses Maximum Marginal Relevance (MMR) to fetch the top most relevant chunks.
6. **Generation:** The context and the question are fed into the `mistral` LLM, which streams the answer back to the frontend chunk-by-chunk via SSE.

## ⚙️ Running the Project
Ensure **Docker Desktop** and **Ollama** are running, and the required models are pulled:
```bash
ollama pull mistral
ollama pull nomic-embed-text
```

Then, start the application via Docker Compose:
```bash
docker-compose up -d --build
```

- **Frontend UI:** `http://localhost:5173`
- **Backend API Docs (Swagger):** `http://localhost:8000/docs`

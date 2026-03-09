# 🚑 Lifeline-Core Backend

AI-powered real-time emergency call triage and dispatch system

Lifeline-Core is an intelligent backend system designed to analyze emergency calls in real time, extract critical information, determine incident severity, and assist dispatchers with structured incident summaries and recommended actions.

The system integrates speech processing, AI workflows, and real-time streaming to support emergency response services.

---

# 📌 Features

- Real-time audio streaming from emergency callers  
- Voice Activity Detection (VAD) to process speech segments  
- Speech-to-Text (STT) using Sarvam AI with Deepgram fallback  
- AI-based incident analysis using LangGraph workflow  
- Severity scoring and triage classification  
- Follow-up question generation for incomplete reports  
- Real-time dispatcher dashboard updates via WebSocket  
- Text-to-Speech responses to callers  

---

# 🏗 System Architecture

```
Caller Audio ──► WebSocket /api/stream/audio
                   │
                   ├─ VAD (Voice Activity Detection)
                   ├─ STT (Sarvam AI / Deepgram fallback)
                   ├─ Severity Scoring (rule-based)
                   │
                   ▼
              LangGraph AI Workflow
              ┌─────────────┐
              │   Analyst    │
              │      ▼       │
              │ Completeness │
              │      ▼       │
              │  Summarizer  │
              │    /    \    │
              │   ▼      ▼   │
              │ Question Router│
              └─────────────┘
                   │
                   ├──► Dispatcher Dashboard
                   └──► TTS → Caller
```

---

# ⚙️ Tech Stack

## Backend
- Python 3.11  
- FastAPI  
- Uvicorn  

## AI & LLM Framework
- LangGraph  
- LangChain  
- Ollama  
- Qwen3 LLM  

## Speech Processing
- Sarvam AI (STT + TTS)  
- Deepgram (Fallback STT)  

## Vector Database
- ChromaDB  

## Streaming & Communication
- WebSockets  
- REST APIs  

## Frontend
- React  
- TypeScript  
- Vite  

## DevOps & Infrastructure
- Docker  
- Virtual Environment (venv)  

---

# 📂 Project Structure

```
lifeline-core/
│
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── api/
│   │   ├── services/
│   │   ├── workflows/
│   │   └── models/
│   │
│   ├── tests/
│   ├── ingest_knowledge.py
│   ├── requirements.txt
│   └── Dockerfile
│
├── frontend/
│   ├── src/
│   ├── components/
│   ├── pages/
│   └── vite.config.ts
│
└── README.md
```

---

# ⚡ Prerequisites

Before running the project, ensure you have:

- Python 3.11+
- Node.js
- Ollama installed
- Sarvam AI API Key

Optional:
- Deepgram API Key

---

# 🚀 Quick Start

## 1️⃣ Start Ollama

```
ollama serve
ollama pull qwen3:8b
```

---

## 2️⃣ Setup Backend

```
cd backend
python -m venv venv
```

### Activate environment

**Windows**
```
venv\Scripts\activate
```

**Linux / Mac**
```
source venv/bin/activate
```

### Install dependencies

```
pip install -r requirements.txt
```

---

## 3️⃣ Configure Environment

```
cp .env.example .env
```

Edit `.env` and add your API keys:

```
SARVAM_API_KEY=
DEEPGRAM_API_KEY=
OLLAMA_MODEL=qwen3
```

---

## 4️⃣ Ingest Knowledge Base

Run once to load protocols into the vector database.

```
python ingest_knowledge.py
```

---

## 5️⃣ Run the Backend Server

```
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Server will run at:

```
http://localhost:8000
```

---

# 📡 API Endpoints

| Method | Endpoint | Description |
|------|------|------|
| GET | /api/health | Health check |
| POST | /api/call/message | Send caller transcript |
| GET | /api/call/question/{session_id} | Get follow-up question |
| GET | /api/call/question/{session_id}/audio | Stream TTS |
| GET | /api/call/status/{session_id} | Incident status |
| GET | /api/call/sessions | Active sessions |
| DELETE | /api/call/{session_id} | End call session |

---

# 🔊 WebSocket Endpoints

| Endpoint | Description |
|------|------|
| /api/stream/audio?session_id=X | Real-time audio ingestion |
| /api/dispatcher/ws | Live dispatcher updates |

---

# 🧪 Testing

Run backend tests:

```
cd backend
pytest tests/ -v
```

---

# 🐳 Docker Deployment

### Build image

```
docker build -t lifeline-core .
```

### Run container

```
docker run -p 8000:8000 --env-file .env lifeline-core
```

---

# 🎯 Example API Request

### Send caller message

```
curl -X POST http://localhost:8000/api/call/message \
-H "Content-Type: application/json" \
-d '{"session_id": "call_123", "message": "There is a fire at the main market!"}'
```

### Get call status

```
curl http://localhost:8000/api/call/status/call_123
```

---

# 🔮 Future Improvements

- Multi-language emergency detection  
- GIS location tracking  
- Ambulance route optimization  
- AI incident prediction  
- Integration with public emergency services  

---

# 👨‍💻 Contributors

Farheen Banu  
Lifeline AI Project Team

---

# 📜 License

This project is licensed under the MIT License.

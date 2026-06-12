# 🤖 RAG Chatbot — n8n + Supabase + Google Drive

A Retrieval-Augmented Generation (RAG) chatbot that automatically ingests documents and answers questions using only your document content.

## 🔗 Live Demo

| | Link |
|---|---|
| 💬 **Chat with the bot** | [Open Chatbot](https://maithri-2007.app.n8n.cloud/webhook/1228d592-f85b-408f-a8cb-9b4c84163873/chat) |
| 📤 **Upload documents** | [Upload Page](https://Maithri04.github.io/n8n1/upload.html) |

---
<img width="1089" height="564" alt="Screenshot 2026-06-12 143659" src="https://github.com/user-attachments/assets/8ea9c7e3-0703-41cb-ac65-9c361a27bce2" />
<img width="1729" height="763" alt="Screenshot 2026-06-12 143735" src="https://github.com/user-attachments/assets/b2fcfbd0-03be-402f-ba4a-7215b8d105d5" />
<img width="1021" height="503" alt="Screenshot 2026-06-12 143759" src="https://github.com/user-attachments/assets/f78ecaa5-5285-48ad-b074-1d7996e83bbe" />

## Architecture

```
📁 Google Drive (auto-ingest)        🌐 Upload Page (manual upload)
        ↓                                      ↓
Workflow 1: RAG Ingestion            Workflow 3: File Upload Webhook
  - Google Drive Trigger               - Webhook (POST)
  - Download File                      - Default Data Loader
  - Default Data Loader                - Recursive Text Splitter
  - Recursive Text Splitter            - Gemini Embeddings
  - Gemini Embeddings                  - Supabase Vector Store
  - Supabase Vector Store              - Respond to Webhook
        ↓                                      ↓
              Supabase (documents table — pgvector)
                            ↑
              Workflow 2: Chat Agent
                - Chat Trigger
                - AI Agent (Groq LLM)
                - Simple Memory
                - Supabase Vector Store (retrieve as tool)
                - Gemini Embeddings
                            ↓
                      💬 Chat Response
```

---

## Tech Stack

| Component | Tool |
|-----------|------|
| Workflow Automation | [n8n](https://n8n.io) |
| Vector Database | [Supabase](https://supabase.com) (pgvector) |
| Embeddings | Google Gemini (`gemini-embedding-2`) |
| LLM | Groq (`qwen/qwen3-32b`) |
| Document Source | Google Drive + Upload Page |
| Frontend | Vanilla HTML/CSS/JS |

---

## Workflows

| Workflow | Description |
|----------|-------------|
| `rag_ingestion.json` | Auto-ingests files from Google Drive |
| `chat_workflow.json` | Chat agent that answers from knowledge base |
| `file_upload_webhook.json` | Webhook to receive uploaded files from the web page |

---

## Setup Instructions

### 1. Supabase Setup
- Create a new Supabase project
- Go to **SQL Editor** and run `schema.sql`

### 2. API Keys Required

| Service | Where to get |
|---------|-------------|
| Groq API Key | [console.groq.com](https://console.groq.com) |
| Google Gemini API Key | [aistudio.google.com](https://aistudio.google.com) |
| Supabase URL + Key | Your Supabase project settings |
| Google Drive OAuth2 | Google Cloud Console |

### 3. n8n Setup
- Install [n8n](https://docs.n8n.io/hosting/) or use [n8n Cloud](https://app.n8n.io)
- Import all 3 workflow JSON files from the `workflows/` folder
- Add credentials for Google Drive, Supabase, Groq, and Gemini
- Publish all 3 workflows

### 4. Upload Page
- Deploy `upload.html` to GitHub Pages or any static host
- Update the `WEBHOOK_URL` in `upload.html` to your own n8n webhook URL

### 5. Usage
1. Upload `.txt`, `.pdf`, or `.docx` files via the upload page
2. Files are automatically embedded into the knowledge base
3. Open the chatbot and ask questions about your documents!

---

## Supported Document Formats
- `.txt` ✅
- `.pdf` ✅
- `.docx` ✅
- `.md` ✅

---

## Notes
- Gemini `gemini-embedding-2` produces **3072-dimensional** vectors
- Simple Memory stores chat history in n8n (not persistent across restarts). For production, use Postgres or Redis memory
- n8n free tier allows 1000 executions/month

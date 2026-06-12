# RAG Chatbot with n8n, Supabase & Google Drive

A Retrieval-Augmented Generation (RAG) chatbot that automatically ingests documents from Google Drive and answers questions using only your document content.

## Architecture

```
Google Drive (upload document)
        ↓
Workflow 1: RAG Ingestion
  - Google Drive Trigger
  - Download File
  - Default Data Loader + Text Splitter
  - Gemini Embeddings
  - Supabase Vector Store (insert)
        ↓
  Supabase (documents table)
        ↑
Workflow 2: Chat Agent
  - Chat Trigger
  - AI Agent (Groq LLM)
  - Simple Memory
  - Supabase Vector Store (retrieve as tool)
  - Gemini Embeddings
```

## Tech Stack

| Component | Tool |
|-----------|------|
| Workflow Automation | [n8n](https://n8n.io) |
| Vector Database | [Supabase](https://supabase.com) (pgvector) |
| Embeddings | Google Gemini (`gemini-embedding-2`) |
| LLM | Groq (`qwen/qwen3-32b`) |
| Document Source | Google Drive |

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
- Import both workflow JSON files from the `workflows/` folder:
  - `rag_ingestion.json`
  - `chat_workflow.json`
- Add credentials for Google Drive, Supabase, Groq, and Gemini
- Update the Google Drive folder ID in the ingestion workflow
- Activate both workflows

### 4. Usage
1. Upload `.txt`, `.pdf`, or `.docx` files to your watched Google Drive folder
2. Workflow 1 automatically ingests and embeds the documents
3. Open the chat interface in Workflow 2 and ask questions!

## Supported Document Formats
- `.txt` ✅
- `.pdf` ✅
- `.docx` ✅
- `.md` ✅

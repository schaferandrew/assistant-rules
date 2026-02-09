# Chat History Setup Guide

## Prerequisites

1. PostgreSQL installed and running
2. Create a database named `vivian`

## Setup Steps

### 1. Install Backend Dependencies

```bash
cd vivian-backend/apps/api
pip install sqlalchemy psycopg2-binary
```

### 2. Configure Database URL

Create or update `.env` file in `vivian-backend/apps/api/`:

```env
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/vivian
```

### 3. Run Database Migration

```bash
cd vivian-backend
python scripts/migrate.py
```

This will create the `chats` and `chat_messages` tables.

### 4. Start the Backend

```bash
cd vivian-backend/apps/api
python -m vivian_api.main
```

### 5. Start the Frontend

```bash
cd vivian-client
npm run dev
```

## Features Implemented

### Backend (FastAPI)
- PostgreSQL database connection (`apps/api/vivian_api/db/database.py`)
- Chat and ChatMessage models (`apps/api/vivian_api/models/chat_models.py`)
- CRUD operations (`apps/api/vivian_api/crud/chat_crud.py`)
- API endpoints (`apps/api/vivian_api/chat/history_router.py`):
  - `GET /api/v1/chats` - List all chats
  - `POST /api/v1/chats` - Create new chat
  - `GET /api/v1/chats/{id}` - Get chat with messages
  - `DELETE /api/v1/chats/{id}` - Delete chat
  - `PATCH /api/v1/chats/{id}/title` - Update title
  - `POST /api/v1/chats/{id}/generate-summary` - Generate AI summary

### Frontend (Next.js)
- Sidebar with collapsible design (60px collapsed / 260px expanded)
- Chat history list with titles and summaries
- User profile section at bottom
- Two-row chat input with tool buttons (web search, attachments, MCP connectors)
- Auto-expanding textarea
- New chat button in sidebar
- State management for chat history
- Proxy API routes for chat CRUD operations

### AI Summary Generation
Uses Llama 3.3 70B (free model via OpenRouter) to generate:
- 3-5 word titles from first message
- Brief summaries of conversation

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `DATABASE_URL` | PostgreSQL connection string | `postgresql://postgres:postgres@localhost:5432/vivian` |
| `OPENROUTER_API_KEY` | OpenRouter API key | Required for AI features |

## Database Schema

```sql
CREATE TABLE chats (
    id UUID PRIMARY KEY,
    user_id VARCHAR(255) NOT NULL,
    title VARCHAR(500) NOT NULL,
    summary TEXT,
    model VARCHAR(100),
    created_at TIMESTAMP,
    updated_at TIMESTAMP
);

CREATE TABLE chat_messages (
    id UUID PRIMARY KEY,
    chat_id UUID REFERENCES chats(id) ON DELETE CASCADE,
    role VARCHAR(20) NOT NULL,
    content TEXT NOT NULL,
    timestamp TIMESTAMP,
    metadata JSONB
);
```

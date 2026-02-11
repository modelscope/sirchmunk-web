---
title: Web UI
weight: 4
---

Sirchmunk's Web UI provides a modern interface for real-time chat, knowledge analytics, and system monitoring.

## Setup

### Option 1: Single-Port Mode (Recommended)

Build the frontend once, then serve everything from a single port — no Node.js needed at runtime.

```bash
# Build WebUI frontend (requires Node.js 18+ at build time)
sirchmunk web init

# Start server with embedded WebUI
sirchmunk web serve
```

**Access:** http://localhost:8584 (API + WebUI on the same port)

### Option 2: Development Mode

For frontend development with hot-reload:

```bash
# Start backend + Next.js dev server
sirchmunk web serve --dev
```

**Access:**
- Frontend (hot-reload): http://localhost:8585
- Backend APIs: http://localhost:8584/docs

### Option 3: Legacy Script

```bash
# Start frontend and backend
python scripts/start_web.py

# Stop all services
python scripts/stop_web.py
```

## Features

### Home — Chat Interface

The home page provides a streaming chat interface with:

- **Inline search logs** — See exactly how Sirchmunk finds answers
- **Source citations** — Every claim linked back to its source file
- **Multiple chat modes:**
  - Pure LLM conversation
  - Conversation augmented with file-based RAG
  - Web-augmented search
- **Session management** — Save and resume conversations

### Knowledge — Cluster Analytics

Browse and manage knowledge clusters:

- View cluster details, evidence units, and confidence scores
- Track lifecycle states (Emerging → Stable → Deprecated)
- Monitor hotness scores and query histories

### Monitor — System Dashboard

Real-time system health and usage metrics:

- Chat activity and session statistics
- LLM token usage and cost tracking
- Knowledge cluster growth over time
- System resource monitoring

### Settings

Configure Sirchmunk through the web interface:

- LLM API key, base URL, and model selection
- Search parameters (depth, top-k, keyword levels)
- Environment variable management

## Theming

The Web UI supports:

- **Dark / Light themes** — Toggle via the theme switch
- **Bilingual localization** — English and Chinese

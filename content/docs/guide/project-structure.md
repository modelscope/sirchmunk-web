---
title: Project Structure
weight: 1
---

An overview of Sirchmunk's source code organization and architectural layers.

## Repository Layout

```text
sirchmunk/
├── src/
│   ├── sirchmunk/              # Core Python package
│   │   ├── agentic/            # ReAct agent, tools, prompts
│   │   ├── api/                # FastAPI REST endpoints
│   │   │   └── components/     # History, monitor, settings storage
│   │   ├── cli/                # CLI entry point and web launcher
│   │   ├── insight/            # Text insight extraction
│   │   ├── learnings/          # Evidence processing, knowledge base
│   │   ├── llm/                # LLM interface (OpenAI-compatible)
│   │   ├── retrieve/           # Indexless retrieval engine
│   │   ├── scan/               # Directory and file scanners
│   │   ├── schema/             # Data models (knowledge, context, etc.)
│   │   ├── storage/            # DuckDB + Parquet persistence
│   │   ├── utils/              # Utilities (embedding, tokenizer, etc.)
│   │   ├── search.py           # Main search orchestrator
│   │   └── base.py             # Base classes and abstractions
│   └── sirchmunk_mcp/          # MCP server package
│       ├── server.py           # MCP server implementation
│       ├── service.py          # Search service layer
│       └── tools.py            # MCP tool definitions
├── web/                        # Next.js 14 frontend
│   ├── app/                    # Pages (home, history, knowledge, monitor, settings)
│   ├── components/             # React components
│   ├── hooks/                  # Custom React hooks
│   └── lib/                    # Utilities (API, i18n, theme)
├── config/                     # Configuration templates
├── scripts/                    # Helper scripts
└── requirements/               # Dependency files by feature
```

## Architectural Layers

Sirchmunk follows a strict **Separation of Concerns** pattern with four distinct layers:

### Integration Layer
External surfaces that expose Sirchmunk's intelligence:
- **MCP Server** (`sirchmunk_mcp/`) — AI-to-AI communication
- **REST API** (`api/`) — Programmatic HTTP access
- **WebSocket** — Real-time streaming chat
- **CLI** (`cli/`) — Command-line interface
- **Web UI** (`web/`) — Browser-based interface

### Orchestration Layer
The search pipeline coordinator:
- **AgenticSearch** (`search.py`) — Multi-phase search orchestrator
- **SearchContext** (`schema/search_context.py`) — Budget, state, and audit management

### Intelligence Layer
Evidence extraction and knowledge synthesis:
- **EvidenceProcessor** (`learnings/evidence_processor.py`) — Monte Carlo sampling
- **KnowledgeBase** (`learnings/knowledge_base.py`) — Knowledge cluster management
- **ReActAgent** (`agentic/react_agent.py`) — Autonomous exploration
- **OpenAIChat** (`llm/openai_chat.py`) — Unified LLM interface

### Storage Layer
Persistence and caching:
- **DuckDB** (`storage/duckdb.py`) — In-memory analytical database
- **KnowledgeStorage** (`storage/knowledge_storage.py`) — Parquet-based cluster persistence

## Core Components

| Component | Module | Description |
|-----------|--------|-------------|
| **AgenticSearch** | `search.py` | Search orchestrator with LLM-enhanced retrieval |
| **KnowledgeBase** | `learnings/knowledge_base.py` | Transforms results into structured knowledge clusters |
| **EvidenceProcessor** | `learnings/evidence_processor.py` | Monte Carlo importance sampling for evidence extraction |
| **GrepRetriever** | `retrieve/text_retriever.py` | High-performance indexless file search |
| **DirScanner** | `scan/dir_scanner.py` | Directory structure analysis |
| **ReActAgent** | `agentic/react_agent.py` | Budget-bounded autonomous exploration |
| **OpenAIChat** | `llm/openai_chat.py` | Unified LLM interface with streaming and usage tracking |
| **MonitorTracker** | `api/components/monitor_tracker.py` | Real-time system metrics |

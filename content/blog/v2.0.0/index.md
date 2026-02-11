---
title: "Sirchmunk v0.0.2 Released — MCP Support, CLI & Knowledge Persistence"
summary: "Sirchmunk v0.0.2 brings MCP protocol integration for Claude Desktop and Cursor IDE, a powerful CLI, DuckDB-powered knowledge persistence, and semantic cluster reuse."
date: 2026-02-05
authors:
  - me
tags:
  - Release
  - Sirchmunk
  - MCP
  - CLI
image:
  caption: 'Sirchmunk v0.0.2'
---

We are thrilled to announce **Sirchmunk v0.0.2** — a major step forward with Model Context Protocol (MCP) integration, a comprehensive CLI, and persistent knowledge management.

<!--more-->

## Highlights

### MCP Integration

Full [Model Context Protocol](https://modelcontextprotocol.io) support enables seamless integration with AI assistants like **Claude Desktop** and **Cursor IDE**. Sirchmunk's search capabilities are exposed as MCP tools that any compliant client can discover and invoke.

```json
{
  "mcpServers": {
    "sirchmunk": {
      "command": "sirchmunk",
      "args": ["mcp", "serve"],
      "env": {
        "SIRCHMUNK_SEARCH_PATHS": "/path/to/your_docs"
      }
    }
  }
}
```

The MCP server supports both **stdio** (for local desktop AI tools) and **HTTP** (for remote scenarios) transport modes.

### CLI Commands

A new `sirchmunk` CLI provides a single entry point for all operations:

| Command | Description |
|---------|-------------|
| `sirchmunk init` | Initialize working directory, .env, and MCP config |
| `sirchmunk serve` | Start the backend API server |
| `sirchmunk search` | Perform search queries directly from the terminal |
| `sirchmunk web init` | Build WebUI frontend |
| `sirchmunk web serve` | Start API + WebUI (single port) |
| `sirchmunk mcp serve` | Start the MCP server |

### Knowledge Persistence

DuckDB-powered storage with Apache Parquet export provides efficient knowledge management:

- **In-memory first**: Microsecond-level read/write during runtime
- **Atomic persistence**: Crash-safe writes using the temp-file-then-rename pattern
- **Parquet export**: Columnar format for excellent compression and analytical query performance

### Semantic Cluster Reuse

Knowledge clusters now store embedding vectors (384 dimensions), enabling fast cosine-similarity matching for repeated or paraphrased queries. This delivers **sub-second response times** for queries that match existing knowledge.

## How to Upgrade

```bash
pip install --upgrade sirchmunk
```

Or install with all extras:

```bash
pip install "sirchmunk[all]"
```

## What's Next?

Our roadmap includes:
- Web search integration
- Multi-modal support (images, videos)
- Distributed search across nodes
- Knowledge visualization and deep analytics

---

*[GitHub Repository](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

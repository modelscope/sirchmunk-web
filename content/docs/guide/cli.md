---
title: CLI Commands
weight: 3
---

Sirchmunk provides a comprehensive command-line interface for all operations — initialization, server management, search, and MCP.

## Installation

```bash
# Basic installation (search + CLI)
pip install sirchmunk

# With Web UI support
pip install "sirchmunk[web]"

# With MCP support
pip install "sirchmunk[mcp]"

# Everything
pip install "sirchmunk[all]"
```

## Commands

### `sirchmunk init`

Initialize the working directory, `.env` configuration, and MCP config.

```bash
# Default work path: ~/.sirchmunk/
sirchmunk init

# Custom work path
sirchmunk init --work-path /path/to/workspace
```

### `sirchmunk serve`

Start the backend API server.

```bash
# Default: localhost:8584
sirchmunk serve

# Custom host and port
sirchmunk serve --host 0.0.0.0 --port 8000
```

### `sirchmunk search`

Perform search queries directly from the terminal.

```bash
# Search in current directory
sirchmunk search "How does authentication work?"

# Search in specific paths
sirchmunk search "find all API endpoints" ./src ./docs

# Quick filename search (no LLM required)
sirchmunk search "config" --mode FILENAME_ONLY

# Output as JSON
sirchmunk search "database schema" --output json

# Use API server (requires running server)
sirchmunk search "query" --api --api-url http://localhost:8584
```

### `sirchmunk web init`

Build the WebUI frontend. Requires Node.js 18+ at build time.

```bash
sirchmunk web init
```

### `sirchmunk web serve`

Start API + WebUI from a single port.

```bash
# Production mode (requires prior `web init`)
sirchmunk web serve

# Development mode with hot-reload
sirchmunk web serve --dev
```

### `sirchmunk mcp serve`

Start the MCP server for AI assistant integration.

```bash
# stdio mode (for Claude Desktop, Cursor IDE)
sirchmunk mcp serve

# HTTP mode
sirchmunk mcp serve --transport http --port 3000
```

### `sirchmunk version`

Display version information.

```bash
sirchmunk version
sirchmunk mcp version
```

## Command Reference

| Command | Description |
|---------|-------------|
| `sirchmunk init` | Initialize working directory, .env, and MCP config |
| `sirchmunk serve` | Start the backend API server |
| `sirchmunk search` | Perform search queries |
| `sirchmunk web init` | Build WebUI frontend (requires Node.js 18+) |
| `sirchmunk web serve` | Start API + WebUI (single port) |
| `sirchmunk web serve --dev` | Start API + Next.js dev server (hot-reload) |
| `sirchmunk mcp serve` | Start the MCP server (stdio/HTTP) |
| `sirchmunk mcp version` | Show MCP version information |
| `sirchmunk version` | Show version information |

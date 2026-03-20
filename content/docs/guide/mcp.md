---
title: MCP Integration
weight: 5
---

Sirchmunk provides a [Model Context Protocol (MCP)](https://modelcontextprotocol.io) server that exposes its intelligent search capabilities as MCP tools. This enables seamless integration with AI assistants like **Claude Desktop** and **Cursor IDE**.

## Quick Start

```bash
# Install with MCP support
pip install "sirchmunk[mcp]"

# Initialize (generates .env and mcp_config.json)
sirchmunk init

# Edit ~/.sirchmunk/.env with your LLM API key

# Test with MCP Inspector
npx @modelcontextprotocol/inspector sirchmunk mcp serve
```

## Configuration

After running `sirchmunk init`, a `mcp_config.json` is generated at `~/.sirchmunk/mcp_config.json`. Copy it to your MCP client configuration:

```json
{
  "mcpServers": {
    "sirchmunk": {
      "command": "sirchmunk",
      "args": ["mcp", "serve"],
      "env": {
        "SIRCHMUNK_SEARCH_PATHS": "/path/to/your_docs,/another/path"
      }
    }
  }
}
```

| Parameter | Description |
|-----------|-------------|
| `command` | Path to sirchmunk binary. Use full path for virtual environments. |
| `args` | `["mcp", "serve"]` starts the MCP server in stdio mode. |
| `env.SIRCHMUNK_SEARCH_PATHS` | Default search directories (comma-separated). |

## Transport Modes

### stdio (Default)

For local integration with desktop AI tools:

```bash
sirchmunk mcp serve
```

### HTTP

For remote or networked scenarios:

```bash
sirchmunk mcp serve --transport http --port 3000
```

## Available MCP Tools

### `sirchmunk_search`

Execute an intelligent search query.

**Parameters:**
- `query` (string, required) — The search question
- `paths` (string[], optional) — Directories to search; when omitted, falls back to `SIRCHMUNK_SEARCH_PATHS`, then the current working directory
- `mode` (string, optional) — `FAST` (default), `DEEP`, or `FILENAME_ONLY` (`paths` is optional with the same fallback chain as above)

### `sirchmunk_get_cluster`

Retrieve a specific knowledge cluster by ID.

**Parameters:**
- `cluster_id` (string, required) — The cluster identifier

### `sirchmunk_list_clusters`

List all stored knowledge clusters.

## Search Modes

| Mode | Description | LLM Required |
|------|-------------|:------------:|
| **FAST** | Greedy search with 2-level keyword cascade and early stopping (2-5s, ~10x faster than DEEP) | Yes |
| **DEEP** | Full multi-phase analysis with Monte Carlo evidence sampling (10-30s) | Yes |
| **FILENAME_ONLY** | Filename-based search without content analysis | No |

## OpenClaw Integration

Sirchmunk is available as an [OpenClaw](https://openclaw.org/) skill on [ClawHub](https://clawhub.ai/wangxingjun778/sirchmunk). Any OpenClaw-compatible agent can invoke Sirchmunk's search capability via natural language — no MCP configuration needed. See [openclaw-recipe](https://github.com/modelscope/sirchmunk/tree/main/recipes/openclaw_skills) for setup details.

## Integration with Claude Desktop

1. Install Sirchmunk with MCP support: `pip install "sirchmunk[mcp]"`
2. Run `sirchmunk init` to generate configuration
3. Copy `mcp_config.json` to Claude Desktop's MCP config directory
4. Restart Claude Desktop
5. Sirchmunk search tools will appear in Claude's tool list

## Integration with Cursor IDE

1. Install Sirchmunk with MCP support
2. Run `sirchmunk init`
3. Add the MCP server configuration to Cursor's settings
4. Sirchmunk becomes available as a tool in Cursor's AI chat

> [!NOTE]
> MCP Inspector is a great way to test the integration before connecting to your AI assistant. In MCP Inspector: **Connect** → **Tools** → **List Tools** → `sirchmunk_search` → Input parameters → **Run Tool**.

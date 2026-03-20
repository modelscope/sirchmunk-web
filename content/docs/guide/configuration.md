---
title: Configuration
weight: 2
---

Sirchmunk is configured through environment variables stored in a `.env` file. After running `sirchmunk init`, the configuration file is created at `~/.sirchmunk/.env`.

<!--more-->

## Environment Variables

### LLM Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `LLM_API_KEY` | Your LLM API key (required for FAST and DEEP modes) | — |
| `LLM_BASE_URL` | OpenAI-compatible API base URL | `https://api.openai.com/v1` |
| `LLM_MODEL` | Model name to use | `gpt-4o` |

### Search Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `SIRCHMUNK_WORK_PATH` | Working directory for data storage | `~/.sirchmunk/` |
| `SIRCHMUNK_SEARCH_PATHS` | Default search paths (comma-separated) | — |
| `SIRCHMUNK_MAX_DEPTH` | Maximum directory traversal depth | `10` |
| `SIRCHMUNK_TOP_K_FILES` | Number of top files to analyze | `20` |
| `SIRCHMUNK_MAX_CONCURRENT_SEARCHES` | Max concurrent search tasks | `3` |
| `SIRCHMUNK_ENABLE_CLUSTER_REUSE` | Enable knowledge cluster reuse | `false` |

### Chat Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `CHAT_HISTORY_MAX_TURNS` | Maximum number of chat turns retained in history | — |
| `CHAT_HISTORY_MAX_TOKENS` | Maximum token budget for retained chat history | — |

### Server Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `SIRCHMUNK_HOST` | API server bind address | `127.0.0.1` |
| `SIRCHMUNK_PORT` | API server port | `8584` |

## Data Storage Layout

All persistent data is stored under `SIRCHMUNK_WORK_PATH`:

```text
{SIRCHMUNK_WORK_PATH}/
  ├── .cache/
  │   ├── history/              # Chat session history (DuckDB)
  │   │   └── chat_history.db
  │   ├── knowledge/            # Knowledge clusters (Parquet)
  │   │   └── knowledge_clusters.parquet
  │   └── settings/             # User settings (DuckDB)
  │       └── settings.db
  ├── .env                      # Environment configuration
  └── mcp_config.json           # MCP server configuration
```

## Search Parameters

When invoking search (via SDK, CLI, or API), the following parameters are available:

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `query` | `string` | *required* | Search query or question |
| `paths` | `string \| string[]` | *optional* | Directories or files to search; falls back to `SIRCHMUNK_SEARCH_PATHS`, then cwd |
| `mode` | `string` | `FAST` | `FAST` (greedy, 2-5s), `DEEP` (Monte Carlo, 10-30s), or `FILENAME_ONLY` |
| `max_depth` | `int` | `null` | Maximum directory depth |
| `top_k_files` | `int` | `null` | Number of top files to return |
| `enable_dir_scan` | `bool` | `true` | Enable directory scanning |
| `max_loops` | `int` | `null` | DEEP mode loop limit |
| `max_token_budget` | `int` | `null` | DEEP mode token budget (default 128K when unset) |
| `include_patterns` | `string[]` | `null` | File glob patterns to include |
| `exclude_patterns` | `string[]` | `null` | File glob patterns to exclude |
| `return_context` | `bool` | `false` | Return SearchContext with cluster and telemetry |

> [!NOTE]
> `FILENAME_ONLY` mode does not require an LLM API key. `FAST` and `DEEP` modes require a configured LLM. `FAST` mode uses a greedy strategy with 2-level keyword cascade and early stopping — approximately **10x faster** than `DEEP` mode.

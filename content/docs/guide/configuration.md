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
| `LLM_MODEL_NAME` | Model name to use | `gpt-5.2` |

### Search Configuration

| Variable | Description | Default |
|----------|-------------|---------|
| `SIRCHMUNK_WORK_PATH` | Working directory for data storage | `~/.sirchmunk/` |
| `SIRCHMUNK_SEARCH_PATHS` | Default search paths (comma-separated) | — |
| `SIRCHMUNK_MAX_DEPTH` | Maximum directory traversal depth | `10` |
| `SIRCHMUNK_TOP_K_FILES` | Number of top files to analyze | `20` |
| `SIRCHMUNK_MAX_CONCURRENT_SEARCHES` | Max concurrent search tasks | `3` |
| `SIRCHMUNK_ENABLE_CLUSTER_REUSE` | Enable knowledge cluster reuse | `true` |

### Retrieval Cost Configuration

| Variable | Description | Default |
|----------|-------------|--------|
| `GREP_MAX_FILESIZE_MB` | Per-file size cap (MB); files over this limit are skipped on the query hot path | `64` |
| `GREP_RGA_ADAPTERS` | Allowed rga adapters (bounded only; archives disabled on hot path) | `poppler,pandoc,postprocpagebreaks` |
| `GREP_TIERED_SCAN` | Enable tiered scan: fast native-rg pass + bounded rga pass for rich formats | `true` |
| `GREP_TEXT_TIMEOUT` | Timeout (seconds) for the native rg text pass | `15.0` |
| `GREP_TIMEOUT` | Timeout (seconds) for the rga rich pass | `60.0` |
| `GREP_RICH_EXTENSIONS` | File extensions routed to the rga rich pass | `pdf,docx,epub,odt` |

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
  │   ├── compile/              # Compile artifacts (Beta)
  │   │   ├── manifest.json     # File manifest with hashes
  │   │   ├── document_catalog.json
  │   │   ├── summary_index.json
  │   │   ├── trees/            # Hierarchical tree indices
  │   │   ├── table_digests/    # Table extraction digests
  │   │   └── xlsx_digests/     # Spreadsheet digests
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
| `mode` | `string` | `DEEP` | `DEEP` (agentic retrieval with budgeted evidence exploration), `FAST` (greedy, 2-5s), or `FILENAME_ONLY` |
| `max_depth` | `int` | `null` | Maximum directory depth |
| `top_k_files` | `int` | `null` | Number of top files to return |
| `enable_dir_scan` | `bool` | `true` | Enable directory scanning |
| `max_loops` | `int` | `null` | DEEP mode loop limit |
| `max_token_budget` | `int` | `null` | DEEP mode token budget (default 128K when unset) |
| `include_patterns` | `string[]` | `null` | File glob patterns to include |
| `exclude_patterns` | `string[]` | `null` | File glob patterns to exclude |
| `response_format` | `string` | `rich` | `"rich"` Markdown report, `"minimal"` short answer, `"context"` SearchContext, or `"json"` serialized context |

> [!NOTE]
> `FILENAME_ONLY` mode does not require an LLM API key. `FAST` and `DEEP` modes require a configured LLM. Default mode is `DEEP`, which performs budgeted evidence exploration with multi-path retrieval.

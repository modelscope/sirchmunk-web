---
linkTitle: Reference
title: API Reference
---

Sirchmunk exposes a REST API when running in server mode (`sirchmunk serve` or `sirchmunk web serve`).

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/search` | Execute a search query (JSON response when complete) |
| `POST` | `/api/v1/search/stream` | Same request body; SSE stream of logs plus final result or error |
| `GET` | `/api/v1/search/status` | Check server and LLM configuration (includes `max_concurrent_searches`) |

**Interactive Docs:** When the server is running, visit http://localhost:8584/docs for Swagger UI.

## Search API

### `POST /api/v1/search`

Execute an intelligent search query. The handler returns JSON after the search finishes.

**Request body:** `paths` is optional (see [Request parameters](#request-parameters)).

```json
{
  "query": "How does authentication work?",
  "mode": "FAST",
  "paths": ["/path/to/project"],
  "max_depth": 10,
  "top_k_files": 20,
  "enable_dir_scan": true,
  "max_loops": 8,
  "max_token_budget": 131072,
  "include_patterns": ["*.py", "*.java"],
  "exclude_patterns": ["*test*", "*__pycache__*"],
  "return_context": false
}
```

**Response:**

```json
{
  "success": true,
  "data": {
    "summary": "Markdown-formatted search result...",
    "context": null
  }
}
```

### Request parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `query` | string | **Required.** Natural-language search question. |
| `paths` | string \| string[] | **Optional.** Root path(s) to search. If omitted, the server uses `SIRCHMUNK_SEARCH_PATHS` (if set), otherwise the process current working directory. |
| `mode` | string | Search mode, e.g. `FAST` (default), `DEEP`, `FILENAME_ONLY`. |
| `max_depth` | int | Maximum directory depth. |
| `top_k_files` | int | Cap on files considered. |
| `enable_dir_scan` | bool | Whether to scan directories for candidates; default `true`. |
| `max_loops` | int | **DEEP mode:** maximum agent / reasoning loops. |
| `max_token_budget` | int | **DEEP mode:** token budget for the run; default **128K** (131072). |
| `include_patterns` | string[] | Glob patterns to include. |
| `exclude_patterns` | string[] | Glob patterns to exclude. |
| `return_context` | bool | When true, include extra context in the response (replaces legacy `return_cluster`). |

### `POST /api/v1/search/stream`

Same JSON body as `POST /api/v1/search`. The connection stays open and emits [Server-Sent Events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events): progress logs during execution, then a final event with the completed result or an error payload.

### `GET /api/v1/search/status`

Check server health, LLM configuration, and concurrency limits.

**Response:**

```json
{
  "status": "ok",
  "llm_configured": true,
  "version": "0.0.6post1",
  "max_concurrent_searches": 4
}
```

## Client Examples

### cURL

```bash
# FAST mode (default, greedy search with 2 LLM calls); paths optional
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
    "mode": "FAST",
    "paths": ["/path/to/project"]
  }'

# DEEP mode (comprehensive analysis with Monte Carlo sampling)
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
    "paths": ["/path/to/project"],
    "mode": "DEEP"
  }'

# Filename search (no LLM required)
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "config",
    "paths": ["/path/to/project"],
    "mode": "FILENAME_ONLY"
  }'
```

### Python (requests)

```python
import requests

response = requests.post(
    "http://localhost:8584/api/v1/search",
    json={
        "query": "How does authentication work?",
        "paths": ["/path/to/project"],
    },
    timeout=300
)

data = response.json()
if data.get("success"):
    print(data.get("data", {}).get("summary", data))
```

### Python (httpx, async)

```python
import httpx
import asyncio

async def search():
    async with httpx.AsyncClient(timeout=300) as client:
        resp = await client.post(
            "http://localhost:8584/api/v1/search",
            json={
                "query": "find all API endpoints",
                "paths": ["/path/to/project"],
            }
        )
        data = resp.json()
        print(data.get("data", {}).get("summary", data))

asyncio.run(search())
```

### JavaScript

```javascript
const response = await fetch("http://localhost:8584/api/v1/search", {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({
    query: "How does authentication work?",
    paths: ["/path/to/project"],
  })
});

const data = await response.json();
if (data.success) {
  console.log(data.data?.summary ?? data);
}
```

## SSE streaming (`POST /api/v1/search/stream`)

### cURL

```bash
curl -N -X POST http://localhost:8584/api/v1/search/stream \
  -H "Content-Type: application/json" \
  -H "Accept: text/event-stream" \
  -d '{"query":"How does authentication work?","mode":"FAST"}'
```

### Python (httpx)

```python
import httpx

url = "http://localhost:8584/api/v1/search/stream"
payload = {"query": "How does authentication work?", "mode": "FAST"}

with httpx.Client(timeout=None) as client:
    with client.stream(
        "POST",
        url,
        json=payload,
        headers={"Accept": "text/event-stream"},
    ) as response:
        response.raise_for_status()
        for line in response.iter_lines():
            if line:
                print(line)
```

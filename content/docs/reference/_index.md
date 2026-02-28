---
linkTitle: Reference
title: API Reference
---

Sirchmunk exposes a REST API when running in server mode (`sirchmunk serve` or `sirchmunk web serve`).

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/v1/search` | Execute a search query |
| `GET` | `/api/v1/search/status` | Check server and LLM configuration status |

**Interactive Docs:** When the server is running, visit http://localhost:8584/docs for Swagger UI.

## Search API

### `POST /api/v1/search`

Execute an intelligent search query.

**Request Body:**

```json
{
  "query": "How does authentication work?",
  "paths": ["/path/to/project"],
  "mode": "FAST",
  "max_depth": 10,
  "top_k_files": 20,
  "keyword_levels": 3,
  "include_patterns": ["*.py", "*.java"],
  "exclude_patterns": ["*test*", "*__pycache__*"],
  "return_cluster": false
}
```

**Response:**

```json
{
  "success": true,
  "data": {
    "result": "Markdown-formatted search result...",
    "cluster": null
  }
}
```

### `GET /api/v1/search/status`

Check server health and LLM configuration.

**Response:**

```json
{
  "status": "ok",
  "llm_configured": true,
  "version": "0.0.4"
}
```

## Client Examples

### cURL

```bash
# FAST mode (default, greedy search with 2 LLM calls)
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
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
if data["success"]:
    print(data["data"]["result"])
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
        print(data["data"]["result"])

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
  console.log(data.data.result);
}
```

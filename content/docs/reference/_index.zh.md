---
linkTitle: 参考
title: API 参考
---

Sirchmunk 在服务器模式下（`sirchmunk serve` 或 `sirchmunk web serve`）会暴露 REST API。

## API 端点

| 方法 | 端点 | 描述 |
|------|------|------|
| `POST` | `/api/v1/search` | 执行搜索查询 |
| `GET` | `/api/v1/search/status` | 检查服务器和 LLM 配置状态 |

**交互式文档：** 服务器运行时，访问 http://localhost:8584/docs 查看 Swagger UI。

## 搜索 API

### `POST /api/v1/search`

执行智能搜索查询。

**请求体：**

```json
{
  "query": "How does authentication work?",
  "paths": ["/path/to/project"],
  "mode": "DEEP",
  "max_depth": 10,
  "top_k_files": 20,
  "keyword_levels": 3,
  "include_patterns": ["*.py", "*.java"],
  "exclude_patterns": ["*test*", "*__pycache__*"],
  "return_cluster": false
}
```

**响应：**

```json
{
  "success": true,
  "data": {
    "result": "Markdown 格式的搜索结果...",
    "cluster": null
  }
}
```

### `GET /api/v1/search/status`

检查服务器健康状态和 LLM 配置。

**响应：**

```json
{
  "status": "ok",
  "llm_configured": true,
  "version": "0.0.2"
}
```

## 客户端示例

### cURL

```bash
# 基础搜索（DEEP 模式）
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
    "paths": ["/path/to/project"],
    "mode": "DEEP"
  }'

# 文件名搜索（快速，无需 LLM）
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
        "mode": "DEEP"
    },
    timeout=300
)

data = response.json()
if data["success"]:
    print(data["data"]["result"])
```

### Python (httpx, 异步)

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
                "mode": "DEEP"
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
    mode: "DEEP"
  })
});

const data = await response.json();
if (data.success) {
  console.log(data.data.result);
}
```

---
linkTitle: 参考
title: API 参考
---

Sirchmunk 在服务器模式下（`sirchmunk serve` 或 `sirchmunk web serve`）会暴露 REST API。

## API 端点

| 方法 | 端点 | 描述 |
|------|------|------|
| `POST` | `/api/v1/search` | 执行搜索查询（完成后返回 JSON） |
| `POST` | `/api/v1/search/stream` | 请求体相同；SSE 流式输出日志及最终结果或错误 |
| `GET` | `/api/v1/search/status` | 检查服务器与 LLM 配置（含 `max_concurrent_searches`） |

**交互式文档：** 服务器运行时，访问 http://localhost:8584/docs 查看 Swagger UI。

## 搜索 API

### `POST /api/v1/search`

执行智能搜索查询。处理完成后返回 JSON。

**请求体：** `paths` 为可选字段（详见[请求参数](#请求参数)）。

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

**响应：**

```json
{
  "success": true,
  "data": {
    "summary": "Markdown 格式的搜索结果...",
    "context": null
  }
}
```

### 请求参数 {#请求参数}

| 参数 | 类型 | 说明 |
|------|------|------|
| `query` | string | **必填。** 自然语言搜索问题。 |
| `paths` | string \| string[] | **可选。** 搜索根路径。省略时依次使用环境变量 `SIRCHMUNK_SEARCH_PATHS`（若已设置），否则为进程当前工作目录。 |
| `mode` | string | 搜索模式，如 `FAST`（默认）、`DEEP`、`FILENAME_ONLY`。 |
| `max_depth` | int | 最大目录深度。 |
| `top_k_files` | int | 参与检索的文件数量上限。 |
| `enable_dir_scan` | bool | 是否扫描目录以发现候选文件；默认 `true`。 |
| `max_loops` | int | **DEEP 模式：** 智能体 / 推理循环次数上限。 |
| `max_token_budget` | int | **DEEP 模式：** 本次运行的 token 预算；默认 **128K**（131072）。 |
| `include_patterns` | string[] | 包含的 glob 模式。 |
| `exclude_patterns` | string[] | 排除的 glob 模式。 |
| `return_context` | bool | 为 `true` 时在响应中包含额外上下文（替代旧版 `return_cluster`）。 |

### `POST /api/v1/search/stream`

请求 JSON 与 `POST /api/v1/search` 相同。连接保持打开，通过 [Server-Sent Events](https://developer.mozilla.org/zh-CN/docs/Web/API/Server-sent_events) 推送：执行过程中为进度日志，最后一条事件携带完成结果或错误信息。

### `GET /api/v1/search/status`

检查服务器健康状态、LLM 配置与并发限制。

**响应：**

```json
{
  "status": "ok",
  "llm_configured": true,
  "version": "0.0.6post1",
  "max_concurrent_searches": 4
}
```

## 客户端示例

### cURL

```bash
# FAST 模式（默认，贪心搜索，2 次 LLM 调用）；paths 可选
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
    "mode": "FAST",
    "paths": ["/path/to/project"]
  }'

# DEEP 模式（蒙特卡洛证据采样全面分析）
curl -X POST http://localhost:8584/api/v1/search \
  -H "Content-Type: application/json" \
  -d '{
    "query": "How does authentication work?",
    "paths": ["/path/to/project"],
    "mode": "DEEP"
  }'

# 文件名搜索（无需 LLM）
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

## SSE 流式接口（`POST /api/v1/search/stream`）

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

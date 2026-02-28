---
title: Docker Deployment
weight: 8
---

Pre-built Docker images are available on Alibaba Cloud Container Registry for quick containerized deployment.

## Available Images

| Region | Image |
|---|---|
| US West | `modelscope-registry.us-west-1.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.4` |
| China Beijing | `modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.4` |

## Quick Start

```bash
# Pull the image
docker pull modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.4

# Start the service
docker run -d \
  --name sirchmunk \
  -p 8584:8584 \
  -e LLM_API_KEY="your-api-key-here" \
  -e LLM_BASE_URL="https://api.openai.com/v1" \
  -e LLM_MODEL_NAME="gpt-5.2" \
  -e LLM_TIMEOUT=60.0 \
  -e UI_THEME=light \
  -e UI_LANGUAGE=en \
  -e SIRCHMUNK_VERBOSE=false \
  -v /path/to/your_work_path:/data/sirchmunk \
  -v /path/to/your/docs:/mnt/docs:ro \
  modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.4
```

Open http://localhost:8584 to access the WebUI, or call the API directly:

```python
import requests

response = requests.post(
    "http://localhost:8584/api/v1/search",
    json={
        "query": "your search question here",
        "paths": ["/mnt/docs"],
    },
)
print(response.json())
```

## Volume Mounts

| Mount | Purpose |
|---|---|
| `-v /path/to/your_work_path:/data/sirchmunk` | Persistent storage for knowledge clusters and chat history |
| `-v /path/to/your/docs:/mnt/docs:ro` | Mount your document directory (read-only) |

## Environment Variables

| Variable | Description | Default |
|---|---|---|
| `LLM_API_KEY` | Your LLM API key | *required* |
| `LLM_BASE_URL` | OpenAI-compatible API base URL | `https://api.openai.com/v1` |
| `LLM_MODEL_NAME` | Model name | `gpt-5.2` |
| `LLM_TIMEOUT` | LLM request timeout (seconds) | `60.0` |
| `UI_THEME` | Web UI theme (`light` or `dark`) | `light` |
| `UI_LANGUAGE` | UI language (`en` or `zh`) | `en` |
| `SIRCHMUNK_VERBOSE` | Enable verbose logging | `false` |

> [!TIP]
> For full Docker parameters and advanced usage, see the [docker/README.md](https://github.com/modelscope/sirchmunk/blob/main/docker/README.md) in the Sirchmunk repository.

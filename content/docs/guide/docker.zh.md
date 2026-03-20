---
title: Docker 部署
weight: 8
---

预构建的 Docker 镜像托管在阿里云容器镜像服务，支持容器化一键部署。

## 可用镜像

| 区域 | 镜像 |
|---|---|
| 美西 | `modelscope-registry.us-west-1.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.6` |
| 北京 | `modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.6` |

## 快速开始

```bash
# 拉取镜像（根据地理位置选择最近的 Registry）
docker pull modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.6

# 启动服务
docker run -d \
  --name sirchmunk \
  --cpus="4" \
  --memory="2g" \
  -p 8584:8584 \
  -e LLM_API_KEY="your-api-key-here" \
  -e LLM_BASE_URL="https://api.openai.com/v1" \
  -e LLM_MODEL_NAME="gpt-5.2" \
  -e LLM_TIMEOUT=60.0 \
  -e UI_THEME=light \
  -e UI_LANGUAGE=en \
  -e SIRCHMUNK_VERBOSE=false \
  -e SIRCHMUNK_ENABLE_CLUSTER_REUSE=false \
  -e SIRCHMUNK_SEARCH_PATHS=/mnt/docs \
  -v /path/to/your_work_path:/data/sirchmunk \
  -v /path/to/your/docs:/mnt/docs:ro \
  modelscope-registry.cn-beijing.cr.aliyuncs.com/modelscope-repo/sirchmunk:ubuntu22.04-py312-0.0.6
```

打开 http://localhost:8584 访问 WebUI，或直接调用 API：

```python
import requests

response = requests.post(
    "http://localhost:8584/api/v1/search",
    json={
        "query": "你的搜索问题",
        "paths": ["/mnt/docs"],
    },
)
print(response.json())
```

## 卷挂载

| 挂载 | 用途 |
|---|---|
| `-v /path/to/your_work_path:/data/sirchmunk` | 持久化存储知识聚类和聊天历史 |
| `-v /path/to/your/docs:/mnt/docs:ro` | 挂载文档目录（只读） |

## 环境变量

| 变量 | 描述 | 默认值 |
|---|---|---|
| `LLM_API_KEY` | LLM API 密钥 | *必填* |
| `LLM_BASE_URL` | OpenAI 兼容 API 基础 URL | `https://api.openai.com/v1` |
| `LLM_MODEL_NAME` | 模型名称 | `gpt-5.2` |
| `LLM_TIMEOUT` | LLM 请求超时（秒） | `60.0` |
| `UI_THEME` | Web UI 主题（`light` 或 `dark`） | `light` |
| `UI_LANGUAGE` | 界面语言（`en` 或 `zh`） | `en` |
| `SIRCHMUNK_VERBOSE` | 启用详细日志 | `false` |
| `SIRCHMUNK_ENABLE_CLUSTER_REUSE` | 启用知识簇复用 | `false` |
| `SIRCHMUNK_SEARCH_PATHS` | 默认搜索路径（逗号分隔） | — |
| `SIRCHMUNK_MAX_CONCURRENT_SEARCHES` | 最大并发搜索任务数 | `3` |

> [!TIP]
> 完整 Docker 参数和高级用法，请参阅 Sirchmunk 仓库中的 [docker/README.md](https://github.com/modelscope/sirchmunk/blob/main/docker/README.md)。

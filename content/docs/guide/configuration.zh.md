---
title: 配置
weight: 2
---

Sirchmunk 通过存储在 `.env` 文件中的环境变量进行配置。运行 `sirchmunk init` 后，将在 `~/.sirchmunk/.env` 生成配置文件。

<!--more-->

## 环境变量

### LLM 配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `LLM_API_KEY` | LLM API 密钥（DEEP 模式必需） | — |
| `LLM_BASE_URL` | OpenAI 兼容 API 基础 URL | `https://api.openai.com/v1` |
| `LLM_MODEL` | 使用的模型名称 | `gpt-4o` |

### 搜索配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `SIRCHMUNK_WORK_PATH` | 数据存储工作目录 | `~/.sirchmunk/` |
| `SIRCHMUNK_SEARCH_PATHS` | 默认搜索路径（逗号分隔） | — |
| `SIRCHMUNK_MAX_DEPTH` | 最大目录遍历深度 | `10` |
| `SIRCHMUNK_TOP_K_FILES` | 分析的最大文件数 | `20` |
| `SIRCHMUNK_KEYWORD_LEVELS` | 关键词粒度级别 | `3` |

### 服务器配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `SIRCHMUNK_HOST` | API 服务器绑定地址 | `127.0.0.1` |
| `SIRCHMUNK_PORT` | API 服务器端口 | `8584` |

## 数据存储布局

所有持久化数据存储在 `SIRCHMUNK_WORK_PATH` 下：

```text
{SIRCHMUNK_WORK_PATH}/
  ├── .cache/
  │   ├── history/              # 聊天会话历史（DuckDB）
  │   │   └── chat_history.db
  │   ├── knowledge/            # 知识簇（Parquet）
  │   │   └── knowledge_clusters.parquet
  │   └── settings/             # 用户设置（DuckDB）
  │       └── settings.db
  ├── .env                      # 环境配置
  └── mcp_config.json           # MCP 服务器配置
```

## 搜索参数

通过 SDK、CLI 或 API 调用搜索时，可使用以下参数：

| 参数 | 类型 | 默认值 | 描述 |
|------|------|--------|------|
| `query` | `string` | *必填* | 搜索查询或问题 |
| `paths` | `string[]` | *必填* | 要搜索的目录或文件 |
| `mode` | `string` | `DEEP` | `DEEP`（完整分析）或 `FILENAME_ONLY`（快速） |
| `max_depth` | `int` | `null` | 最大目录深度 |
| `top_k_files` | `int` | `null` | 返回的文件数量 |
| `keyword_levels` | `int` | `null` | 关键词粒度级别 |
| `include_patterns` | `string[]` | `null` | 要包含的文件 glob 模式 |
| `exclude_patterns` | `string[]` | `null` | 要排除的文件 glob 模式 |
| `return_cluster` | `bool` | `false` | 返回完整的 KnowledgeCluster 对象 |

> [!NOTE]
> `FILENAME_ONLY` 模式不需要 LLM API 密钥。`DEEP` 模式需要配置 LLM。

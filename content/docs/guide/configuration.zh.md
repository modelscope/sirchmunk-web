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
| `LLM_API_KEY` | LLM API 密钥（FAST 和 DEEP 模式必需） | — |
| `LLM_BASE_URL` | OpenAI 兼容 API 基础 URL | `https://api.openai.com/v1` |
| `LLM_MODEL_NAME` | 使用的模型名称 | `gpt-5.2` |

### 搜索配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `SIRCHMUNK_WORK_PATH` | 数据存储工作目录 | `~/.sirchmunk/` |
| `SIRCHMUNK_SEARCH_PATHS` | 默认搜索路径（逗号分隔） | — |
| `SIRCHMUNK_MAX_DEPTH` | 最大目录遍历深度 | `10` |
| `SIRCHMUNK_TOP_K_FILES` | 分析的最大文件数 | `20` |
| `SIRCHMUNK_MAX_CONCURRENT_SEARCHES` | 最大并发搜索任务数 | `3` |
| `SIRCHMUNK_ENABLE_CLUSTER_REUSE` | 启用知识簇复用 | `true` |

### 检索成本配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `GREP_MAX_FILESIZE_MB` | 单文件大小上限（MB）；超过此限制的文件在查询热路径上被跳过 | `64` |
| `GREP_RGA_ADAPTERS` | 允许的 rga 适配器（仅有界的；查询热路径禁用归档解压） | `poppler,pandoc,postprocpagebreaks` |
| `GREP_TIERED_SCAN` | 启用分层扫描：快速原生 rg 扫描 + 有界 rga 富格式扫描 | `true` |
| `GREP_TEXT_TIMEOUT` | 原生 rg 文本扫描超时（秒） | `15.0` |
| `GREP_TIMEOUT` | rga 富格式扫描超时（秒） | `60.0` |
| `GREP_RICH_EXTENSIONS` | 路由到 rga 富格式扫描的文件扩展名 | `pdf,docx,epub,odt` |

### 对话配置

| 变量 | 描述 | 默认值 |
|------|------|--------|
| `CHAT_HISTORY_MAX_TURNS` | 保留在历史中的最大对话轮数 | — |
| `CHAT_HISTORY_MAX_TOKENS` | 保留对话历史的最大 token 预算 | — |

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
  │   ├── compile/              # 编译产物（Beta）
  │   │   ├── manifest.json     # 文件清单与哈希
  │   │   ├── document_catalog.json
  │   │   ├── summary_index.json
  │   │   ├── trees/            # 层次化树索引
  │   │   ├── table_digests/    # 表格提取摘要
  │   │   └── xlsx_digests/     # 电子表格摘要
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
| `paths` | `string \| string[]` | *可选* | 要搜索的目录或文件；未设置时依次回退到 `SIRCHMUNK_SEARCH_PATHS`、当前工作目录 |
| `mode` | `string` | `DEEP` | `DEEP`（预算约束的证据探索智能体检索）、`FAST`（贪心搜索，2-5s）或 `FILENAME_ONLY` |
| `max_depth` | `int` | `null` | 最大目录深度 |
| `top_k_files` | `int` | `null` | 返回的文件数量 |
| `enable_dir_scan` | `bool` | `true` | 是否启用目录扫描 |
| `max_loops` | `int` | `null` | DEEP 模式循环上限 |
| `max_token_budget` | `int` | `null` | DEEP 模式 token 预算（未设置时默认 128K） |
| `include_patterns` | `string[]` | `null` | 要包含的文件 glob 模式 |
| `exclude_patterns` | `string[]` | `null` | 要排除的文件 glob 模式 |
| `response_format` | `string` | `rich` | `"rich"` Markdown 报告、`"minimal"` 简短回答、`"context"` SearchContext 对象、`"json"` 序列化上下文 |

> [!NOTE]
> `FILENAME_ONLY` 模式不需要 LLM API 密钥。`FAST` 和 `DEEP` 模式需要配置 LLM。默认模式为 `DEEP`，执行多路检索的预算约束证据探索。

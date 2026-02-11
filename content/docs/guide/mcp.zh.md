---
title: MCP 集成
weight: 5
---

Sirchmunk 提供了一个[模型上下文协议 (MCP)](https://modelcontextprotocol.io) 服务器，将其智能搜索能力作为 MCP 工具暴露。可与 **Claude Desktop** 和 **Cursor IDE** 等 AI 助手无缝集成。

## 快速开始

```bash
# 安装 MCP 支持
pip install "sirchmunk[mcp]"

# 初始化（生成 .env 和 mcp_config.json）
sirchmunk init

# 编辑 ~/.sirchmunk/.env 填入 LLM API 密钥

# 使用 MCP Inspector 测试
npx @modelcontextprotocol/inspector sirchmunk mcp serve
```

## 配置

运行 `sirchmunk init` 后，会在 `~/.sirchmunk/mcp_config.json` 生成配置文件。将其复制到 MCP 客户端配置中：

```json
{
  "mcpServers": {
    "sirchmunk": {
      "command": "sirchmunk",
      "args": ["mcp", "serve"],
      "env": {
        "SIRCHMUNK_SEARCH_PATHS": "/path/to/your_docs,/another/path"
      }
    }
  }
}
```

| 参数 | 描述 |
|------|------|
| `command` | sirchmunk 可执行文件路径。虚拟环境中请使用完整路径。 |
| `args` | `["mcp", "serve"]` 以 stdio 模式启动 MCP 服务器。 |
| `env.SIRCHMUNK_SEARCH_PATHS` | 默认搜索目录（逗号分隔）。 |

## 传输模式

### stdio（默认）

用于与桌面 AI 工具的本地集成：

```bash
sirchmunk mcp serve
```

### HTTP

用于远程或网络化场景：

```bash
sirchmunk mcp serve --transport http --port 3000
```

## 可用 MCP 工具

### `sirchmunk_search`

执行智能搜索查询。

**参数：**
- `query` (string, 必填) — 搜索问题
- `paths` (string[], 可选) — 搜索目录（默认使用 `SIRCHMUNK_SEARCH_PATHS`）
- `mode` (string, 可选) — `DEEP` 或 `FILENAME_ONLY`

### `sirchmunk_get_cluster`

通过 ID 获取特定知识簇。

**参数：**
- `cluster_id` (string, 必填) — 知识簇标识符

### `sirchmunk_list_clusters`

列出所有存储的知识簇。

## 搜索模式

| 模式 | 描述 | 需要 LLM |
|------|------|:--------:|
| **DEEP** | 包含蒙特卡洛证据采样的完整多阶段分析 | 是 |
| **FILENAME_ONLY** | 快速文件名搜索，无内容分析 | 否 |

## 与 Claude Desktop 集成

1. 安装 MCP 支持：`pip install "sirchmunk[mcp]"`
2. 运行 `sirchmunk init` 生成配置
3. 将 `mcp_config.json` 复制到 Claude Desktop 的 MCP 配置目录
4. 重启 Claude Desktop
5. Sirchmunk 搜索工具将出现在 Claude 的工具列表中

## 与 Cursor IDE 集成

1. 安装 MCP 支持
2. 运行 `sirchmunk init`
3. 将 MCP 服务器配置添加到 Cursor 的设置中
4. Sirchmunk 将作为 Cursor AI 聊天中的可用工具

> [!NOTE]
> MCP Inspector 适合在连接 AI 助手之前测试集成。在 MCP Inspector 中：**Connect** → **Tools** → **List Tools** → `sirchmunk_search` → 输入参数 → **Run Tool**。

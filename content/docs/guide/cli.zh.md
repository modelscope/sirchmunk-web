---
title: CLI 命令
weight: 3
---

Sirchmunk 提供了全面的命令行界面，涵盖所有操作 — 初始化、服务器管理、搜索和 MCP。

## 安装

```bash
# 基础安装（搜索 + CLI）
pip install sirchmunk

# 含 Web UI 支持
pip install "sirchmunk[web]"

# 含 MCP 支持
pip install "sirchmunk[mcp]"

# 全部安装
pip install "sirchmunk[all]"
```

## 命令

### `sirchmunk init`

初始化工作目录、`.env` 配置和 MCP 配置。

```bash
# 默认工作路径：~/.sirchmunk/
sirchmunk init

# 自定义工作路径
sirchmunk init --work-path /path/to/workspace
```

### `sirchmunk serve`

启动后端 API 服务器。

```bash
# 默认：localhost:8584
sirchmunk serve

# 自定义主机和端口
sirchmunk serve --host 0.0.0.0 --port 8000
```

### `sirchmunk search`

直接从终端执行搜索查询。

```bash
# 在当前目录中搜索
sirchmunk search "How does authentication work?"

# 在指定路径中搜索
sirchmunk search "find all API endpoints" ./src ./docs

# 快速文件名搜索（无需 LLM）
sirchmunk search "config" --mode FILENAME_ONLY

# JSON 格式输出
sirchmunk search "database schema" --output json

# 使用 API 服务器（需要运行中的服务器）
sirchmunk search "query" --api --api-url http://localhost:8584
```

### `sirchmunk web init`

构建 WebUI 前端。构建时需要 Node.js 18+。

```bash
sirchmunk web init
```

### `sirchmunk web serve`

从单个端口启动 API + WebUI。

```bash
# 生产模式（需要先执行 `web init`）
sirchmunk web serve

# 开发模式（热重载）
sirchmunk web serve --dev
```

### `sirchmunk mcp serve`

启动 MCP 服务器，用于 AI 助手集成。

```bash
# stdio 模式（用于 Claude Desktop、Cursor IDE）
sirchmunk mcp serve

# HTTP 模式
sirchmunk mcp serve --transport http --port 3000
```

### `sirchmunk version`

显示版本信息。

```bash
sirchmunk version
sirchmunk mcp version
```

## 命令参考

| 命令 | 描述 |
|------|------|
| `sirchmunk init` | 初始化工作目录、.env 和 MCP 配置 |
| `sirchmunk serve` | 启动后端 API 服务器 |
| `sirchmunk search` | 执行搜索查询 |
| `sirchmunk web init` | 构建 WebUI 前端（需要 Node.js 18+） |
| `sirchmunk web serve` | 启动 API + WebUI（单端口） |
| `sirchmunk web serve --dev` | 启动 API + Next.js 开发服务器（热重载） |
| `sirchmunk mcp serve` | 启动 MCP 服务器（stdio/HTTP） |
| `sirchmunk mcp version` | 显示 MCP 版本信息 |
| `sirchmunk version` | 显示版本信息 |

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
# 在当前目录中搜索（默认 FAST 模式）
sirchmunk search "How does authentication work?"

# 在指定路径中搜索
sirchmunk search "find all API endpoints" ./src ./docs

# DEEP 模式：智能体检索全面分析
sirchmunk search "数据库架构" --mode DEEP

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

### `sirchmunk compile`（Beta）

将文档集预处理为层次化树索引和知识簇。这是一个**可选**步骤 — 无需编译即可搜索，但编译产物可显著提升大型文档集的检索精度。

```bash
# 编译文档（默认增量模式）
sirchmunk compile --paths /path/to/documents

# 全量重新编译（忽略缓存）
sirchmunk compile --paths /path/to/documents --full

# 浅层模式（跳过树索引，更快）
sirchmunk compile --paths /path/to/documents --shallow

# 查看编译状态
sirchmunk compile --paths /path/to/documents --status

# 运行知识健康检查
sirchmunk compile --lint --work-path ~/.sirchmunk

# 自动修复检查问题
sirchmunk compile --lint --fix --work-path ~/.sirchmunk
```

| 选项 | 描述 |
|------|------|
| `--paths` | 要编译的目录或文件（必填） |
| `--full` | 强制全量重编译，忽略增量缓存 |
| `--shallow` | 跳过树索引，仅使用 LLM 直接摘要（更快） |
| `--max-files` | 最大处理文件数（超出时触发重要性采样） |
| `--concurrency` | 最大并行编译数（默认：3） |
| `--status` | 显示编译状态而非执行编译 |
| `--lint` | 运行知识健康检查 |
| `--fix` | 自动修复检查问题（需配合 `--lint`） |
| `--work-path` | 工作目录（默认：`~/.sirchmunk`） |

> [!NOTE]
> 编译产物会被搜索管线自动检测 — 编译完成后无需额外配置。当不存在编译产物时，搜索会回退到标准检索管线。

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
| `sirchmunk compile` | 将文档编译为知识索引 **（Beta）** |
| `sirchmunk web init` | 构建 WebUI 前端（需要 Node.js 18+） |
| `sirchmunk web serve` | 启动 API + WebUI（单端口） |
| `sirchmunk web serve --dev` | 启动 API + Next.js 开发服务器（热重载） |
| `sirchmunk mcp serve` | 启动 MCP 服务器（stdio/HTTP） |
| `sirchmunk mcp version` | 显示 MCP 版本信息 |
| `sirchmunk version` | 显示版本信息 |

---
title: "Sirchmunk v0.0.2 发布 — MCP 支持、CLI 与知识持久化"
summary: "Sirchmunk v0.0.2 带来 MCP 协议集成（适用于 Claude Desktop 和 Cursor IDE）、强大的 CLI、DuckDB 驱动的知识持久化和语义簇复用。"
date: 2026-02-05
authors:
  - me
tags:
  - 发布
  - Sirchmunk
  - MCP
  - CLI
image:
  caption: 'Sirchmunk v0.0.2'
---

我们非常高兴地宣布 **Sirchmunk v0.0.2** — 在模型上下文协议（MCP）集成、综合 CLI 和持久化知识管理方面迈出了重要一步。

<!--more-->

## 亮点

### MCP 集成

全面支持[模型上下文协议](https://modelcontextprotocol.io)，实现与 **Claude Desktop** 和 **Cursor IDE** 等 AI 助手的无缝集成。Sirchmunk 的搜索能力作为 MCP 工具暴露，任何兼容的客户端都可以发现和调用。

```json
{
  "mcpServers": {
    "sirchmunk": {
      "command": "sirchmunk",
      "args": ["mcp", "serve"],
      "env": {
        "SIRCHMUNK_SEARCH_PATHS": "/path/to/your_docs"
      }
    }
  }
}
```

MCP 服务器支持 **stdio**（用于本地桌面 AI 工具）和 **HTTP**（用于远程场景）两种传输模式。

### CLI 命令

新的 `sirchmunk` CLI 为所有操作提供统一入口：

| 命令 | 描述 |
|------|------|
| `sirchmunk init` | 初始化工作目录、.env 和 MCP 配置 |
| `sirchmunk serve` | 启动后端 API 服务器 |
| `sirchmunk search` | 直接从终端执行搜索 |
| `sirchmunk web init` | 构建 Web UI 前端 |
| `sirchmunk web serve` | 启动 API + Web UI（单端口） |
| `sirchmunk mcp serve` | 启动 MCP 服务器 |

### 知识持久化

DuckDB 驱动的存储搭配 Apache Parquet 导出，提供高效的知识管理：

- **内存优先**：运行时微秒级读写
- **原子持久化**：使用临时文件重命名模式实现崩溃安全写入
- **Parquet 导出**：列式格式，出色的压缩率和分析查询性能

### 语义簇复用

知识簇现在存储嵌入向量（384 维），支持快速余弦相似度匹配以处理重复或改述的查询。对于匹配已有知识的查询，可实现**亚秒级响应**。

## 如何升级

```bash
pip install --upgrade sirchmunk
```

或安装所有附加组件：

```bash
pip install "sirchmunk[all]"
```

## 下一步

我们的路线图包括：
- 网络搜索集成
- 多模态支持（图片、视频）
- 分布式跨节点搜索
- 知识可视化与深度分析

---

*[GitHub 仓库](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

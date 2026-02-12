---
title: "MCP 集成 — AI 智能体间协作"
summary: "无缝对接 Claude Desktop、Cursor IDE 及所有兼容 MCP 协议的 AI 助手。"
links:
  - type: site
    url: https://modelcontextprotocol.io
date: 2026-02-05
---

Sirchmunk 基于模型上下文协议（MCP）对外提供智能搜索服务，让 AI 智能体之间可以直接协作调用。任何支持 MCP 协议的 AI 助手，都能将 Sirchmunk 的搜索能力作为内置工具直接使用。

## 在 Cursor IDE 中使用 Sirchmunk MCP

以下演示展示了 Sirchmunk 以 MCP 工具的形式接入 Cursor IDE 的实际效果。AI 编程助手可以在编码过程中直接调用 Sirchmunk 进行深度搜索，无需切换窗口，也无需手动复制粘贴。搜索结果以流式方式实时返回，并附带来源引用与证据摘要。

![在 Cursor IDE 中使用 Sirchmunk MCP](Sirchmunk_mcp_cursor.gif "Sirchmunk MCP 工具在 Cursor IDE 中进行智能本地文件搜索")

**功能特性：**
- 支持 stdio 和 HTTP 两种传输模式
- 兼容 Claude Desktop、Cursor IDE 等主流 AI 工具
- 提供搜索、目录扫描、知识管理等多种 MCP 工具
- 通过 `mcp_config.json` 即可完成配置
- 搜索结果实时流式返回，附带来源引用

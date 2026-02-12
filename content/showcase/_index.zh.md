---
title: 案例展示
description: "查看 Sirchmunk 实际效果 — Cursor IDE 中的 MCP 集成与 Web UI 演示。"
type: landing

sections:
  - block: hero
    content:
      title: Sirchmunk 实际效果
      text: '了解 Sirchmunk 如何赋能智能搜索 — 从 AI 编程助手到独立 Web 界面。'
      primary_action:
        icon: brands/github
        text: 在 GitHub 上查看
        url: "https://github.com/modelscope/sirchmunk"
      secondary_action:
        text: 阅读技术报告
        url: ../blog/technical-deep-dive/
    design:
      no_padding: true
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
  - block: markdown
    content:
      title: "MCP 集成 — 在 Cursor IDE 中使用"
      subtitle: "基于模型上下文协议的 AI 智能体间协作"
      text: |
        Sirchmunk 可以作为 MCP 工具无缝接入 AI 编程助手。以下演示展示了 **Cursor IDE** 中的 AI 智能体直接调用 Sirchmunk 进行深度搜索的实际效果 — 无需切换窗口，无需手动复制粘贴。搜索结果以流式方式实时返回，并附带来源引用与证据摘要。

        <img src="/media/Sirchmunk_mcp_cursor.gif" alt="Sirchmunk MCP 在 Cursor IDE 中的集成" style="width:100%; border-radius:8px; margin:1rem 0;" />
    design:
      spacing:
        padding: ['3rem', '1rem', '3rem', '1rem']
  - block: markdown
    content:
      title: "Web UI — 实时聊天与搜索"
      subtitle: "功能完善的智能文档搜索界面"
      text: |
        Sirchmunk 的 Web UI 提供了基于对话的智能文档搜索界面。用户可以使用自然语言提问，实时获得流式回答，并附带内嵌搜索日志、来源引用和证据高亮。

        <img src="/media/Sirchmunk_Web.gif" alt="Sirchmunk Web UI 演示" style="width:100%; border-radius:8px; margin:1rem 0;" />
    design:
      spacing:
        padding: ['3rem', '1rem', '6rem', '1rem']
---

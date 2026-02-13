---
title: 'Sirchmunk'
date: 2026-02-10
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: hero
    content:
      title: '<span class="hero-title-with-logo"><img src="../images/icon.png" alt="Sirchmunk" class="hero-logo" /><span>从原始数据到自进化智能</span></span>'
      text: "Sirchmunk 是一款开源、无需向量嵌入的智能搜索引擎，能够实时检索多种类型原始数据并转化为动态知识，具备自我进化的能力。"
      primary_action:
        text: 快速开始
        url: docs/getting-started/
        icon: rocket-launch
      secondary_action:
        text: 阅读技术报告
        url: blog/technical-deep-dive/
      announcement:
        text: "Sirchmunk v0.0.3 发布 — 核心搜索算法与 MCP 集成双升级！"
        link:
          text: "查看所有版本"
          url: "https://github.com/modelscope/sirchmunk/releases"
    design:
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      css_class: ""
      background:
        color: ""
        image:
          filename: ""
          filters:
            brightness: 0.5
  - block: stats
    content:
      items:
        - statistic: "100+"
          description: |
            文件格式  
            即时搜索
        - statistic: "0"
          description: |
            无需预索引  
            直接搜索
        - statistic: "5"
          description: |
            集成接口  
            MCP · REST · WS · CLI · Web
    design:
      css_class: "bg-gray-100 dark:bg-gray-800"
      spacing:
        padding: ["1rem", 0, "1rem", 0]
  - block: features
    id: features
    content:
      title: 核心特性
      text: "一个超越传统 RAG 的智能搜索引擎 — 无需嵌入、自进化、高效利用 Token。"
      items:
        - name: 无嵌入检索
          icon: magnifying-glass
          description: "直接处理原始数据。无需向量数据库、无需预索引、无需 ETL 管线。只需放入文件即可立即搜索。"
        - name: 自进化知识
          icon: arrow-path
          description: "知识簇随每次搜索不断积累。系统持续学习与改进，搜索结果越来越快、越来越丰富。"
        - name: 蒙特卡洛证据采样
          icon: chart-bar
          description: "使用探索-利用策略对文档进行智能采样，无需阅读整个文件即可提取精确证据。"
        - name: ReAct 智能体自适应检索
          icon: cpu-chip
          description: "当常规检索不足时，ReAct 智能体自主迭代推理并探索替代检索策略，直到找到答案。"
        - name: 多接口集成
          icon: globe-alt
          description: "MCP 协议实现 AI 到 AI 通信，REST API、WebSocket 实时聊天、CLI 和现代 Web UI — 全部内置。"
        - name: Token 高效设计
          icon: bolt
          description: "仅在必要时触发 LLM 推理。蒙特卡洛采样和知识复用最大程度降低成本，同时最大化智能。"
  - block: cta-card
    content:
      title: "开始使用 Sirchmunk 搜索"
      text: "放入文件即刻搜索 — 无需向量数据库、无需预索引、无需复杂配置。从原始数据中实时检索并自主构建知识体系。"
      button:
        text: 快速入门指南
        url: docs/getting-started/
    design:
      card:
        css_class: "bg-primary-700"
        css_style: ""
---

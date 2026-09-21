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
        text: 阅读论文
        url: https://arxiv.org/abs/2608.16185
      announcement:
        text: "Sirchmunk v0.2.0 — LENS 论文已发布 · 多路 DEEP 检索 · 大语料鲁棒性"
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
        - statistic: "6"
          description: |
            集成接口  
            SKILL · MCP · REST · WS · CLI · Web
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
          description: "直接处理原始数据 — 无需向量数据库、无需预索引、无需 ETL 管线。放入文件即可搜索，完整保留源数据保真度。"
        - name: 自进化知识
          icon: arrow-path
          description: "每次搜索都生成可复用的 KnowledgeCluster。聚类随使用不断合并、拓展并形成元社区 — 系统在使用中持续变得更聪明。"
        - name: "LENS：隐式证据探索"
          icon: chart-bar
          description: "在查询条件下的隐式证据空间中进行预算约束证据定位。LENS 框架在显式成本约束下从原始动态文档中定位源可追溯证据。"
        - name: 多路 DEEP 检索
          icon: cpu-chip
          description: "词法、实体、目录、结构与主题图多条并行检索路径，通过置信度加权 RRF 融合 — 高置信单文件查询触发 soft 路由收缩快通道。"
        - name: 大语料鲁棒性
          icon: shield-check
          description: "单文件与单次查询的检索成本均设界：rg 优先分层扫描、适配器白名单、文件大小上限、单文件匹配上限与硬 token 预算，确保大规模语料高效稳定。"
        - name: 多接口集成
          icon: globe-alt
          description: "MCP 协议、OpenClaw 技能、REST API、WebSocket 实时聊天、CLI 与知识图谱可视化的现代 Web UI — 全部内置。"
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

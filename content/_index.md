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
      title: '<span class="hero-title-with-logo"><img src="images/icon.png" alt="Sirchmunk" class="hero-logo" /><span>Raw Data to Self-Evolving Intelligence</span></span>'
      text: "Sirchmunk is an open-source, embedding-free, agentic search engine that transforms raw data into self-evolving intelligence in real time."
      primary_action:
        text: Get Started
        url: docs/getting-started/
        icon: rocket-launch
      secondary_action:
        text: Read the Technical Report
        url: blog/technical-deep-dive/
      announcement:
        text: "Sirchmunk v0.0.3rc0 Released — Agentic Search Pipeline, Unified CLI & ReAct Agent!"
        link:
          text: "View all releases"
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
            File formats  
            searched instantly
        - statistic: "Zero"
          description: |
            Pre-indexing  
            required
        - statistic: "5"
          description: |
            Integration surfaces  
            MCP · REST · WS · CLI · Web
    design:
      css_class: "bg-gray-100 dark:bg-gray-800"
      spacing:
        padding: ["1rem", 0, "1rem", 0]
  - block: features
    id: features
    content:
      title: Key Features
      text: "An agentic search engine that goes beyond traditional RAG — embedding-free, self-evolving, and token-efficient."
      items:
        - name: Embedding-Free Retrieval
          icon: magnifying-glass
          description: "Work directly with raw data. No vector database, no pre-indexing, no ETL pipeline. Just drop your files and search immediately."
        - name: Self-Evolving Knowledge
          icon: arrow-path
          description: "Knowledge clusters compound with every search. The system learns and improves over time, delivering faster and richer results."
        - name: Monte Carlo Evidence Sampling
          icon: chart-bar
          description: "Strategically sample documents using exploration-exploitation methods. Extract precise evidence without reading entire files."
        - name: ReAct Agent Fallback
          icon: cpu-chip
          description: "When standard retrieval falls short, an autonomous ReAct agent iteratively explores alternative strategies until answers are found."
        - name: Multi-Surface Integration
          icon: globe-alt
          description: "MCP protocol for AI-to-AI communication, REST API, WebSocket real-time chat, CLI, and a modern Web UI — all built in."
        - name: Token-Efficient Design
          icon: bolt
          description: "LLM inference triggered only when necessary. Monte Carlo sampling and knowledge reuse minimize costs while maximizing intelligence."
  - block: cta-card
    content:
      title: "Start Searching with Sirchmunk"
      text: "Drop your files and search instantly — no vector database, no pre-indexing, no complex setup. Get self-evolving intelligence from your raw data in real time."
      button:
        text: Quick Start Guide
        url: docs/getting-started/
    design:
      card:
        css_class: "bg-primary-700"
        css_style: ""
---

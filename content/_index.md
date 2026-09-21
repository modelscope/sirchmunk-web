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
        text: Read the Paper
        url: https://arxiv.org/abs/2608.16185
      announcement:
        text: "Sirchmunk v0.2.0 — LENS Paper on arXiv · Multi-Path DEEP Retrieval · Large Corpus Robustness"
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
        - statistic: "6"
          description: |
            Integration surfaces  
            SKILL · MCP · REST · WS · CLI · Web
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
          description: "Work directly with raw data — no vector database, no pre-indexing, no ETL pipeline. Drop your files and search immediately with full source fidelity."
        - name: Self-Evolving Knowledge
          icon: arrow-path
          description: "Every search produces a reusable KnowledgeCluster. Clusters merge, broaden, and form meta-communities over time — the system literally gets smarter as you use it."
        - name: "LENS: Latent Evidence Exploration"
          icon: chart-bar
          description: "Budgeted evidence localization over a query-conditioned latent evidence space. The LENS framework locates source-grounded evidence from raw dynamic documents under explicit cost constraints."
        - name: Multi-Path DEEP Retrieval
          icon: cpu-chip
          description: "Parallel lexical, entity, directory, structural, and topic-graph retrieval routes fused by confidence-weighted RRF — with soft route-collapse for high-confidence single-file lookups."
        - name: Large Corpus Robustness
          icon: shield-check
          description: "Bounded per-file and per-query retrieval cost: tiered rg-first scan, adapter whitelist, file-size cap, per-file match limits, and hard token budgets keep huge corpora fast."
        - name: Multi-Surface Integration
          icon: globe-alt
          description: "MCP protocol, OpenClaw skill, REST API, WebSocket real-time chat, CLI, and a modern Web UI with knowledge graph visualization — all built in."
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

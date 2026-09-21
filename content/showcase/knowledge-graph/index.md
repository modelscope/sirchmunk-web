---
title: "Knowledge Graph Visualization"
summary: "Interactive knowledge graph powered by Cytoscape.js — visualize self-evolving knowledge clusters, their relationships, and lifecycle states."
date: 2026-07-21
image:
  filename: Sirchmunk_Knowledge_Graph.png
  caption: "Sirchmunk Knowledge Graph"
---

Sirchmunk's Web UI includes an interactive knowledge graph that visualizes the self-evolving knowledge clusters built incrementally from search interactions. Powered by [Cytoscape.js](https://js.cytoscape.org/), the graph presents semantic relationships between clusters and their lifecycle states — from **Emerging** to **Stable** — giving you a live view of how the system's intelligence grows over time.

Knowledge clusters are persisted in **DuckDB + Parquet** and evolve through a four-phase runtime cycle: connect & merge, edge refresh, meta-cluster discovery (via the Leiden community detection algorithm), and global recalibration. The graph UI lets you explore these communities interactively, filter by lifecycle stage, and trace how queries contribute to cluster formation.

![Sirchmunk Knowledge Graph](Sirchmunk_Knowledge_Graph.png "Knowledge Graph — Interactive visualization of knowledge clusters with lifecycle stages")

---
title: "Knowledge Graph Visualization"
summary: "Interactive knowledge graph powered by Cytoscape.js — visualize self-evolving knowledge clusters, their relationships, and lifecycle states."
date: 2026-07-21
image:
  filename: Sirchmunk_Knowledge_Graph.png
  caption: "Sirchmunk Knowledge Graph"
---

Sirchmunk's knowledge system is not static — it evolves. Every search interaction produces reusable knowledge clusters that merge, broaden, and form meta-communities over time. This page showcases the self-evolving knowledge architecture and its interactive visualization.

## Knowledge Graph Visualization

![Sirchmunk Knowledge Graph](Sirchmunk_Knowledge_Graph.png "Knowledge Graph — Interactive visualization of knowledge clusters with lifecycle stages")

Powered by [Cytoscape.js](https://js.cytoscape.org/), the Web UI's Knowledge Graph page renders clusters as nodes, similarity edges and co-query edges as links, with lifecycle states color-coded:

- **Emerging** — Newly formed clusters with limited validation
- **Stable** — Clusters consistently reinforced across multiple queries
- **Meta** — Higher-order communities discovered through community detection
- **Deprecated** — Clusters whose underlying evidence is no longer supported
- **Contested** — Clusters with conflicting or contradictory evidence

Users can explore cluster details, trace evidence back to source documents, and observe how the knowledge topology evolves across search sessions.

## Knowledge Evolver Architecture

![Knowledge Evolver Architecture](Knowledge_Evolver_Architecture.png "KnowledgeEvolver — Four-phase evolution cycle for knowledge graph maintenance")

The `KnowledgeEvolver` orchestrates a four-phase evolution cycle that runs asynchronously in the background, triggered by search activity:

### Phase 1 — Connect & Merge

When new clusters accumulate in the buffer, the evolver computes pairwise similarity. Clusters with similarity ≥ 0.90 are merged; those with similarity ≥ 0.60 gain inter-cluster edges. This consolidates fragmented knowledge from related queries into coherent units.

### Phase 2 — Refresh Edges

Existing edges are re-evaluated. Stale or weak connections are pruned, and edge weights are updated based on recent co-query patterns and semantic drift. This keeps the graph topology aligned with actual usage patterns rather than historical artifacts.

### Phase 3 — Detect Meta Clusters

Leiden community detection (via igraph) discovers higher-order structure — groups of clusters that form coherent knowledge communities. These meta-clusters represent emergent domain expertise that no single query could have produced.

### Phase 4 — Global Update

Lifecycle states are synchronized across the graph. Clusters that have been consistently reinforced graduate from Emerging to Stable; orphaned or contradicted clusters move toward Deprecated. Version numbers and timestamps are updated to maintain audit trails.

The entire cycle runs under budget constraints (LLM concurrency semaphore, buffer thresholds) and never blocks the query hot path. Results persist to DuckDB + Parquet with an incremental manifest for crash recovery.

## Knowledge Evolution in Action

<video controls width="100%" poster="Sirchmunk_Knowledge_Graph.png">
  <source src="knowledge_evolving.mp4" type="video/mp4">
</video>

This time-lapse replay demonstrates knowledge clusters evolving across 200 queries over 4 documents. Watch as isolated clusters emerge, connect through shared evidence, merge into consolidated knowledge units, and eventually form meta-communities — all without manual curation.

## Design Principles

- **Observation-driven, not rule-driven**: Evolution is triggered by actual search patterns, not pre-defined rules. The system learns what knowledge is worth preserving by observing which clusters get reused.
- **Source fidelity preserved**: Knowledge evolution changes navigation efficiency — the paths the system takes to find evidence — but never alters the source documents. Every fact remains traceable to its original location.
- **Bounded cost**: Evolution runs under budget constraints (LLM concurrency semaphore, buffer thresholds). It never blocks the query hot path.

## Usage

Knowledge evolution is enabled by default. You can configure it via environment variables or the SDK parameter `enable_knowledge_evolution`.

The Knowledge Graph is accessible from the Web UI sidebar navigation under **"Knowledge"**. Clusters and their relationships can also be queried programmatically through the Python SDK.

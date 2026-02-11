---
title: Architecture
weight: 0
---

Sirchmunk's architecture is organized into cleanly separated layers, following the principle of **Separation of Concerns**.

## System Overview

![Sirchmunk Architecture](Sirchmunk_Architecture.png "Sirchmunk high-level architecture diagram")

## Multi-Phase Search Pipeline

At the heart of Sirchmunk is a multi-phase search pipeline designed around **maximum parallelism within each phase** and **strict phase dependencies** between them.

### Phase 0 — Knowledge Cluster Reuse

Before any computation begins, the system checks whether a semantically similar query has been answered before. A lightweight embedding of the query is compared via cosine similarity against stored knowledge clusters. If a close match is found, the cached cluster is returned in sub-second time.

### Phase 1 — Parallel Probing

Four independent probes launch concurrently to gather diverse signals:

1. **LLM Keyword Extraction** — Multi-granularity keyword decomposition
2. **Directory Structure Scan** — File metadata collection and structural analysis
3. **Knowledge Cache Lookup** — Partial match search across existing clusters
4. **Spec-Path Context Load** — Previously computed context for known paths

### Phase 2 — Retrieval & Ranking

Two complementary strategies run in parallel:

- **Content-based retrieval** — IDF-weighted keyword search through raw file contents
- **Structure-based ranking** — LLM-guided evaluation of candidate files by metadata

### Phase 3 — Knowledge Cluster Construction

Results are merged, deduplicated, and processed through Monte Carlo evidence sampling. The LLM synthesizes evidence fragments into structured Knowledge Clusters.

### Phase 4 — Summarization or ReAct Refinement

- **Evidence found** → LLM generates a structured briefing
- **No evidence** → ReAct agent activates for iterative exploration

### Phase 5 — Persist

Valuable clusters are saved with their embeddings for future reuse.

## Key Algorithms

### Monte Carlo Evidence Sampling

An exploration–exploitation approach for extracting evidence from large documents:

![Monte Carlo Evidence Sampling Algorithm](Sirchmunk_MonteCarloSamplingAlgo.png "Monte Carlo Evidence Sampling: Three-act exploration–exploitation strategy")

1. **Act 1 — Casting the Net**: Fuzzy anchoring + stratified random sampling
2. **Act 2 — Zooming In**: Gaussian importance sampling around high-scoring seeds
3. **Act 3 — Synthesis**: Top-K snippets → LLM summary

### ReAct Agent

An autonomous Think → Act → Observe loop with:

- Prioritized tool strategy (keyword search → file read → knowledge query → directory scan)
- Dual-budget mechanism (token budget + loop count)
- Memory of previously explored avenues

## Design Principles

Sirchmunk adheres to **SOLID principles**:

- **Single Responsibility** — Each component has one clear purpose
- **Open/Closed** — Extended through abstractions, not modifications
- **Liskov Substitution** — All implementations honor abstract contracts
- **Interface Segregation** — Minimal, focused interfaces
- **Dependency Inversion** — High-level logic depends on abstractions

For a comprehensive technical analysis, read the [Technical Deep Dive](/blog/technical-deep-dive/).

---
title: "Introducing Sirchmunk v0.0.1 — Initial Release"
summary: "The first release of Sirchmunk, an embedding-free agentic search engine. Features indexless retrieval, self-evolving knowledge clusters, Monte Carlo evidence sampling, and a real-time Web UI."
date: 2026-01-22
authors:
  - me
tags:
  - Release
  - Sirchmunk
  - Agentic Search
image:
  caption: 'Sirchmunk v0.0.1'
---

We are excited to announce the initial release of **Sirchmunk** — an open-source, embedding-free, agentic search engine that transforms raw data into self-evolving intelligence in real time.

<!--more-->

## What is Sirchmunk?

Sirchmunk takes a fundamentally different approach to retrieval-augmented generation. Instead of relying on static vector embeddings and pre-computed indexes, it operates directly on raw files — delivering instant, full-fidelity retrieval without any pre-processing.

## Key Features in v0.0.1

### Indexless Retrieval
Drop your files and search immediately. No vector database, no ETL pipeline, no pre-indexing required. Sirchmunk leverages ripgrep-all to search across 100+ file formats in a streaming fashion.

### Self-Evolving Knowledge Clusters
Knowledge compounds with every search. The system builds structured Knowledge Clusters that evolve from Emerging to Stable as they are validated across multiple queries.

### Monte Carlo Evidence Sampling
Instead of reading entire documents, Sirchmunk uses an exploration-exploitation approach to strategically sample and score document regions — extracting precise evidence while minimizing LLM token costs.

### ReAct Agent Fallback
When standard retrieval falls short, an autonomous ReAct agent takes over with iterative reasoning-and-action cycles, exploring alternative strategies until answers are found.

### Web UI
A modern web interface with real-time streaming chat, source citations, knowledge cluster browsing, and system monitoring dashboards. Supports dark/light themes and bilingual localization.

### Python SDK

```python
import asyncio
from sirchmunk import AgenticSearch
from sirchmunk.llm import OpenAIChat

llm = OpenAIChat(
    api_key="your-api-key",
    base_url="your-base-url",
    model="your-model-name"
)

async def main():
    searcher = AgenticSearch(llm=llm)
    result = await searcher.search(
        query="How does transformer attention work?",
        paths=["/path/to/documents"],
    )
    print(result)

asyncio.run(main())
```

## Getting Started

```bash
pip install sirchmunk
```

For the full documentation, see the [Getting Started guide](/docs/getting-started/).

## What's Next?

We are actively working on MCP integration, CLI improvements, and knowledge persistence. Stay tuned for v0.0.2!

---

*[GitHub Repository](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

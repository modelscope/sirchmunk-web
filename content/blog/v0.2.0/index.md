---
title: "Sirchmunk v0.2.0: LENS Paper, Multi-Path DEEP Retrieval & Large Corpus Robustness"
summary: "Sirchmunk v0.2.0 is a major milestone — the LENS research paper is published on arXiv, the retrieval engine gains multi-path DEEP fusion and bounded retrieval cost invariants, and the system adopts a generalization-first design philosophy."
date: 2026-09-20
authors:
  - admin
tags:
  - Release
  - LENS
  - DEEP
  - Retrieval
image:
  caption: 'LENS Framework'
---

Sirchmunk v0.2.0 marks a turning point for the project. The core algorithm behind Sirchmunk's retrieval engine has been formalized in a research paper and published on arXiv, the DEEP search pipeline has been rebuilt around multi-path fusion, and the system now enforces strict retrieval cost invariants that keep performance predictable on corpora of any size.

<!--more-->

## LENS: The Research Paper

The theoretical foundations of Sirchmunk's in-context search have been formalized in **"LENS: In-Context Search via Latent Evidence Exploration over Dynamic Raw Documents"** ([arXiv:2608.16185](https://arxiv.org/abs/2608.16185)).

LENS reframes retrieval as *Budgeted Evidence Localization*: given a query and a raw-document corpus, the system maintains a query-conditioned belief over a latent evidence space and iteratively refines it through proposal policies and an LLM relevance oracle — all under an explicit token budget.

Key results from the controlled evaluation:

- **500-question evaluation**: 62.4% Exact Match with 84.8% evidence recall (vs. ReAct baseline at 65.2% EM but only 50.4% evidence recall).
- **150-question fullwiki subset** (raw Wikipedia dump, zero indexing): LENS achieves 43.3% EM vs. ReAct's 42.7% EM, with substantially stronger evidence grounding (84.0% vs. 70.7%).

These numbers demonstrate that LENS trades a modest amount of accuracy for dramatically better evidence traceability — the answer is not only correct, but *provably grounded* in source material.

## Multi-Path DEEP Retrieval

DEEP mode has been fundamentally restructured. Instead of a single retrieval strategy, it now runs **five complementary retrieval paths** in parallel:

1. **Lexical** — keyword-driven content matching with IDF-weighted scoring.
2. **Entity** — exact-match probes for named entities, identifiers, and structured values.
3. **Directory** — file-system structure analysis using naming conventions, path hierarchy, and modification timestamps.
4. **Structural** — heuristic document tree navigation (v2) with structure anchors for DOCX, RST, and other formatted sources.
5. **Topic-Graph** — cross-document topic-map routing that leverages the self-evolving knowledge graph to identify relevant document clusters.

Results from all five paths are fused via **confidence-weighted Reciprocal Rank Fusion (RRF)**. A **soft route-collapse** mechanism dynamically disables low-yield paths when a single path produces a high-confidence match, cutting latency and token consumption without sacrificing answer quality.

![Sirchmunk Architecture](Sirchmunk_Architecture.png "Sirchmunk v0.2.0 system architecture with multi-path DEEP retrieval and LENS framework integration.")

## Large Corpus Robustness

Previous versions could stall or time out on very large, archive-heavy corpora. v0.2.0 introduces **retrieval cost invariants** — hard bounds that ensure per-file and per-query cost never grows unbounded:

- **`GREP_RGA_ADAPTERS`**: Capability-based adapter whitelist. Only bounded document extractors (poppler, pandoc) are enabled on the query hot path; unbounded recursive adapters (decompress, zip, tar, sqlite, ffmpeg) are restricted to offline extraction.
- **`GREP_MAX_FILESIZE_MB`**: Per-file size cap. Files exceeding the threshold are skipped during live queries.
- **`GREP_TIERED_SCAN`**: A fast native-`rg` pass over all files is unioned with an `rga` pass restricted to rich-format extensions, so adapter dispatch never walks the entire tree.
- **Fail-fast timeout budgets**: `GREP_TEXT_TIMEOUT` for the text pass and `GREP_TIMEOUT` for the rich pass; on timeout, search degrades gracefully to native `rg` rather than hanging.

Directory scanning is now enabled by default for stronger filename routing, and a hard token budget keeps each query within a configurable limit.

## Generalization-First Design

v0.2.0 adopts a principled stance against benchmark-specific hard rules. Benchmark-tied logic such as hardcoded entity patterns, fixed column positions, and English-only stop words has been replaced with generalizable alternatives:

- **Grounded numeric verification**: Computation answers are re-checked deterministically from model-disclosed, evidence-grounded operands — entirely corpus-agnostic.
- **Injectable tokenizers**: Tokenizers and lexical policies are dependency-injected with a general default implementation, rather than embedded inside modules.
- **Corpus-adaptive statistics**: Document-frequency-based adaptive stop-word pruning replaces fixed word lists, working natively across languages and domains.

Every replacement behavior is gated behind an environment switch, defaults to the new implementation, and allows single-item rollback on failure.

## Knowledge Graph Visualization

The Web UI now includes an **interactive knowledge graph** powered by Cytoscape.js. The graph visualizes self-evolving knowledge clusters and their semantic relationships, including lifecycle states (Emerging, Stable, Meta) and edge weights. Users can explore, filter, and drill down into the cluster topology to understand how the system's knowledge evolves with use.

Behind the visualization, the `KnowledgeEvolver` orchestrates a four-phase background evolution cycle — Connect & Merge, Refresh Edges, Detect Meta Clusters (via Leiden community detection), and Global Update — that continuously maintains and consolidates the knowledge graph without blocking queries.

![Knowledge Evolver Architecture](Knowledge_Evolver_Architecture.png "KnowledgeEvolver — Four-phase evolution cycle for knowledge graph maintenance")

The design philosophy is observation-driven: evolution is triggered by actual search patterns rather than pre-defined rules, and source fidelity is always preserved — the system changes how it navigates to evidence, never the evidence itself. For a full animated demonstration, see the [Knowledge Evolution showcase](/showcase/knowledge-graph/).

## Other Improvements

- **Broader format coverage**: Native exact-match fallback for LOG, PPTX, and XLSX files, plus heuristic document tree v2 (including DOCX/RST) with structure anchors guiding evidence extraction.
- **DeepSeek V4 compatibility**: Full support for DeepSeek V4's thinking mode (`thinking_content`) in the OpenAI-compatible client.
- **Stabilized large-corpus retrieval**: Combined improvements in tiered scanning, per-file match caps, and adapter whitelisting eliminate the timeout and stall issues observed on large corpora in earlier versions.

## Get Started

```bash
pip install --upgrade sirchmunk
```

Or install with all extras:

```bash
pip install "sirchmunk[all]"
```

- **Documentation**: [modelscope.github.io/sirchmunk-web](https://modelscope.github.io/sirchmunk-web/)
- **GitHub**: [github.com/modelscope/sirchmunk](https://github.com/modelscope/sirchmunk)
- **Paper**: [arXiv:2608.16185](https://arxiv.org/abs/2608.16185)

---

*[GitHub Repository](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

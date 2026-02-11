---
title: Customization
linkTitle: Customization
weight: 1
---

## Extending Sirchmunk

Sirchmunk is designed around the **Open/Closed Principle** — extend through abstractions, not modifications.

### Custom LLM Providers

Any OpenAI-compatible API endpoint works out of the box. Configure in your `.env`:

```bash
LLM_API_KEY=your-api-key
LLM_BASE_URL=https://your-provider.com/v1
LLM_MODEL=your-model-name
```

### File Type Filtering

Control which files are searched using include/exclude patterns:

```python
result = await searcher.search(
    query="database queries",
    paths=["./src"],
    include_patterns=["*.py", "*.sql"],
    exclude_patterns=["*test*", "*migration*"],
)
```

### Search Depth

Tune the search depth and file count:

```python
result = await searcher.search(
    query="authentication flow",
    paths=["./src"],
    max_depth=5,           # Limit directory traversal
    top_k_files=10,        # Analyze fewer files for speed
    keyword_levels=2,      # Fewer keyword granularity levels
)
```

### Knowledge Management

Access and manage knowledge clusters programmatically:

```python
# Knowledge clusters are stored at:
# {SIRCHMUNK_WORK_PATH}/.cache/knowledge/knowledge_clusters.parquet

# Query using DuckDB or the KnowledgeManager API
```

## Environment Variables Reference

See the [Configuration guide](/docs/guide/configuration/) for a complete list of environment variables.

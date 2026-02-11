---
title: Python SDK
weight: 6
---

Sirchmunk provides a Python SDK for programmatic access to its intelligent search capabilities.

## Installation

```bash
pip install sirchmunk
```

## Basic Usage

```python
import asyncio
from sirchmunk import AgenticSearch
from sirchmunk.llm import OpenAIChat

# Initialize the LLM interface
llm = OpenAIChat(
    api_key="your-api-key",
    base_url="your-base-url",   # e.g., https://api.openai.com/v1
    model="your-model-name"     # e.g., gpt-4o
)

async def main():
    # Create the search engine
    searcher = AgenticSearch(llm=llm)

    # Execute a search
    result: str = await searcher.search(
        query="How does transformer attention work?",
        paths=["/path/to/documents"],
    )

    print(result)

asyncio.run(main())
```

> [!WARNING]
> Upon initialization, `AgenticSearch` automatically checks if `ripgrep-all` and `ripgrep` are installed. If they are missing, it will attempt to install them automatically. If the automatic installation fails, please install them manually:
> - [ripgrep](https://github.com/BurntSushi/ripgrep)
> - [ripgrep-all](https://github.com/phiresky/ripgrep-all)

## Search Parameters

```python
result = await searcher.search(
    query="database connection pooling",        # Required: search question
    paths=["/path/to/project/src"],             # Required: directories to search
    mode="DEEP",                                # DEEP or FILENAME_ONLY
    max_depth=10,                               # Max directory depth
    top_k_files=20,                             # Number of top files
    keyword_levels=3,                           # Keyword granularity
    include_patterns=["*.py", "*.java"],        # File patterns to include
    exclude_patterns=["*test*", "*__pycache__*"], # Patterns to exclude
    return_cluster=True,                        # Return full KnowledgeCluster
)
```

## Accessing Results

### Basic Result

```python
# The result is a formatted markdown string
result = await searcher.search(query="...", paths=["..."])
print(result)
```

### With Knowledge Cluster

```python
# Get the full KnowledgeCluster object
result = await searcher.search(
    query="...",
    paths=["..."],
    return_cluster=True,
)

# Access cluster metadata
print(result.cluster_id)
print(result.confidence)
print(result.lifecycle_state)
print(result.evidence_units)
```

### LLM Usage Tracking

```python
# After search completion
for usage in searcher.llm_usages:
    print(f"Prompt tokens: {usage.prompt_tokens}")
    print(f"Completion tokens: {usage.completion_tokens}")
    print(f"Total tokens: {usage.total_tokens}")
```

## LLM Provider Compatibility

Sirchmunk works with any OpenAI-compatible API endpoint:

- **OpenAI** — GPT-4, GPT-4o, GPT-3.5
- **Local models** — Ollama, llama.cpp, vLLM, SGLang
- **Claude** — Via API proxy
- **Any OpenAI-compatible endpoint**

```python
# Example: Using a local Ollama model
llm = OpenAIChat(
    api_key="ollama",
    base_url="http://localhost:11434/v1",
    model="llama3"
)

# Example: Using a custom endpoint
llm = OpenAIChat(
    api_key="your-key",
    base_url="https://your-custom-endpoint.com/v1",
    model="your-model"
)
```

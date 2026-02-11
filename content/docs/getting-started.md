---
title: Quick Start
date: 2026-02-10
weight: 1
---

Get Sirchmunk up and running in minutes.

## Prerequisites

- **Python** 3.10+
- **LLM API Key** (OpenAI-compatible endpoint, local or remote)
- **Node.js** 18+ (optional, for Web UI)

## Installation

{{% steps %}}

### Install Sirchmunk

```bash
# Create virtual environment (recommended)
conda create -n sirchmunk python=3.13 -y && conda activate sirchmunk

# Install from PyPI
pip install sirchmunk

# Or via UV
uv pip install sirchmunk

# Or install from source
git clone https://github.com/modelscope/sirchmunk.git && cd sirchmunk
pip install -e .
```

### Initialize

```bash
# Initialize with default settings (work path: ~/.sirchmunk/)
sirchmunk init

# Or with a custom work path
sirchmunk init --work-path /path/to/workspace
```

### Configure your LLM

Edit `~/.sirchmunk/.env` with your LLM API key and endpoint:

```bash
LLM_API_KEY=your-api-key
LLM_BASE_URL=https://api.openai.com/v1
LLM_MODEL=gpt-4o
```

### Run your first search

```bash
# Search in a directory
sirchmunk search "How does authentication work?" ./src

# Or use the Python SDK
python -c "
import asyncio
from sirchmunk import AgenticSearch
from sirchmunk.llm import OpenAIChat

llm = OpenAIChat(api_key='your-key', base_url='your-url', model='your-model')

async def main():
    searcher = AgenticSearch(llm=llm)
    result = await searcher.search(
        query='How does authentication work?',
        paths=['./src'],
    )
    print(result)

asyncio.run(main())
"
```

{{% /steps %}}

## Next Steps

{{< cards >}}
  {{< card url="../guide/architecture" title="Architecture" icon="cpu-chip" subtitle="Understand the multi-phase search pipeline" >}}
  {{< card url="../guide/configuration" title="Configuration" icon="adjustments-vertical" subtitle="Customize Sirchmunk's behavior" >}}
  {{< card url="../guide/web-ui" title="Web UI" icon="computer-desktop" subtitle="Set up the web interface" >}}
  {{< card url="../guide/mcp" title="MCP Integration" icon="link" subtitle="Connect with Claude Desktop and Cursor IDE" >}}
{{< /cards >}}

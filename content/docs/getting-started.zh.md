---
title: 快速入门
date: 2026-02-10
weight: 1
---

几分钟内启动并运行 Sirchmunk。

## 前置要求

- **Python** 3.10+
- **LLM API 密钥**（OpenAI 兼容端点，本地或远程）
- **Node.js** 18+（可选，用于 Web UI）

## 安装

{{% steps %}}

### 安装 Sirchmunk

```bash
# 创建虚拟环境（推荐）
conda create -n sirchmunk python=3.13 -y && conda activate sirchmunk

# 从 PyPI 安装
pip install sirchmunk

# 或通过 UV 安装
uv pip install sirchmunk

# 或从源码安装
git clone https://github.com/modelscope/sirchmunk.git && cd sirchmunk
pip install -e .
```

### 初始化

```bash
# 使用默认设置初始化（工作路径：~/.sirchmunk/）
sirchmunk init

# 或自定义工作路径
sirchmunk init --work-path /path/to/workspace
```

### 配置 LLM

编辑 `~/.sirchmunk/.env`，填入您的 LLM API 密钥和端点：

```bash
LLM_API_KEY=your-api-key
LLM_BASE_URL=https://api.openai.com/v1
LLM_MODEL=gpt-4o
```

### 执行首次搜索

```bash
# 在目录中搜索
sirchmunk search "How does authentication work?" ./src

# 或使用 Python SDK
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

## 下一步

{{< cards >}}
  {{< card url="../guide/architecture" title="架构" icon="cpu-chip" subtitle="了解多阶段搜索管线" >}}
  {{< card url="../guide/configuration" title="配置" icon="adjustments-vertical" subtitle="自定义 Sirchmunk 的行为" >}}
  {{< card url="../guide/web-ui" title="Web UI" icon="computer-desktop" subtitle="设置 Web 界面" >}}
  {{< card url="../guide/mcp" title="MCP 集成" icon="link" subtitle="连接 Claude Desktop 和 Cursor IDE" >}}
{{< /cards >}}

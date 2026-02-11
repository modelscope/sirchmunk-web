---
title: Python SDK
weight: 6
---

Sirchmunk 提供 Python SDK，用于以编程方式访问其智能搜索能力。

## 安装

```bash
pip install sirchmunk
```

## 基本用法

```python
import asyncio
from sirchmunk import AgenticSearch
from sirchmunk.llm import OpenAIChat

# 初始化 LLM 接口
llm = OpenAIChat(
    api_key="your-api-key",
    base_url="your-base-url",   # 如 https://api.openai.com/v1
    model="your-model-name"     # 如 gpt-4o
)

async def main():
    # 创建搜索引擎
    searcher = AgenticSearch(llm=llm)

    # 执行搜索
    result: str = await searcher.search(
        query="How does transformer attention work?",
        paths=["/path/to/documents"],
    )

    print(result)

asyncio.run(main())
```

> [!WARNING]
> 初始化时，`AgenticSearch` 会自动检查 `ripgrep-all` 和 `ripgrep` 是否已安装。如果缺失，系统会尝试自动安装。如果自动安装失败，请手动安装：
> - [ripgrep](https://github.com/BurntSushi/ripgrep)
> - [ripgrep-all](https://github.com/phiresky/ripgrep-all)

## 搜索参数

```python
result = await searcher.search(
    query="database connection pooling",        # 必填：搜索问题
    paths=["/path/to/project/src"],             # 必填：搜索目录
    mode="DEEP",                                # DEEP 或 FILENAME_ONLY
    max_depth=10,                               # 最大目录深度
    top_k_files=20,                             # 最大文件数
    keyword_levels=3,                           # 关键词粒度
    include_patterns=["*.py", "*.java"],        # 要包含的文件模式
    exclude_patterns=["*test*", "*__pycache__*"], # 要排除的文件模式
    return_cluster=True,                        # 返回完整的 KnowledgeCluster
)
```

## 访问结果

### 基本结果

```python
# 结果是格式化的 Markdown 字符串
result = await searcher.search(query="...", paths=["..."])
print(result)
```

### 含知识簇

```python
# 获取完整的 KnowledgeCluster 对象
result = await searcher.search(
    query="...",
    paths=["..."],
    return_cluster=True,
)

# 访问知识簇元数据
print(result.cluster_id)
print(result.confidence)
print(result.lifecycle_state)
print(result.evidence_units)
```

### LLM 使用量跟踪

```python
# 搜索完成后
for usage in searcher.llm_usages:
    print(f"提示词 Token: {usage.prompt_tokens}")
    print(f"补全 Token: {usage.completion_tokens}")
    print(f"总 Token: {usage.total_tokens}")
```

## LLM 供应商兼容性

Sirchmunk 适用于任何 OpenAI 兼容的 API 端点：

- **OpenAI** — GPT-4、GPT-4o、GPT-3.5
- **本地模型** — Ollama、llama.cpp、vLLM、SGLang
- **Claude** — 通过 API 代理
- **任何 OpenAI 兼容端点**

```python
# 示例：使用本地 Ollama 模型
llm = OpenAIChat(
    api_key="ollama",
    base_url="http://localhost:11434/v1",
    model="llama3"
)

# 示例：使用自定义端点
llm = OpenAIChat(
    api_key="your-key",
    base_url="https://your-custom-endpoint.com/v1",
    model="your-model"
)
```

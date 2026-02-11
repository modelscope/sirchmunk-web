---
title: "Sirchmunk v0.0.1 发布 — 首个版本"
summary: "Sirchmunk 首次发布，一个无需嵌入的智能搜索引擎。支持无索引检索、自进化知识簇、蒙特卡洛证据采样和实时 Web UI。"
date: 2026-01-22
authors:
  - me
tags:
  - 发布
  - Sirchmunk
  - 智能搜索
image:
  caption: 'Sirchmunk v0.0.1'
---

我们很高兴宣布 **Sirchmunk** 首个版本的发布 — 一款开源、无需向量嵌入的智能搜索引擎，能够将原始数据实时转化为自进化智能。

<!--more-->

## 什么是 Sirchmunk？

Sirchmunk 采用了与传统检索增强生成完全不同的方法。它不依赖静态向量嵌入和预计算索引，而是直接操作原始文件 — 无需任何预处理即可提供即时、完整保真度的检索。

## v0.0.1 核心功能

### 无索引检索
放入文件即刻搜索。无需向量数据库、无需 ETL 管线、无需预索引。Sirchmunk 利用 ripgrep-all 以流式方式搜索 100 多种文件格式。

### 自进化知识簇
知识随每次搜索不断积累。系统构建结构化的知识簇，随着多次查询验证从"萌芽"进化到"稳定"。

### 蒙特卡洛证据采样
Sirchmunk 不读取整个文档，而是使用探索-利用方法战略性地采样和评分文档区域 — 在最小化 LLM Token 成本的同时提取精确证据。

### ReAct 智能体自适应检索
当标准检索不足时，ReAct 智能体自主接管，通过迭代推理-行动循环探索替代检索策略，直到找到答案。

### Web UI
现代 Web 界面，支持实时流式聊天、来源引用、知识簇浏览和系统监控仪表盘。支持深色/浅色主题和双语本地化。

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

## 快速开始

```bash
pip install sirchmunk
```

查看完整文档，请参阅 [快速入门指南](/zh/docs/getting-started/)。

## 下一步

我们正在积极开发 MCP 集成、CLI 改进和知识持久化。敬请期待 v0.0.2！

---

*[GitHub 仓库](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

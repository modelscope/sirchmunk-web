---
title: 自定义
linkTitle: 自定义
weight: 1
---

## 扩展 Sirchmunk

Sirchmunk 围绕**开放封闭原则**设计 — 通过抽象扩展，而非修改。

### 自定义 LLM 供应商

任何 OpenAI 兼容的 API 端点均可直接使用。在 `.env` 中配置：

```bash
LLM_API_KEY=your-api-key
LLM_BASE_URL=https://your-provider.com/v1
LLM_MODEL=your-model-name
```

### 文件类型过滤

使用包含/排除模式控制搜索的文件范围：

```python
result = await searcher.search(
    query="database queries",
    paths=["./src"],
    include_patterns=["*.py", "*.sql"],
    exclude_patterns=["*test*", "*migration*"],
)
```

### 搜索深度

调整搜索深度和文件数量：

```python
result = await searcher.search(
    query="authentication flow",
    paths=["./src"],
    max_depth=5,           # 限制目录遍历深度
    top_k_files=10,        # 分析更少文件以提升速度
    keyword_levels=2,      # 更少的关键词粒度级别
)
```

### 知识管理

以编程方式访问和管理知识簇：

```python
# 知识簇存储在：
# {SIRCHMUNK_WORK_PATH}/.cache/knowledge/knowledge_clusters.parquet

# 使用 DuckDB 或 KnowledgeManager API 查询
```

## 环境变量参考

请参阅 [配置指南](/zh/docs/guide/configuration/) 了解完整的环境变量列表。

---
title: 项目结构
weight: 1
---

Sirchmunk 源代码组织和架构层次概览。

## 仓库布局

```text
sirchmunk/
├── src/
│   ├── sirchmunk/              # 核心 Python 包
│   │   ├── agentic/            # ReAct 智能体、工具、提示词
│   │   ├── api/                # FastAPI REST 端点
│   │   │   └── components/     # 历史、监控、设置存储
│   │   ├── cli/                # CLI 入口和 Web 启动器
│   │   ├── insight/            # 文本洞察提取
│   │   ├── learnings/          # 证据处理、知识库
│   │   ├── llm/                # LLM 接口（OpenAI 兼容）
│   │   ├── retrieve/           # 无索引检索引擎
│   │   ├── scan/               # 目录和文件扫描器
│   │   ├── schema/             # 数据模型（知识、上下文等）
│   │   ├── storage/            # DuckDB + Parquet 持久化
│   │   ├── utils/              # 工具（嵌入、分词器等）
│   │   ├── search.py           # 主搜索编排器
│   │   └── base.py             # 基类和抽象
│   └── sirchmunk_mcp/          # MCP 服务器包
│       ├── server.py           # MCP 服务器实现
│       ├── service.py          # 搜索服务层
│       └── tools.py            # MCP 工具定义
├── web/                        # Next.js 14 前端
│   ├── app/                    # 页面（首页、历史、知识、监控、设置）
│   ├── components/             # React 组件
│   ├── hooks/                  # 自定义 React hooks
│   └── lib/                    # 工具库（API、i18n、主题）
├── config/                     # 配置模板
├── scripts/                    # 辅助脚本
└── requirements/               # 按功能分类的依赖文件
```

## 架构层次

Sirchmunk 遵循严格的**关注点分离**模式，分为四个独立层次：

### 集成层
暴露 Sirchmunk 智能的外部接口：
- **MCP 服务器** (`sirchmunk_mcp/`) — AI 到 AI 通信
- **REST API** (`api/`) — 编程式 HTTP 访问
- **WebSocket** — 实时流式聊天
- **CLI** (`cli/`) — 命令行界面
- **Web UI** (`web/`) — 基于浏览器的界面

### 编排层
搜索管线协调器：
- **AgenticSearch** (`search.py`) — 多阶段搜索编排器
- **SearchContext** (`schema/search_context.py`) — 预算、状态和审计管理

### 智能层
证据提取和知识合成：
- **EvidenceProcessor** (`learnings/evidence_processor.py`) — 蒙特卡洛采样
- **KnowledgeBase** (`learnings/knowledge_base.py`) — 知识簇管理
- **ReActAgent** (`agentic/react_agent.py`) — 自主探索
- **OpenAIChat** (`llm/openai_chat.py`) — 统一 LLM 接口

### 存储层
持久化和缓存：
- **DuckDB** (`storage/duckdb.py`) — 内存分析数据库
- **KnowledgeStorage** (`storage/knowledge_storage.py`) — 基于 Parquet 的知识簇持久化

## 核心组件

| 组件 | 模块 | 描述 |
|------|------|------|
| **AgenticSearch** | `search.py` | 支持 LLM 增强检索的搜索编排器 |
| **KnowledgeBase** | `learnings/knowledge_base.py` | 将结果转化为结构化知识簇 |
| **EvidenceProcessor** | `learnings/evidence_processor.py` | 蒙特卡洛重要性采样证据提取 |
| **GrepRetriever** | `retrieve/text_retriever.py` | 高性能无索引文件搜索 |
| **DirScanner** | `scan/dir_scanner.py` | 目录结构分析 |
| **ReActAgent** | `agentic/react_agent.py` | 预算约束下的自主探索 |
| **OpenAIChat** | `llm/openai_chat.py` | 统一 LLM 接口（支持流式和使用量跟踪） |
| **MonitorTracker** | `api/components/monitor_tracker.py` | 实时系统指标 |

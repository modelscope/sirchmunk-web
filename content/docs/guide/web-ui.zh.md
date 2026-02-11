---
title: Web UI
weight: 4
---

Sirchmunk 的 Web UI 提供了用于实时聊天、知识分析和系统监控的现代界面。

## 设置

### 方案 1：单端口模式（推荐）

构建一次前端，然后从单个端口提供所有服务 — 运行时无需 Node.js。

```bash
# 构建 WebUI 前端（构建时需要 Node.js 18+）
sirchmunk web init

# 启动嵌入 WebUI 的服务器
sirchmunk web serve
```

**访问地址：** http://localhost:8584（API + WebUI 同一端口）

### 方案 2：开发模式

用于前端开发的热重载模式：

```bash
# 启动后端 + Next.js 开发服务器
sirchmunk web serve --dev
```

**访问地址：**
- 前端（热重载）：http://localhost:8585
- 后端 API：http://localhost:8584/docs

### 方案 3：传统脚本

```bash
# 启动前端和后端
python scripts/start_web.py

# 停止所有服务
python scripts/stop_web.py
```

## 功能特性

### 首页 — 聊天界面

首页提供流式聊天界面：

- **内嵌搜索日志** — 查看 Sirchmunk 如何找到答案
- **来源引用** — 每个结论都链接回源文件
- **多种聊天模式：**
  - 纯 LLM 对话
  - 文件 RAG 增强对话
  - 网络增强搜索
- **会话管理** — 保存和恢复对话

### 知识 — 知识簇分析

浏览和管理知识簇：

- 查看知识簇详情、证据单元和置信度分数
- 追踪生命周期状态（萌芽 → 稳定 → 弃用）
- 监控热度分数和查询历史

### 监控 — 系统仪表盘

实时系统健康和使用指标：

- 聊天活动和会话统计
- LLM Token 使用量和成本跟踪
- 知识簇增长趋势
- 系统资源监控

### 设置

通过 Web 界面配置 Sirchmunk：

- LLM API 密钥、基础 URL 和模型选择
- 搜索参数（深度、top-k、关键词级别）
- 环境变量管理

## 主题

Web UI 支持：

- **深色 / 浅色主题** — 通过主题开关切换
- **双语本地化** — 英文和中文

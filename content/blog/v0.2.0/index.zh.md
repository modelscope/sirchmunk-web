---
title: "Sirchmunk v0.2.0：LENS 论文发布、多路 DEEP 检索与大语料鲁棒性"
summary: "Sirchmunk v0.2.0 是一个重要里程碑 — LENS 研究论文已发布至 arXiv，检索引擎引入多路 DEEP 融合与有界检索成本不变量，系统全面采用泛化优先的设计理念。"
date: 2026-09-20
authors:
  - admin
tags:
  - Release
  - LENS
  - DEEP
  - Retrieval
image:
  caption: 'LENS 框架'
---

Sirchmunk v0.2.0 是项目的一个转折点。Sirchmunk 检索引擎背后的核心算法已在研究论文中形式化并发布至 arXiv，DEEP 搜索管线围绕多路融合进行了重构，系统现在强制执行严格的检索成本不变量，确保在任意规模的语料上都能保持可预测的性能。

<!--more-->

## LENS：研究论文

Sirchmunk 上下文搜索的理论基础已在 **"LENS: In-Context Search via Latent Evidence Exploration over Dynamic Raw Documents"**（[arXiv:2608.16185](https://arxiv.org/abs/2608.16185)）中形式化。

LENS 将检索重新表述为*预算约束证据定位*：给定查询和原始文档语料，系统在隐式证据空间上维护一个查询条件信念，通过提议策略和 LLM 相关性预言机迭代精炼 — 全程受显式 token 预算约束。

受控评估的关键结果：

- **500 题评估**：62.4% 精确匹配（EM），84.8% 证据召回率（对比 ReAct 基线 65.2% EM，但证据召回仅 50.4%）。
- **150 题 fullwiki 子集**（原始 Wikipedia 转储，零索引）：LENS 达到 43.3% EM，对比 ReAct 的 42.7% EM，且证据接地能力显著更强（84.0% vs. 70.7%）。

这些数据表明 LENS 以微小的准确率代价换取了显著更好的证据可追溯性 — 答案不仅正确，而且*可证地接地*于源材料。

## 多路 DEEP 检索

DEEP 模式经过了根本性重构。取代单一检索策略，现在并行运行 **五条互补的检索路径**：

1. **词法路径** — 基于关键词的内容匹配，采用 IDF 加权评分。
2. **实体路径** — 对命名实体、标识符和结构化值的精确匹配探测。
3. **目录路径** — 利用命名约定、路径层次和修改时间戳进行文件系统结构分析。
4. **结构路径** — 启发式文档树导航（v2），带有针对 DOCX、RST 等格式源的结构锚点。
5. **主题图路径** — 跨文档主题图路由，利用自进化知识图谱识别相关文档集群。

五条路径的结果通过**置信度加权倒数排名融合（RRF）**进行融合。**soft 路由收缩**机制在单条路径产生高置信度匹配时动态禁用低产出路径，在不损失答案质量的前提下降低延迟和 token 消耗。

![Sirchmunk 架构](Sirchmunk_Architecture.png "Sirchmunk v0.2.0 系统架构：多路 DEEP 检索与 LENS 框架集成。")

## 大语料鲁棒性

早期版本在超大、含归档文件的语料上可能卡顿或超时。v0.2.0 引入了**检索成本不变量** — 确保单文件和单次查询成本永远不会无界增长的硬约束：

- **`GREP_RGA_ADAPTERS`**：基于能力的适配器白名单。查询热路径上仅启用有界文档提取器（poppler、pandoc）；无界递归适配器（decompress、zip、tar、sqlite、ffmpeg）限制为离线提取。
- **`GREP_MAX_FILESIZE_MB`**：单文件大小上限。超过阈值的文件在实时查询中被跳过。
- **`GREP_TIERED_SCAN`**：对所有文件执行快速原生 `rg` 扫描，与限定富格式扩展名的 `rga` 扫描取并集，避免适配器调度遍历整棵文件树。
- **快速失败超时预算**：`GREP_TEXT_TIMEOUT` 用于文本扫描，`GREP_TIMEOUT` 用于富文档扫描；超时后搜索优雅降级为原生 `rg`，而非挂起。

目录扫描现已默认开启以增强文件名路由，硬 token 预算将每次查询限制在可配置额度内。

## 泛化优先设计

v0.2.0 对基准特定硬规则采取了原则性立场。硬编码的实体模式、固定列位置、仅限英语的停用词等基准绑定逻辑已被可泛化的替代方案取代：

- **接地数值校验**：计算类答案基于模型披露且证据接地的操作数进行确定性复算 — 完全语料无关。
- **可注入分词器**：分词器和词法策略通过依赖注入提供通用默认实现，而非嵌入模块内部。
- **语料自适应统计**：基于文档频率的自适应停用词剪枝替代固定词表，原生适配所有语言和领域。

每个替换行为都通过环境开关控制，默认启用新实现，并允许单项故障回滚。

## 知识图谱可视化

Web UI 新增基于 Cytoscape.js 的**交互式知识图谱**。图谱可视化自进化知识聚类及其语义关系，包括生命周期状态（Emerging、Stable、Meta）和边权重。用户可以探索、过滤和深入聚类拓扑，直观了解系统知识随使用的演化过程。

可视化背后，`KnowledgeEvolver` 编排了四阶段后台进化周期——连接与合并、边刷新、元聚类发现（基于 Leiden 社区发现算法）和全局更新——持续维护和整合知识图谱，且不阻塞查询。

![知识进化器架构](Knowledge_Evolver_Architecture.png "KnowledgeEvolver — 知识图谱维护的四阶段进化周期")

设计理念是观测驱动的：进化由实际搜索模式触发，而非预定义规则；源头保真始终被保护——系统改变的是寻找证据的导航路径，而非证据本身。完整动画演示请参见[知识进化展示](/zh/showcase/knowledge-graph/)。

## 其他改进

- **更广格式覆盖**：LOG、PPTX、XLSX 文件原生精确匹配回退，启发式文档树 v2（含 DOCX/RST）并以结构锚点引导证据抽取。
- **DeepSeek V4 兼容**：OpenAI 兼容客户端完整支持 DeepSeek V4 思考模式（`thinking_content`）。
- **大语料检索稳定性**：分层扫描、单文件匹配上限和适配器白名单的综合改进，消除了早期版本在大语料上的超时和卡顿问题。

## 开始使用

```bash
pip install --upgrade sirchmunk
```

或安装全部附加组件：

```bash
pip install "sirchmunk[all]"
```

- **文档**：[modelscope.github.io/sirchmunk-web/zh](https://modelscope.github.io/sirchmunk-web/zh/)
- **GitHub**：[github.com/modelscope/sirchmunk](https://github.com/modelscope/sirchmunk)
- **论文**：[arXiv:2608.16185](https://arxiv.org/abs/2608.16185)

---

*[GitHub 仓库](https://github.com/modelscope/sirchmunk) · [ModelScope](https://github.com/modelscope)*

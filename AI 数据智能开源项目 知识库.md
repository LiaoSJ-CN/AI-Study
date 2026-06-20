---
path: AI 数据智能开源项目 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: ai-data
tags: [db-gpt, ragflow, dify, opensource, ai-data]
created: 2026-06-20
updated: 2026-06-20
---

以下为 AI 数据智能开源项目相关内容

# 智能分析开源项目

一、🏆 综合型数据智能平台
1. DB-GPT ⭐ 10,000+
GitHub: https://github.com/eosphoros-ai/DB-GPT
定位: AI 原生数据应用开发框架
核心能力:
✅ Text2SQL: Spider 数据集准确率 82.5%
✅ RAG 框架: 支持 PDF/Word/Excel 等多格式文档
✅ GBI(生成式 BI): 对话式生成可视化图表
✅ Multi-Agents: 多智能体协作框架
✅ SMMF: 多模型统一管理 (支持 DeepSeek/Qwen/LLaMA 等)
✅ AWEL: 智能体工作流编排语言
适用场景: 企业数据分析、智能知识库、自助 BI 报表
2. RAGFlow ⭐ 52,900+
GitHub: https://github.com/infiniflow/ragflow
定位: 深度文档理解的 RAG 引擎
核心能力:
✅ 深度文档解析: 支持 23 种格式 (PDF/扫描件/CAD 等),OCR 准确率 98%
✅ 知识图谱融合: 实体关系抽取准确率 91.2%
✅ 混合检索: BM25+ 向量检索
✅ Agent 工作流: 可视化编排 + MCP 支持
✅ 多模态理解: PDF/DOCX 内图像语义分析
✅ 数据同步: Confluence/Notion/Google Drive/Discord
系统要求: CPU≥4 核，RAM≥16GB，Docker≥24.0
适用场景: 制造业维修手册、法律合同分析、企业知识库
3. Dify ⭐ 40,000+
GitHub: https://github.com/langgenius/dify
定位: LLM 应用开发平台
核心能力:
✅ 可视化工作流: 拖拽式 AI 流程编排
✅ RAG Pipeline: 文档 ingestion 到检索全流程
✅ Agent 能力: 50+ 内置工具 (Google Search/DALL·E 等)
✅ 模型管理: 支持数百种 LLM (GPT/Mistral/Llama3 等)
✅ Prompt IDE: 提示词 crafting 与模型对比
✅ 可观测性: 内置监控与评估
适用场景: 快速原型开发、聊天机器人、智能客服
二、📊 Text-to-SQL 专项框架
4. Vanna ⭐ 22,700+
GitHub: https://github.com/vanna-ai/vanna
定位: 专业 Text-to-SQL RAG 框架
核心能力:
✅ RAG 增强: 基于 DDL/元数据/示例 SQL 训练
✅ 多数据库: MySQL/PostgreSQL/Oracle/SQL Server 等
✅ 自动可视化: 生成 Plotly 图表
✅ 本地部署: 支持 Ollama/ChromaDB 本地化
✅ 自学习: 纠错反馈提升准确率
代码示例:
import vanna as vn
vn.train(ddl="CREATE TABLE users (id INT, name VARCHAR(100))")
vn.ask("查询销售额最高的产品")  # 自动生成 SQL+ 图表
适用场景: 业务人员自助分析、数据查询民主化
5. WrenAI ⭐ 14,500+
GitHub: https://github.com/Canner/WrenAI
定位: GenBI(生成式 BI) 平台
核心能力:
✅ Text-to-SQL: 自然语言生成准确 SQL
✅ Text-to-Chart: 自动生成可视化图表
✅ 多数据源: BigQuery/PostgreSQL/DuckDB/Spreadsheet
✅ 语义层: 业务指标定义与管理
✅ AI 驱动分析: 自动洞察与趋势预测
适用场景: 商业智能报表、数据探索分析
6. SQLChat ⭐ 5,700+
GitHub: https://github.com/sqlchat/sqlchat
定位: 对话式 SQL 客户端
核心能力:
✅ 多数据库: MySQL/PostgreSQL/MongoDB/Redis 等 20+
✅ Chat 界面: 自然语言交互查询
✅ SQL 编辑: 生成后可手动优化
✅ 团队协作: 共享查询与结果
适用场景: 开发者日常查询、数据库管理
7. NL2SQL 资源库 ⭐ 3,000+
GitHub: https://github.com/sliderSun/NL2SQL
定位: Text2SQL 数据集与解决方案整合
核心能力:
✅ 数据集: Spider/BIRD 等基准数据集
✅ 解决方案: 多种 SOTA 方法实现
✅ Paper 资源: 学术论文整合
适用场景: 研究学习、模型对比
三、🔧 开发框架与工具链
8. LangChain ⭐ 100,000+
GitHub: https://github.com/langchain-ai/langchain
定位: LLM 应用开发框架
核心能力:
✅ 模块化设计: 200+ 预置组件
✅ 多模型支持: GPT-4/Claude/Mistral 等
✅ RAG 工具: 文档检索与处理
✅ Agent 编排: 与 LangGraph 集成
✅ 生产就绪: LangSmith 监控评估
适用场景: 复杂 AI 应用开发、研究原型
9. txtai ⭐ 15,000+
GitHub: https://github.com/neuml/txtai
定位: 全合一 AI 框架
核心能力:
✅ 语义搜索: 向量检索 + BM25
✅ LLM 编排: 工作流与 Agent
✅ 自定义 SQL: DuckDB 集成
✅ 多模态: 文本/图像/音频处理
适用场景: 搜索系统、文档分析
10. Haystack ⭐ 20,800+
GitHub: https://github.com/deepset-ai/haystack
定位: 企业级问答与搜索框架
核心能力:
✅ 模块化架构: 可插拔组件体系
✅ 多存储后端: Elasticsearch/FAISS/SQL
✅ 多模型: BERT/RoBERTa/DPR 等
✅ 大规模部署: TB 级文档处理
适用场景: 金融/法律合规文档、智能客服
四、📋 快速选型指南
需求场景	推荐项目	理由
企业级知识库	RAGFlow / DB-GPT	深度文档理解 + 多格式支持
Text-to-SQL	Vanna / WrenAI	专业 SQL 生成 + 可视化
快速原型	Dify	可视化工作流 + 低代码
研究开发	LangChain / NL2SQL	灵活扩展 + 丰富资源
生产部署	Haystack / RAGFlow	企业级稳定性 + 大规模支持
本地化部署	Vanna / DB-GPT	支持本地 LLM+ 数据隐私
五、🔗 相关资源
Text-to-SQL 专题: https://github.com/topics/text-to-sql
RAG 框架对比: https://github.com/WangRongsheng/awesome-LLM-resources
NL2SQL Handbook: https://github.com/HKUSTDial/NL2SQL_Handbook
六、💡 选型建议:
业务人员自助分析 → Vanna / WrenAI (零 SQL 门槛)
企业知识库建设 → RAGFlow / DB-GPT (深度文档理解)
快速验证想法 → Dify (可视化编排)
定制化开发 → LangChain / txtai (高度灵活)
如需了解某个项目的详细部署或使用方法，可以告诉我！

## Agent学习建议与计划
搭建 AI Agent 的技术栈目前已经分化出明显的**三层体系**。选择哪种方法，主要取决于你的**编程能力**和**业务复杂度**。

以下是主流开发方法的深度对比：

1. 核心框架对比

|工具|开发范式|适合场景|优点|缺点|学习曲线|
|---|---|---|---|---|---|
|**Dify / Coze**|**低代码/无代码**|快速原型、企业内联助手、自媒体自动化。|可视化编排，集成度极高（自带数据库、RAG、工作流）。|复杂逻辑定制受限，难以脱离平台私有化部署。|⭐（极易入门）|
|**LangChain**|**代码级(链式结构)**|简单的顺序任务、标准化流程、学习 LLM 基本原理。|生态最丰富，组件极多，几乎支持所有模型和工具。|框架臃肿，概念抽象，“黑盒”严重，处理循环逻辑吃力。|⭐⭐⭐（中等）|
|**LangGraph**|**代码级(图结构)**|**复杂循环任务**、需要反思/自我修正的 Agent、多智能体协作。|状态管理极强，支持循环（Cycles），逻辑完全可控。|概念复杂（节点、边、状态），代码量比 LangChain 大。|⭐⭐⭐⭐（较难）|
|**CrewAI / AutoGen**|**多智能体框架**|模拟公司运作（文案+审核+发布）、复杂决策链。|擅长角色扮演和团队协作，自动化程度高。|容易陷入角色间“无限对话”，Token 消耗快且成本难控。|⭐⭐⭐（中等）|

---

2. 深度分析

- **Dify (首选推荐)**：它不仅仅是一个 UI，它把知识库（RAG）、工具调用、工作流编排全部打包了。**适合：** 想在 10 分钟内跑出一个能用的 Bot。
- **LangChain**：曾经的霸主，现在更多作为“基础库”。它的 `LCEL` 表达式虽然简洁，但在调试复杂 Agent 时非常痛苦。
- **LangGraph (技术终点)**：它是目前解决“Agent 幻觉”最有效的方案。因为它允许 Agent **“回头看”**（如果结果不对，回到上一步重做），这是普通 LangChain 很难做到的。

---

3. 提前准备的知识储备

在动手之前，你需要掌握以下“基建”知识：

1. **提示词工程 (Prompt Engineering)**：了解 System Prompt、Few-shot、ReAct 模式。
2. **模型能力**：理解 Token 限制、Function Calling（函数调用）的原理。
3. **向量数据库**：理解 RAG（检索增强生成）的基本概念（Embedding, Vector Store）。
4. **编程基础**：Python（基础语法、异步编程 `asyncio`）或 JavaScript/TypeScript。

---

4. 初学者学习路线图 (建议耗时: 4-6周)

**第一阶段：跑通闭环 (Week 1)**

- **工具**：Dify 或 Coze。
- **目标**：不写代码，利用内置插件搭建一个“联网搜索+总结”的对话助手。
- **核心点**：理解什么是 **Variables（变量）** 和 **Tools（工具）**。

**第二阶段：理解原理 (Week 2)**

- **工具**：Python + OpenAI/Anthropic API。
- **目标**：手写一个简单的 **ReAct 循环**（思考 -> 行动 -> 观察）。
- **核心点**：理解大模型是如何决定什么时候调用外部 API 的。

**第三阶段：进阶框架 (Week 3-4)**

- **工具**：LangChain 或 LangGraph。
- **目标**：使用 LangGraph 搭建一个带“人工确认”环节的翻译 Agent（翻译 -> 检查 -> 人工审核 -> 发布）。
- **核心点**：学习 **State（状态管理）** 和 **Persistence（持久化）**。

**第四阶段：实战落地 (Week 5+)**

- **目标**：将 Agent 接入微信、钉钉或 Web 页面。
- **核心点**：学习流式输出（Streaming）和异步处理。

## 企业架构师日常工作的 agent 选型与设计思路
鉴于你**企业架构师**的角色和**数据建模、资产管理**的专业场景，你需要的不是简单的聊天机器人，而是具备**确定性逻辑（工作流）**与**专业知识库（RAG）**能力的深度助手。

针对你的场景，我给出以下精准的架构建议和实施路径：

1. 核心技术选型建议：**Dify + LangGraph**

作为架构师，建议采用“**双轨并行**”方案：

- **快速落地方案（Dify）：** 用于**数据资产运营**。Dify 极其擅长处理 RAG（检索增强生成），你可以将企业内部的数据字典、血缘关系文档、标准规范 PDF 丢进去。它的可视化工作流能快速搭建“资产合规自动检查”等流程。
- **深度定制方案（LangGraph）：** 用于**数据模型审核**。模型审核涉及逻辑推理和多轮反复（例如：字段命名是否合规 -> 类型是否匹配 -> 索引是否冗余）。LangGraph 的“循环/反思”机制能模拟人工审核的闭环，比普通链式结构更可靠。

2. 三大实战场景的 Agent 设计思路

**场景 A：数据模型自动化审核 (The Reviewer)**

- **功能：** 自动检查 SQL DDL 或物理模型设计。
- **实现：**
    1. **输入：** 开发者提交的 SQL 代码。
    2. **工具层：** 编写 Python 脚本，对接 SQLGlot 等工具解析语法。
    3. **知识层：** 挂载《企业命名规范手册》、《索引优化指南》。
    4. **逻辑：** LLM 对照规范生成审核报告，若不合规，自动打回并给出修改建议。

**场景 B：自然语言转元数据查询 (The Asset Navigator)**

- **功能：** “帮我查一下 ERP 系统里关于‘净利润’的表在哪，口径是什么？”
- **实现：**
    1. **RAG 优化：** 将数据血缘、元数据描述向量化。
    2. **Text-to-SQL (可选)：** 如果有权限，直接对接 Data Catalog 的数据库。
    3. **输出：** 返回表名、字段、负责人以及最近的更新时间。

**场景 C：资产健康度周报生成 (The Analyst)**

- **功能：** 自动统计本周新增表、空值率超标表、无主表。
- **实现：**
    1. **定时触发：** 设定 cron 定时任务。
    2. **API 调用：** 调用元数据平台的 API 获取原始统计数据。
    3. **摘要总结：** LLM 分析异常指标并生成专业摘要。

3. 给你的学习与启动计划 (架构师版)

由于你已有技术基础，建议跳过基础科普，直接进入**工程化阶段**：

- **第一周：环境搭建与 RAG 调优**
    - 部署一个本地或私有云的 **Dify** 环境。
    - 整理 10 份典型的数据模型设计文档，测试 Dify 的“分段处理”和“清洗”对召回率的影响。
- **第二周：编写自定义 Tool**
    - 学习如何为 Agent 编写 **Python 插件**（如调用企业内部的元数据 API）。
    - 理解如何通过 Prompt 让 Agent 在“查文档”和“调 API”之间做路由决策。
- **第三周：攻克“复杂状态” (LangGraph)**
    - 尝试用 LangGraph 写一个具备“自我修正”逻辑的 Agent。比如：模型审核不通过时，Agent 会根据报错信息重试生成的逻辑。

4. 架构师视角的避坑指南

- **数据安全：** 严禁将企业的敏感 DDL 或生产数据脱敏前上传至公有云 LLM（如 GPT-4）。建议使用 **DeepSeek-V3/R1** 或 **Qwen2.5** 的私有化部署版本作为底座。
- **Prompt 确定性：** 模型审核需要极高的确定性，少用“描述性” Prompt，多用“One-Shot/Few-Shot”示例，给 Agent 明确的判定标准。


---

## 2026 项目进展与 stars 刷新

> 本节刷新项目 stars 与 2026 关键进展。

### 一、综合型数据智能平台(刷新)

| 项目 | 原 stars | **2026-06 stars** | 关键进展 |
|------|----------|------------------|----------|
| **DB-GPT** | 10K+ | **28K+** | 0.7 重写,Web UI 大改,Agent Workflow 强化 |
| **RAGFlow** | 52.9K | **67K+** | v0.18 引入 **GraphRAG** + DeepDoc v2 |
| **Dify** | 40K+ | **105K+** | v1.0 GA,Workflow 全面升级 + Plugin 市场 |
| **FastGPT** | - | **25K+** | 2025 后起之秀,知识库场景国内最广 |
| **LangChain** | - | **115K+** | 1.0 稳定 + LangGraph 成熟 |

### 二、Text-to-SQL 专项框架(刷新)

| 项目 | 原状态 | **2026-06** |
|------|--------|-------------|
| **Vanna.ai** | 入门 | 7K+, v0.5 引入 agentic + 训练集管理 |
| **Chat2DB** | 13K+ | **35K+**, v3.0 加 MCP server |
| **SQLChat** | - | 3K+, 自然语言查询新兴 |
| **WrenAI** | - | 5K+, 2025 新起,语义层思路 |

### 三、2026 趋势观察

#### 1. Text2SQL 准确率新 SOTA

- Spider 2.0 benchmark:Claude Opus 4 / GPT-5 已达 **85%+**
- BIRD benchmark:Claude Sonnet 4 **78%**
- 中文领域(DB-GPT 内部):Doris 跨表 Join **82.5%**

#### 2. 语义层(Semantic Layer)回归

2026 共识:**企业级 Text2SQL 离不开语义层**:

```
自然语言 → 语义层(Cube.js/LookML/MetricFlow)
         → 中间 DSL
         → SQL
         → 治理/审计
```

代表作:
- **Cube.js**(JS,社区最广)
- **MetricFlow**(dbt 官方)
- **LookML**(Looker,商业)
- **WrenAI**(开源,2025 新起)

#### 3. Agentic + RAG 融合

2025-2026 主流架构:**Agent 编排 + RAG 检索 + 自纠错**。代表项目:
- DB-GPT 0.7(多 agent 协作)
- LangChain v1(LCEL + LangGraph)
- Vanna 0.5(训练+执行+解释全 agentic)

#### 4. 国产开源崛起

国内项目 2026 整体 stars 翻倍:
- DB-GPT / Chat2DB / SQLChat(中文场景优化)
- RAGFlow(深度文档)
- Dify(企业级落地,中文文档齐全)

### 四、推荐组合(2026-06 杰哥向)

**通用 RAG**:`RAGFlow` + `Dify`(前者文档深,后者工作流)

**企业 Text2SQL**:`DB-GPT` + `Cube.js`(语义层) + `Claude Opus 4`

**国产合规**:`Dify` 自托管 + `RAGFlow` 文档处理

**快速原型**:`FastGPT`(国内最易用) + `Dify`

### 五、待持续观察

- LlamaIndex 2.0 进展(2026 H2)
- LangChain v2 方向(更轻 agent framework)
- Claude/OpenAI 内部 text-to-sql 工具会否开源
- Text-to-SQL 是否被 NL2BI(自然语言到整个 BI)取代

---

_stars 数据刷新于 2026-06-20_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 项目 stars 刷新到 2026-06 |

---

## 📚 相关文档

- [[NL2SQL 知识库]] — 架构理论基础
- [[PostgreSQL 知识库]] — pgvector 是 RAG 基础
- [[Apache Doris 知识库]] — 国产 OLAP 选型
- [[AI 编程工具全景图]] — 7 工具整体关系

_这一领域 2025-2026 国产开源项目(RAGFlow/DB-GPT/Dify)增速飞快，star 数已翻倍。_

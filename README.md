---
path: README.md
workspace: ~/Documents/obsidian/ai-study/
category: index
tags: [readme, index, overview]
created: 2026-06-22
updated: 2026-06-22
---

# AI 学习知识库

> AI 编程工具 × 数据工程 实践沉淀

## 简介

本知识库收录 AI 编程工具、Agent 框架、数据工程组件的实践笔记,包括:

- **AI 编程工具全景** — 从终端 agent 到 IDE 增强,横向对比主流工具的定位、特性、适用场景
- **数据工程工具栈** — OLAP/OLTP 数据库安装、配置、实践
- **AI × 数据** — Text-to-SQL、RAG、Agentic BI 等交叉领域的开源项目与方法论
- **Skill 系统对比** — 不同 AI 编程工具的 skill 机制差异与协同方案

## 文档索引

### 入口

- [AI 编程工具全景图](AI 编程工具全景图.md) — 8 个主流 AI 编程工具横向对比,搞清**谁做什么、何时用谁**
- [AI 术语小白指南](AI 术语小白指南.md) — AI 编程常见概念扫盲(MCP / Skill / Agent / Hooks / RAG ...)

### AI 编程工具(8 篇)

| 工具 | 定位 | 文档 |
|------|------|------|
| OpenClaw | AI 助手运行时 / Gateway | [OpenClaw 知识库](OpenClaw%20知识库.md) |
| Claude Code | Anthropic 终端编程 agent | [Claude Code 知识库](Claude%20Code%20知识库.md) |
| OpenCode | 多 provider 终端 agent | [OpenCode 知识库](OpenCode%20知识库.md) |
| Hermes | Nous Research 自我进化 agent | [Hermes 知识库](Hermes%20知识库.md) |
| ECC | Everything Claude Code 增强套件 | [ECC 知识库](ECC%20知识库.md) |
| Superpowers | 7 阶段方法论 | [Superpowers 知识库](Superpowers%20知识库.md) |
| Agent Skills | Google 工程师实践 skill 集合 | [Agent Skills 知识库](Agent%20Skills%20知识库.md) |
| CodeGraph | 本地代码图谱(MCP-native) | [CodeGraph 知识库](CodeGraph%20知识库.md) |

### AI × 数据(2 篇)

- [AI 数据智能开源项目 知识库](AI%20数据智能开源项目%20知识库.md) — DB-GPT / RAGFlow / Dify / Vanna 等开源项目盘点
- [NL2SQL 知识库](NL2SQL%20知识库.md) — Text-to-SQL 架构、语义层、主流方案对比

### 数据工程(2 篇)

- [Apache Doris 知识库](Apache%20Doris%20知识库.md) — macOS 安装、配置、运维
- [PostgreSQL 知识库](PostgreSQL%20知识库.md) — 安装、pgvector、SQL 实践

### Skill 系统

- [Skill 系统对比](Skill%20系统对比.md) — OpenClaw / Claude Code / ECC / Hermes 四套 skill 机制差异与协同

### 编辑器

- [VS Code 知识库](VS%20Code%20知识库.md) — macOS 安装、配置、AI 扩展

## 阅读路径建议

| 身份 | 起点 | 延伸 |
|------|------|------|
| AI 编程入门 | AI 术语小白指南 → AI 编程工具全景图 | 选一个工具深入 |
| 多 harness 玩家 | AI 编程工具全景图 → Skill 系统对比 | 各工具知识库 |
| 数据工程师 | AI 数据智能开源项目 → NL2SQL | 选一个项目落地 |
| macOS 运维 | Apache Doris / PostgreSQL / VS Code | - |

## 文档约定

- 所有文档使用 Obsidian 双向链接 `[[]]`,适合在 Obsidian 中阅读
- frontmatter 包含 `path` / `workspace` / `category` / `tags` / `created` / `updated`
- 命令示例使用占位符(`<user>` / `555-0100` / `10.20.30.40`),替换为实际值即可使用
- 工具标准路径(`~/.claude/` / `~/.hermes/` / `~/doris/`)保留原样,便于读者对照
- "工具评估"章节标注评估日期,反映该时间点的工具状态

## 仓库结构

```
.
├── README.md
├── AI 编程工具全景图.md
├── AI 术语小白指南.md
├── Agent Skills 知识库.md
├── Claude Code 知识库.md
├── CodeGraph 知识库.md
├── ECC 知识库.md
├── Hermes 知识库.md
├── OpenClaw 知识库.md
├── OpenCode 知识库.md
├── Superpowers 知识库.md
├── Skill 系统对比.md
├── AI 数据智能开源项目 知识库.md
├── NL2SQL 知识库.md
├── Apache Doris 知识库.md
├── PostgreSQL 知识库.md
└── VS Code 知识库.md
```

## 贡献

欢迎通过 Issue / PR 补充工具评测、修正错误、补充新工具。

## 许可

MIT License

---
path: AI 编程工具全景图.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: index
tags: [ai, tools, panorama]
created: 2026-06-20
updated: 2026-06-20
---

# AI 编程工具全景图

> 把杰哥当前在用的 7 个 AI 编程工具(OpenClaw / Claude Code / OpenCode / Hermes / ECC / Superpowers / CodeGraph)放到一张图上,说清**谁做什么、不做什么、何时用谁**。

**更新日期**:2026-06-20
**目标读者**:杰哥(也适合其他多 harness 玩家)

---

## 一句话定位

| 工具 | 一句话 | 它**不**做什么 |
|------|--------|----------------|
| **OpenClaw** | AI 助手运行时 + 网关 | 不专注写代码 |
| **Claude Code** | 终端 AI 工程师 | 不做网关/渠道 |
| **OpenCode** | 终端 AI 工程师(Claude Code 替代/补充) | 不做网关/记忆 |
| **Hermes Agent** | 自我成长型 agent | 不做 IDE 集成 |
| **ECC** | Claude Code 的"操作系统" | 不是独立工具,只在 Claude Code 里跑 |
| **Superpowers** | 工程化方法论框架 | 不是工具,是纪律 |
| **CodeGraph** | 本地代码搜索引擎 | 不做规划,只做查询 |

---

## 关系拓扑图

```
                    ┌──────────────────────┐
                    │     OpenClaw 2026.6.8 │  ← 总入口
                    │  (Gateway + Skills)   │
                    └──────────┬───────────┘
                               │ spawns / routes
              ┌────────────────┼────────────────┐
              ↓                ↓                ↓
       ┌──────────┐     ┌──────────┐     ┌──────────┐
       │飞书/TG 等 │     │Webchat/UI│     │TUI/CLI  │
       └──────────┘     └──────────┘     └──────────┘
              │                │                │
              └────────────────┴────────────────┘
                               ↓ 调用
                    ┌──────────────────────┐
                    │   Agent Runtime 层    │
                    ├──────────────────────┤
                    │ • Claude Code 2.1.81  │  ← 主力编程
                    │ • OpenCode 240K ⭐    │  ← 备用编程
                    │ • Hermes Agent        │  ← 自我学习
                    └──────────┬───────────┘
                               │ 增强(可叠加)
              ┌────────────────┼────────────────┐
              ↓                ↓                ↓
       ┌────────────┐   ┌────────────┐   ┌────────────┐
       │    ECC     │   │ Superpowers│   │  CodeGraph │
       │ 操作层     │   │ 方法论     │   │ 搜索引擎   │
       │ 67+271+92  │   │ 7 阶段流程 │   │ MCP 查询   │
       └────────────┘   └────────────┘   └────────────┘
```

---

## 详细职责划分

### 1. OpenClaw — 总入口

**核心价值**:把"所有 AI 入口"统一到一个 Gateway。

**关键能力**:
- 多渠道接入(飞书/TG/微信/Slack/Discord…)
- 技能市场(76 个本地 skill,47 ready)
- 记忆系统(MEMORY.md + 每日记录 + promote)
- 定时任务(cron)
- 插件系统(v2026.6 新独立)
- MCP 配置 + channel bridge

**典型场景**:
- "在飞书里问 AI 一个问题"
- "用 skill 自动整理今天的对话"
- "每周一自动整理 MEMORY.md"

**不擅长**:
- 写大型代码(可以但不专)
- 深度技术研究(可以但不专)

---

### 2. Claude Code — 主力编程

**核心价值**:Anthropic 官方 CLI agent,2026 爆发年(176 次 changelog)。

**关键能力(2026 新)**:
- **Routines** — 持久化多步执行
- **Auto Mode** — 无询问自动执行
- **Ultrathink** — 深度思考模式
- **`/ultrareview`** — 超深度 code review
- **Subagent 5 层嵌套** — 复杂任务可分层
- **Hooks** — 生命周期事件钩子
- **Plugins** — 跨 harness 共享

**典型场景**:
- "用 TDD 重构这个模块"
- "跑完 /ultrareview 看有没有问题"
- "在 commit 前跑我的 precommit routine"

**不擅长**:
- 多渠道消息(那不是它的活)
- 长期记忆(它的 `/memory` 是单 session 的)

**详细**:[[Claude Code 知识库]]

---

### 3. OpenCode — 备用/多 provider 编程

**核心价值**:跟 Claude Code 同类,但是**支持任意 LLM Provider**(Claude/GPT/Gemini/DeepSeek/智谱/MiniMax…)。

**关键能力**:
- 多模型切换(`opencode run --model <id>`)
- 同样的 agent/subagent 架构
- 桌面/TUI/IDE 多端
- 配置灵活(provider 自定义)

**典型场景**:
- "用 GPT-4o 看看跟 Claude 答案差多少"
- "跑国内模型(智谱/MiniMax)试错"
- "用本地 Ollama 模型离线写代码"

**vs Claude Code**:
- ✅ 模型灵活(任意 provider)
- ✅ 开源(opencode.ai)
- ❌ 工具市场没 Claude Code 丰富
- ❌ 2026 爆发性新特性滞后

**详细**:[[OpenCode 知识库]]

---

### 4. Hermes Agent — 自我学习

**核心价值**:**用得越久越聪明**。自动沉淀 skill、优化记忆、跨会话召回。

**关键能力**:
- 学习闭环(每会话自动提炼)
- 长期记忆(用户画像、技能库)
- 跨平台消息(跟 OpenClaw 重叠)
- Web Dashboard

**vs OpenClaw**:
- OpenClaw:**自托管 AI 网关**(基础设施)
- Hermes:**自我成长 Agent**(应用层)
- **互补**:OpenClaw 跑消息入口,Hermes 跑需要深度学习/记忆的复杂任务

**典型场景**:
- "跑一周后看自动学到了什么"
- "用记忆系统管理长期项目"

**详细**:[[Hermes 知识库]]

---

### 5. ECC(Everything Claude Code)— Claude Code 的"操作系统"

**核心价值**:**67 agents + 271 skills + 92 commands + 29 hooks + 14 MCP servers**,一次装好全功能。

**关键能力**:
- 跨 harness 兼容(Claude Code/Codex/Cursor/OpenCode/Gemini/Copilot/Kimi/Hermes…)
- AgentShield 安全扫描(102 条规则)
- `/plan`、`/code-review`、`/build-fix`、`/learn`、`/checkpoint` 等
- Memory 优化
- Continuous learning

**典型场景**:
- "想用 Claude Code 但又想要完整工作流"
- "想跨多个 IDE 保持同一套规范"
- "想要现成的 agents(plan/code-review/test-writer)直接用"

**vs Superpowers**:
- ECC:操作层(什么都有,很全)
- Superpowers:方法论(只管流程纪律,更轻)

**详细**:[[ECC 知识库]]

---

### 6. Superpowers — 工程化方法论

**核心价值**:**7 阶段工作流**(Brainstorm → Spec → Plan → TDD → Subagent → Review → Finalize),强制测试门槛。

**关键能力**:
- `brainstorming` — 先聊清楚再写
- `writing-plans` — 任务分解到 2-5 分钟粒度
- `test-driven-development` — TDD 红色/绿色/重构
- `systematic-debugging` — 4 阶段根因分析
- `verification-before-completion` — 完成前必验证
- **拒绝 ship 没有测试的代码**

**v5.1.0 变更**:
- 移除 `/brainstorm`、`/write-plan` 等老 slash command
- 改用 skill 触发(更标准化)
- OpenCode bootstrap 缓存优化

**典型场景**:
- "中型以上项目,需要严格工程纪律"
- "团队多人协作,需要统一方法论"
- "急性子会嫌慢,愿意等的人受益巨大"

**详细**:[[Superpowers 知识库]]

---

### 7. CodeGraph — 本地代码搜索引擎

**核心价值**:**预索引代码知识图谱**,agent 用 query 代替 grep/Read,**省 token 50%+**。

**关键能力**:
- 自动同步(文件改动立即更新)
- 100% local(数据不出本机)
- MCP-native(任何 harness 都能用)
- 4 个核心 tool:`codegraph_explore` / `_search` / `_callers` / `_node`

**典型场景**:
- "中型以上代码库,Claude Code 探索慢"
- "多人协作,需要一致的代码知识图谱"
- "多 harness 切换(Claude Code ↔ Cursor ↔ Codex)"

**vs 传统工具**:
- ❌ GitDiagram/FalkorDB:画给人看,agent 查不了
- ❌ ECC codemaps:静态 markdown,实时性差
- ✅ CodeGraph:动态、agent 原生、可查询

**详细**:[[CodeGraph 知识库]]

---

## 决策树:用哪个?

```
"我要写新功能"
  ├─ 简单改动(1-2 文件)?  →  Claude Code / OpenCode 直接写
  └─ 复杂特性(多文件)?   →  Superpowers(流程) + ECC(agents) + CodeGraph(查询)
                                ↓
                          Claude Code 跑(主力) / OpenCode 跑(备选)

"我要做架构设计"
  └─ Superpowers `brainstorming` + `writing-specs`

"我要探索陌生代码库"
  └─ 先 CodeGraph init,然后 Claude Code 的 @explore

"我要做长期项目,跨多天"
  └─ Hermes(记忆) + Claude Code(主力) + ECC(规范化)

"我要在飞书/TG 里发消息"
  └─ OpenClaw 路由

"我要做快速实验,试不同模型"
  └─ OpenCode(多 provider 切换)

"我想要 Agent 自动沉淀经验"
  └─ Hermes + OpenClaw 记忆(都有)
```

---

## 组合方案

### 方案 A:主力 + 插件(推荐日常)

```
Claude Code(主力编程)
  + ECC(操作层 + 271 skills)
  + Superpowers(7 阶段工作流)
  + CodeGraph(代码库搜索)
```

**适合**:中型以上项目,严格工程纪律

### 方案 B:多模型探索(实验/选型)

```
OpenCode(主力,任意模型)
  + CodeGraph(本地代码查询)
  + 不装 ECC/Superpowers(避免冲突)
```

**适合**:对比不同模型、研究新方法

### 方案 C:OpenClaw 全家桶(消息入口 + 学习)

```
OpenClaw(总网关)
  + 内置 Claude Code(调起来跑)
  + Hermes(长期记忆)
  + 76 个本地 skills
  + 飞书/微信/钉钉 多渠道
```

**适合**:日常通用工作流,消息入口为主,编程为辅

### 方案 D:企业严肃开发(团队/长期)

```
Claude Code(主力)
  + ECC(统一规范,跨人/跨 IDE)
  + Superpowers(强制 TDD/Review)
  + CodeGraph(代码库一致性)
  + OpenClaw(汇总学习成果,沉淀到团队 memory)
```

**适合**:团队 3+ 人,长期维护项目

---

## 已知坑 & 注意事项

### 1. ECC 的 hooks 可能跟自定义 hook 冲突

ECC 默认装 29 个 hooks。如果你有自己的 hook,先关 ECC 的(`/plugin disable ecc@ecc hooks`)再加自己的,避免重复触发。

### 2. Subagent 5 层嵌套 token 爆炸

Claude Code v2 末支持 5 层 subagent 嵌套。**实际 2-3 层就够**,5 层能烧几百万 token 一次性。

### 3. Superpowers v5.1.0 移除了老的 `/brainstorm` 等命令

如果你的 Claude Code 是旧版,装了 Superpowers 后用 `/brainstorm` 会报错。统一改用 skill 触发(直接说 "use brainstorming")。

### 4. OpenClaw cron job 升级后要 `doctor --fix`

升级 OpenClaw 后,cron 存储格式可能变了。跑 `openclaw doctor --fix` 自动归一化。

### 5. CodeGraph 首次建图慢

中型 monorepo(几千文件)首次建图 5-15 分钟。建好之后秒级查询。

### 6. Hermes 命令的 API Key 必须在 .env

`hermes config set KIMI_API_KEY xxx` **无效**,必须 `echo 'KIMI_API_KEY=xxx' >> ~/.hermes/.env`。

### 7. 同一台机器用 Claude Code + Cursor 跑 ECC

ECC 默认数据 root 是 `~/.claude`,Cursor 跑 ECC 会覆盖 Claude Code 的数据。**必须给 Cursor 配独立 root**。

---

## 升级路线(2026-06 → 2026 末)

按"投入产出比"排序的安装顺序:

| 优先级 | 工具 | 投入 | 产出 |
|--------|------|------|------|
| 1️⃣ | **CodeGraph** | 5 分钟装好 | 探索代码库省 50% token |
| 2️⃣ | **Superpowers** | 装 plugin | 强制方法论,减少返工 |
| 3️⃣ | **Claude Code 子命令** | 0(已装) | `/ultrareview`/`/context` 直接用 |
| 4️⃣ | **OpenClaw cron 整理** | 10 分钟 | 自动化日常 |
| 5️⃣ | **ECC** | 30 分钟配置 | 完整 agents 库,跨 harness 一致 |
| 6️⃣ | **Hermes** | 1 小时安装 | 长期学习能力 |
| 7️⃣ | **OpenCode 备用** | 10 分钟 | 多模型对比能力 |

---

## 速查卡片(可截图保存)

```
┌─ OpenClaw  ─── 总入口/网关/多渠道/记忆 ─┐
│                                              │
│   ┌─ Claude Code  ── 主力编程 ──┐           │
│   │   + ECC      271 skills     │           │
│   │   + Superpowers 7 阶段流程  │           │
│   │   + CodeGraph  知识图谱     │           │
│   └────────────────────────────┘           │
│                                              │
│   ┌─ OpenCode   ── 多 provider ──┐         │
│   └──────────────────────────────┘          │
│                                              │
│   ┌─ Hermes     ── 自我学习 ─────┐          │
│   └──────────────────────────────┘          │
└──────────────────────────────────────────────┘
```

---

## 相关文档

- [[OpenClaw 知识库]] — Gateway + 命令清单
- [[Claude Code 知识库]] — Claude Code 完整使用
- [[OpenCode 知识库]] — OpenCode 详细
- [[Hermes 知识库]] — Hermes 详细
- [[ECC 知识库]] — ECC 详细
- [[Superpowers 知识库]] — Superpowers 详细
- [[CodeGraph 知识库]] — CodeGraph 详细

---

## 📚 知识库完整索引（15 个文档）

> 本知识库共 15 个文档，按主题分组。

### 🛠️ AI 编程生态（8 个）

| 文档 | 主题 | 推荐读谁 |
|------|------|---------|
| [[Claude Code 知识库]] | Claude Code 命令+配置+2026 新特性 | 必读 |
| [[OpenCode 知识库]] | OpenCode 多 provider 编程 | 必读 |
| [[Hermes 知识库]] | Hermes Agent 自我学习 | 进阶 |
| [[ECC 知识库]] | ECC 操作层(67+271+92) | 进阶 |
| [[Superpowers 知识库]] | 7 阶段方法论 | 进阶 |
| [[CodeGraph 知识库]] | 本地代码搜索引擎 | 进阶 |
| [[Skill 系统对比]] | 4 套 skill 系统对比 | 进阶 |
| **本文件** (AI 编程工具全景图) | **7 工具整体关系图** | **主入口** |

### 🚪 OpenClaw 网关（1 个）

| 文档 | 主题 | 推荐读谁 |
|------|------|---------|
| [[OpenClaw 知识库]] | OpenClaw v2026.6.8 完整用法 + Workboard | 必读 |

### 📚 基础知识（5 个）

| 文档 | 主题 | 推荐读谁 |
|------|------|---------|
| [[AI 术语小白指南]] | 入门 + 2026 新词条 | 入门必读 |
| [[Apache Doris 知识库]] | Doris 4.x 安装+湖仓 | 按需 |
| [[PostgreSQL 知识库]] | PG 17/18 + pgvector | 按需 |
| [[NL2SQL 知识库]] | Text-to-SQL 架构+2026 SOTA | 按需 |
| [[AI 数据智能开源项目 知识库]] | DB-GPT/RAGFlow/Dify | 按需 |

### 🔧 开发工具（1 个）

| 文档 | 主题 | 推荐读谁 |
|------|------|---------|
| [[VS Code 知识库]] | VSCode 安装+AI 扩展 | 入门 |

### 阅读路径建议

- **新人 1 周**：AI 术语小白 → Claude Code 知识库 → OpenClaw 知识库 → 全景图（本文件）
- **严肃项目**：Claude Code 知识库 + ECC + Superpowers + CodeGraph
- **多模型实验**：OpenCode 知识库 + 全景图 C 方案
- **AI + 数据**：NL2SQL架构 → 智能分析开源项目 → Doris/PG 知识库

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 新建。把 7 个工具(OpenClaw/Claude Code/OpenCode/Hermes/ECC/Superpowers/CodeGraph)的关系图谱化,补 4 套组合方案(A/B/C/D) |
| 2026-06-20 | 加完整 15 文件索引表 + 4 套阅读路径 |


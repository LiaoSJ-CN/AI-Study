---
path: Agent Skills 知识库.md
workspace: ~/Documents/obsidian/ai-study/
category: ai-coding
tags: [agent-skills, addy-osmani, google, skill-framework, engineering-practice]
created: 2026-06-22
updated: 2026-06-22
---

# Agent Skills 知识库

> Addy Osmani(Google Chrome 团队工程总监)开源的 **24 个生产级工程 skill 集合**,把 Google 工程文化打包成 AI agent 能直接执行的工作流,覆盖从需求到上线的全 SDLC。

**仓库**:[github.com/addyosmani/agent-skills](https://github.com/addyosmani/agent-skills)
**作者**:Addy Osmani(Google Chrome 团队工程总监)
**许可证**:MIT
**定位**:Production-grade engineering skills for AI coding agents
**更新时间**:2026-06-22

---

## 一句话定位

| 维度 | 内容 |
|------|------|
| 是什么 | **Google 工程实践的 skill 化打包**(24 个独立 skill + 4 个 agent persona) |
| 不是 | 不是单一方法论,是工具箱 — 每个 skill 独立可用 |
| 风格 | "我是 senior engineer,告诉你要做什么"(软建议) |
| 范围 | **全 SDLC**:Define → Plan → Build → Verify → Review → Ship |
| 文化源头 | [Software Engineering at Google](https://abseil.io/resources/swe-book) + [Google engineering practices](https://google.github.io/eng-practices/) |

---

## 跟 [[Superpowers 知识库]] 的核心区别

| 维度 | **Agent Skills** | **Superpowers** |
|------|-------------------|-----------------|
| 设计哲学 | 工具箱,每个 skill 独立可选 | 操作系统,7 阶段强制流程 |
| 阶段数 | 6(`/spec /plan /build /test /review /ship`) | 7(Brainstorm→Spec→Plan→TDD→Subagent→Review→Finalize) |
| 约束级别 | **软建议**(skill 写明步骤,agent 自选) | **硬约束**(系统级指令:NO CODE WITHOUT FAILING TEST) |
| 范围 | 全 SDLC — API/前端/安全/性能/可观测/弃用 | 聚焦开发纪律 — TDD、code review、subagent |
| 风格 | "我是 senior engineer,告诉你要做什么" | "我是你的工程教练,逼你别偷懒" |
| Agent persona | 4 个显式 persona | 无,靠 skill 编排 |
| 强制方式 | slash command 触发(7 个入口) | harness 注入行为约束 |

**关系**:不冲突,可叠加。Superpowers 控流程节奏,agent-skills 提供具体工作流模板。

---

## 7 个 Slash Command 入口(Claude Code)

每个 slash command 自动激活对应阶段的 skill:

| 你在做什么 | 命令 | 核心原则 |
|-----------|------|---------|
| 定义要做什么 | `/spec` | Spec before code |
| 计划怎么建 | `/plan` | 小而原子的任务 |
| 增量构建 | `/build` | 一次一片 |
| 证明能跑 | `/test` | 测试就是证明 |
| 上线前评审 | `/review` | 提升代码健康 |
| 简化代码 | `/code-simplify` | 清晰胜过机巧 |
| 部署上线 | `/ship` | 越快越安全 |

**彩蛋**:`/build auto` 一次性生成计划并执行所有任务(仍 TDD,仍 atomic commit,失败/风险点会暂停)。

**自动激活**:skill 也能根据上下文自动触发(设计 API → `api-and-interface-design`,建 UI → `frontend-ui-engineering`)。

---

## 24 个 Skill 清单(按 6 阶段分组)

### Meta - 路由到正确 skill(1 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[using-agent-skills]] | 把传入任务映射到正确的工作流,定义共享运行规则 | 会话开始或决定用哪个 skill |

### Define - 澄清要做什么(3 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[interview-me]] | 一问一答访谈,直到 ~95% 置信度 | 需求不清或用户说 "interview me" |
| [[idea-refine]] | 发散-收敛思维,模糊想法 → 具体方案 | 概念粗糙,需要探索 |
| [[spec-driven-development]] | 写 PRD(objectives/commands/structure/code style/testing/boundaries) | 新项目、新功能、重大改动 |

### Plan - 拆解任务(1 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[planning-and-task-breakdown]] | 把 spec 拆成可验证的小任务,带验收标准、依赖排序 | 有 spec,需要可执行单元 |

### Build - 写代码(7 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[incremental-implementation]] | 垂直切片、feature flag、安全默认值、可回滚 | 改动涉及多文件 |
| [[test-driven-development]] | 红绿重构、80/15/5 测试金字塔、DAMP over DRY、Beyonce Rule | 写逻辑、fix bug、改行为 |
| [[context-engineering]] | rules files / context packing / MCP 集成 | 会话开始、切换任务、质量下降 |
| [[source-driven-development]] | 每个框架决策基于官方文档,引用来源 | 需要权威、引用源的代码 |
| [[doubt-driven-development]] | CLAIM→EXTRACT→DOUBT→RECONCILE→STOP,对抗式评审 | 高风险(生产/安全/不可逆)/陌生代码 |
| [[frontend-ui-engineering]] | 组件架构/design system/state management/响应式/WCAG 2.1 AA | 改 UI |
| [[api-and-interface-design]] | 契约优先、Hyrum's Law、One-Version Rule、error semantics | 设计 API、模块边界、公共接口 |

### Verify - 证明能跑(2 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[browser-testing-with-devtools]] | Chrome DevTools MCP,实时 DOM/console/network/performance | 浏览器相关 |
| [[debugging-and-error-recovery]] | 五步法:reproduce/localize/reduce/fix/guard | 测试失败、构建坏、行为异常 |

### Review - 上线前质量门(4 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[code-review-and-quality]] | 5 轴评审、~100 行变更、Nit/Optional/FYI 标签 | 合并前 |
| [[code-simplification]] | Chesterton's Fence、Rule of 500,保留行为降低复杂度 | 代码能用但难读 |
| [[security-and-hardening]] | OWASP Top 10、auth 模式、secrets 管理、3 层边界系统 | 处理输入/认证/存储/外部集成 |
| [[performance-optimization]] | Measure-first、Core Web Vitals、profiling | 有性能要求或怀疑回归 |

### Ship - 部署上线(5 个)

| Skill | 作用 | 何时用 |
|-------|------|--------|
| [[git-workflow-and-versioning]] | Trunk-based、atomic commits、~100 行变更 | 任何代码改动(总是) |
| [[ci-cd-and-automation]] | Shift Left、Faster is Safer、feature flag、quality gate | 改构建/部署管道 |
| [[deprecation-and-migration]] | Code-as-liability、强制 vs 建议弃用、迁移模式、僵尸代码清理 | 弃用旧系统/迁移用户 |
| [[documentation-and-adrs]] | ADR、API docs、内联文档(讲 why) | 架构决策、改 API、发布功能 |
| [[observability-and-instrumentation]] | 结构化日志、RED 指标、OpenTelemetry、症状驱动告警 | 加埋点或发布生产代码 |
| [[shipping-and-launch]] | 上线清单、feature flag 生命周期、分阶段发布、回滚 | 准备上线 |

---

## 4 个 Agent Persona

预配置的专家 agent,用于定向评审:

| Agent | 角色 | 视角 | 触发 |
|-------|------|------|------|
| [[code-reviewer-persona]] | Senior Staff Engineer | 5 轴代码评审,staff engineer 视角 | 合并前 |
| [[test-engineer-persona]] | QA Specialist | 测试策略、覆盖率分析、Prove-It pattern | 写测试 |
| [[security-auditor-persona]] | Security Engineer | 漏洞检测、威胁建模、OWASP 评估 | 改安全敏感代码 |
| [[web-performance-auditor-persona]] | Web Performance Engineer | Core Web Vitals 审计(Quick/Deep 双模式) | `/webperf` |

---

## SKILL.md 标准格式

每个 skill 都遵循统一结构:

```
┌─────────────────────────────────────────────────┐
│ SKILL.md                                        │
│                                                 │
│ ┌─ Frontmatter ─────────────────────────────┐   │
│ │ name: lowercase-hyphen-name                │   │
│ │ description: Guides agents through [task].│   │
│ │ Use when…                                 │   │
│ └───────────────────────────────────────────┘   │
│ Overview      → 这个 skill 干什么              │
│ When to Use   → 触发条件                       │
│ Process       → 一步步工作流                   │
│ Rationalizations → 借口 + 反驳(防偷懒)         │
│ Red Flags     → 哪里不对的信号                  │
│ Verification  → 证据要求                       │
└─────────────────────────────────────────────────┘
```

**关键设计**:每个 skill 自带 `Rationalizations`(借口 + 反驳)模块 — 这跟 Superpowers 的硬约束哲学呼应,只是放在 skill 内部而不是系统级。

---

## 安装方式(多 Harness 支持)

### Claude Code(推荐)
```bash
/plugin marketplace add addyosmani/agent-skills
/plugin install agent-skills@addy-agent-skills
```

> SSH 错误?仓库默认 SSH 克隆,没配 SSH key 就用完整 HTTPS URL 强制 HTTPS 克隆。

### Cursor
复制任何 `SKILL.md` 到 `.cursor/rules/`,或引用整个 `skills/` 目录。详见 `docs/cursor-setup.md`。

### Antigravity / Gemini CLI / Windsurf / OpenCode / GitHub Copilot
每个 harness 都有独立 setup 指南(`docs/` 目录下)。核心是:
- **Antigravity**:原生 plugin(skills + subagents + slash commands)
- **Gemini CLI**:原生 skill 自动发现 + `GEMINI.md` 持久化上下文
- **Windsurf**:规则配置
- **OpenCode**:`AGENTS.md` + `skill` 工具
- **GitHub Copilot**:`agents/` 作 persona + `.github/copilot-instructions.md`

### 通用
Skills 是纯 Markdown — 任何接受系统提示/指令文件的 agent 都能用。

---

## 项目结构

```
agent-skills/
├── skills/           # 24 个 skill(23 lifecycle + 1 meta)
├── agents/           # 4 个 specialist persona
├── references/       # 4 个 checklist(testing/security/perf/a11y)
├── hooks/            # 会话生命周期 hooks
├── .claude/commands/ # 7 个 slash command(Claude Code)
├── .gemini/commands/ # 7 个 slash command(Gemini CLI)
├── commands/         # 8 个 slash command(Antigravity)
├── plugin.json       # Antigravity plugin manifest
└── docs/             # 各工具 setup 指南
```

---

## 关键设计决策(从 README 提炼)

### 1. SKILL.md 的设计哲学
- **Specific**:可执行步骤,不模糊建议
- **Verifiable**:清晰退出标准 + 证据要求
- **Battle-tested**:基于真实工作流
- **Minimal**:只放必要内容,引导 agent 而非灌输

### 2. 阶段化而非一刀切
`/spec` 不是必走流程,但你**改 API** 时 `api-and-interface-design` 会自动激活。Skill 之间是**乐高**,不是**流水线**。

### 3. 反对 "AI 默认走最短路径"
> AI coding agents default to the shortest path - which often means skipping specs, tests, security reviews, and the practices that make software reliable.
>
> Agent Skills 给 agent 结构化工作流,强制执行 senior engineer 带到生产代码的纪律。

### 4. 不重复造轮子
明确标注借鉴 [Software Engineering at Google](https://abseil.io/resources/swe-book) 和 [engineering practices guide](https://google.github.io/eng-practices/):
- **Hyrum's Law** → API design
- **Beyonce Rule + 测试金字塔** → testing
- **变更粒度 + review 速度** → code review
- **Chesterton's Fence** → simplification
- **Trunk-based development** → git workflow
- **Shift Left + feature flag** → CI/CD
- **Code-as-liability** → deprecation

---

## 实战使用建议

### 适合装的场景
- ✅ 已经在用 Claude Code / Cursor / OpenCode,想要**完整工程化工作流**
- ✅ 团队或项目需要**清晰的代码评审标准**(5 轴 + Nit/Optional/FYI)
- ✅ 想给 agent 喂 **Google 工程实践**(Hyrum's Law、Trunk-based、Beyonce Rule)
- ✅ 多 harness 工作流(Cursor 写代码 + Claude Code 评审 + Copilot CLI 部署)

### 不适合装的场景
- ❌ 只想写脚本/一次性脚本(过度工程化)
- ❌ 不想被 slash command 约束(它会在每个上下文自动激活)
- ❌ 跟现有 SOP 冲突(企业可能有自己的合规流程)

### 跟现有栈的协同关系

| 现有工具 | 关系 |
|---------|------|
| [[Superpowers 知识库\|Superpowers]] | 不冲突,叠加 — Superpowers 控纪律,agent-skills 供模板 |
| [[Claude Code 知识库\|Claude Code]] | 原生支持(plugin marketplace) |
| [[OpenCode 知识库\|OpenCode]] | 支持(AGENTS.md + skill tool) |
| [[ECC 知识库\|ECC]] | 都是 Claude Code 增强层,ECC 操作层 + agent-skills 工作流层 |
| [[OpenClaw 知识库\|OpenClaw]] | OpenClaw 走自己的 skill 系统(`~/.openclaw/skills/`),不是 SKILL.md 标准 |

---

## 参考资料

- [GitHub 仓库](https://github.com/addyosmani/agent-skills)
- [Software Engineering at Google](https://abseil.io/resources/swe-book) — 文化源头
- [Google Engineering Practices](https://google.github.io/eng-practices/) — review 指南
- [Agent Skills 规范](https://agentskills.io/) — SKILL.md 开放标准
- [Anthropic Skills 仓库](https://github.com/anthropics/skills) — 同标准的官方示例
- [[Superpowers 知识库]] — 对比项目
- [[Skill 系统对比]] — 不同 skill 系统的全景对比

---

## 收藏的亮点引用

> AI coding agents default to the shortest path - which often means skipping specs, tests, security reviews, and the practices that make software reliable.

> Each skill encodes hard-won engineering judgment: when to write a spec, what to test, how to review, and when to ship.

> Skills bake in best practices from Google's engineering culture.

> These aren't generic prompts - they're the kind of opinionated, process-driven workflows that separate production-quality work from prototype-quality work.
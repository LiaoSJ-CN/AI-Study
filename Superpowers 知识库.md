---
path: Superpowers 知识库.md
workspace: ~/Documents/obsidian/ai-study/
category: ai-coding
tags: [superpowers, methodology, tdd, brainstorming]
created: 2026-06-20
updated: 2026-06-20
---

# Superpowers知识库

> Superpowers 是 Jesse Vincent 做的 agentic skills framework + 软件工程方法论,把 AI 编程 agent 从"聪明的实习生"升级为"守纪律的开发团队"。

**仓库**：[github.com/obra/superpowers](https://github.com/obra/superpowers)（约 192k ⭐）
**作者**：Jesse Vincent
**当前版本**：v5.1.0
**更新时间**：2026-06-20

---

## 核心定位

> **不是工具集,是流程规范**。

Superpowers 的核心信念:**AI 没变得更聪明,只是变得更守纪律**。它强制 agent 走完一整套工程化流程才允许 ship 代码,而不是"上来就写"。

它跟 ECC 的区别:
- ECC 是 **harness-native operator 层**(操作层,什么都有)
- Superpowers 是 **methodology framework**(方法论框架,只管流程纪律)

---

## 7 阶段工作流

| 阶段 | Skill | 作用 |
|------|-------|------|
| 1. Brainstorm | `brainstorming` | 不写代码,先聊清楚要做什么 |
| 2. Spec | `writing-specs` | 产出可执行的规格 |
| 3. Plan | `writing-plans` | 任务分解到 2-5 分钟粒度 |
| 4. TDD | `test-driven-development` | 先写测试,再写实现 |
| 5. Subagent Development | `subagent-driven-development` / `dispatching-parallel-agents` | 并行 subagent 实施 |
| 6. Review | `requesting-code-review` | 强制 code review |
| 7. Finalize | `verification-before-completion` | 完成前验证 |

**关键约束**:agent 拒绝 ship 没有测试的代码。

---

## 核心 Skills 清单

### 流程类(必装)

| Skill | 触发关键词 | 作用 |
|-------|-----------|------|
| `brainstorming` | "Let's brainstorm X" | 替代直接动手,先澄清意图 |
| `writing-specs` | "Write a spec for X" | 把模糊需求变成可验证 spec |
| `writing-plans` | "Use writing-plans to plan X" | 任务分解到 2-5 分钟颗粒度 |
| `executing-plans` | "Execute this plan" | 按 plan 顺序执行,带 checkpoint |

### 纪律类(测试 + 调试)

| Skill | 触发关键词 | 作用 |
|-------|-----------|------|
| `test-driven-development` | TDD red-green-refactor | 测试驱动开发的循环指南 |
| `systematic-debugging` | "Systematically debug X" | 4 阶段根因分析(含 bash 二分脚本) |
| `verification-before-completion` | "Verify before completion" | 完成前必须验证,不靠"应该跑得通" |

### 协作类(并行 + 隔离)

| Skill | 触发关键词 | 作用 |
|-------|-----------|------|
| `dispatching-parallel-agents` | 并行 subagent 派发 | 多 subagent 并发处理独立任务 |
| `subagent-driven-development` | subagent 协作模式 | 用 subagent 做大规模实现 |
| `requesting-code-review` | code review 流程 | 强制产出 reviewer + 验收 |
| `using-git-worktrees` | "Use git worktrees" | 用 git worktree 隔离并行特性开发 |
| `writing-skills` | "Create a skill for X" | 元 skill:写新 skill |

### SDD (Spec-Driven Development)

`/spec-driven-development` 系列:spec → plan → execute 的端到端工作流,带 pause-every-3-tasks 检查点。

---

## 安装方式

### Claude Code 官方 marketplace(推荐)

```bash
/plugin install superpowers@claude-plugins-official
```

### Superpowers 自家 marketplace

```bash
/plugin marketplace add obra/superpowers-marketplace
/plugin install superpowers@superpowers-marketplace
```

### Kimi Code

打开 `/plugins` → Marketplace → Superpowers → Install

### 源码安装

```bash
git clone https://github.com/obra/superpowers.git ~/.claude/plugins/superpowers
```

### 多 harness 兼容

| Harness | 安装命令 |
|---------|---------|
| Claude Code | `/plugin install superpowers@claude-plugins-official` |
| Antigravity | `agy plugin install <obra/superpowers>` |
| Codex App | 内置 marketplace |
| Codex CLI | 通过 marketplace |
| Cursor | 通过 marketplace |
| Factory Droid | 见官方文档(2026-02 文档新增) |
| Gemini CLI | 通过 marketplace |
| GitHub Copilot CLI | 通过 marketplace |
| Kimi Code | `/plugins` → Superpowers |
| OpenCode | 通过 marketplace(注意 OpenCode bootstrap 缓存) |
| Pi | 通过 marketplace |

> ⚠️ 注意 OpenCode 的 bootstrap 缓存机制(`#1202` 已修复),安装后第一次启动会慢几秒。

---

## 实战示例(Notion-like Web App)

来源:Alex Followell 2026-02 YouTube 演示

```text
# 1. 模糊需求
"我想做个像 Notion 一样的 Web App"

# 2. Superpowers 拦截,触发 brainstorming
→ 问你:目标用户?核心功能?是否要协作?

# 3. writing-specs
→ 产出 PRD + 设计文档

# 4. writing-plans
→ 任务分解,每段 2-5 分钟

# 5. subagent-driven-development
→ 并行多 subagent 执行

# 6. 过程中强制 TDD
→ 每个模块先写测试

# 7. requesting-code-review
→ 派 reviewer subagent 审查

# 8. verification-before-completion
→ 全部跑通才 ship
```

**对比纯 Claude Code**:后者直接写代码,通常需要用户事后发现架构问题。Superpowers 提前 20 分钟,省后续 2 小时返工。

---

## v5.1.0 重要变更(2026-06)

- ❌ **移除** 老的 slash commands:`/brainstorm`、`/execute-plan`、`/write-plan`(改用 skill 触发)
- ❌ **移除** skill 中的 Integration sections(由 Codex 自动处理)
- ✏️ Worktree skills 重写
- ✏️ OpenCode bootstrap caching 优化
- ✏️ Cursor Windows hook 路由修复
- ✏️ Gemini CLI subagent dispatch 映射
- ✏️ Factory Droid 安装文档新增
- ✅ v5.1.0 集成测试:7/7 subagent 全部按 `general-purpose` 派发,spec 验证能拒绝额外特性

---

## 关键设计理念

1. **Discipline over speed** — 宁可慢两分钟,不要 ship 烂 diff。
2. **Subagent honesty** — `superpowers:code-reviewer` 必须独立运行,不能跟 writer 同进程(防止自我批准)。
3. **Tests as gate** — 没有测试 = 不允许 ship,reviewer 必须验证测试存在且失败过。
4. **Skill = markdown** — 每个 skill 都是纯 markdown + 配套脚本,无网络/无凭证/无 telemetry(部分)。
5. **Harness-agnostic** — 同一套 skill 在 Claude Code、Cursor、Codex 等都跑。

---

## 工具评估(2026-06-20)

✅ **优势**
- 方法论非常扎实,适合工程师"宁愿慢不要错"的心态
- 192k ⭐ 社区活跃,2026-02 进 Anthropic 官方 marketplace
- 跨 harness 兼容性第一梯队
- Subagent-driven 流程天然适合大型特性
- **完全 self-contained**,无凭证/无外部调用(安全审计友好)
- 测试驱动、TDD 红色/绿色/重构循环强制执行

⚠️ **注意**
- 工作流**故意放慢**,小改动/脚本可能觉得繁琐
- 必须接受"先 brainstorm 再写代码"的节奏,急性子会烦
- v5.1.0 移除了老的 `/brainstorm`、`/write-plan` 等命令,统一改用 skill 触发

🎯 **适用场景**
- 中型以上项目,需要严格工程纪律
- 团队多人协作,需要统一方法论
- 长期维护的代码库,愿意花前期 20 分钟省后期 2 小时
- Subagent-driven 大特性开发(Superpowers 主场)

📚 **相关**
- [[ECC 知识库]] — ECC 是更广的操作层,Superpowers 更聚焦方法论
- [[CodeGraph 知识库]] — Subagent 探索阶段配合 CodeGraph 可大幅省 token

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 首次结构化（frontmatter + Obsidian 双向链接） |

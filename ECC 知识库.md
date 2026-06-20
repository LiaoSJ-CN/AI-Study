---
path: ECC 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: ai-coding
tags: [ecc, everything-claude-code, agents, marketplace]
created: 2026-06-20
updated: 2026-06-20
---

# Everything Claude Code知识库

> ECC 是一个 harness-native 的 agentic 工作操作系统,把 Claude Code / Codex / Cursor 等 AI 编程工具从"单兵程序员"升级为"专家团队"。

**仓库**：[github.com/affaan-m/everything-claude-code](https://github.com/affaan-m/everything-claude-code)（约 182k ⭐，曾用名 `affaan-m/ECC`）
**作者**：Affaan Mustafa（2025-09 Anthropic Hackathon 冠军）
**官网**：[ecc.tools](https://ecc.tools)
**当前版本**：v2.0.0（新增 Hermes operator 层）
**更新时间**：2026-06-20

---

## 核心定位

> **不是单个 skill，而是一整套跨 harness 的操作层**。

ECC 把 agents / skills / commands / hooks / rules / MCP servers / instincts / memory / security 全部打包,自动注入到支持的 AI 编程工具里。在 Claude Code、Codex、Cursor、OpenCode、Gemini、Zed、Copilot 等 harness 之间切换,ECC 配置跟着走。

---

## 包含内容（按数量）

| 组件 | 数量 | 说明 |
|------|------|------|
| Agents | 67 | 角色化 subagent(planner、code-reviewer、test-writer…) |
| Skills | 271 | 可触发的能力模块 |
| Commands | 92 | 老的 slash command shim,保持兼容 |
| Hooks | 29 | Claude Code 生命周期 hook |
| Rules | 29 | 多语言规则(Python/Go/TS/JS…) |
| MCP Servers | 14 | 内置 MCP 服务注册 |
| Security rules | 102 | AgentShield 扫描每次 session |

> OpenCode 上有部分精简:12 agents + 37 skills + 35 commands + 13 instructions,但 hooks 多 11 个、custom tools 6 个 native。

---

## 安装方式

### 推荐:Claude Code 官方 marketplace

```bash
/plugin marketplace add affaan-m/everything-claude-code
/plugin install ecc@ecc
```

### 备选:npm 全局包

```bash
npm install -g ecc-universal
```

### 手动安装(适合生产环境)

```bash
git clone https://github.com/affaan-m/everything-claude-code.git
cd everything-claude-code
./install.sh --profile full
```

`--profile` 选项可按需裁剪:`core` / `developer` / `security` / `full`。

### 低噪音路径(只想要 rules/agents/commands,不要 hooks)

适合觉得 hook 太"全局干扰"的场景,跳过 `hooks-runtime`,只装 rules/agents/commands/skill。

---

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `/plan` | 进入规划模式(让 subagent 先拆任务) |
| `/code-review` | 触发 code-reviewer agent |
| `/build-fix` | 构建失败自动修复 |
| `/refactor-clean` | 重构并清理死代码 |
| `/learn` | 从当前 session 提炼可复用经验 |
| `/checkpoint` | 保存进度快照 |
| `/quality-gate` | 跑全套质量检查 |
| `/update-docs` | 同步文档到代码现状 |
| `/update-codemaps` | 重建代码地图 |
| `/test-coverage` | 检查/补齐测试覆盖 |
| `/skill-create` | 元命令:创建新 skill |
| `/evolve` | 推动 instinct/rule 演进 |
| `/promote` | 把短期记忆提升为长期 |
| `/projects` | 管理多项目配置 |
| `/prune` | 清理过期/冲突的配置 |
| `/instinct-status` / `/instinct-import` / `/instinct-export` | 管理 instincts |

**多模型命令**(需额外装 ccg-workflow runtime):`/multi-plan` `/multi-execute` `/multi-backend` `/multi-frontend` `/multi-workflow`

**语言专项**:`/go-review` `/go-test` `/go-build` `/python-review`

---

## 多 harness 兼容性

| Harness | Agents | Skills | Commands | Hooks | Rules | MCP | 备注 |
|---------|--------|--------|----------|-------|-------|-----|------|
| Claude Code | 67 | 271 | 92 | 8 types | 29 | 14 | 全功能 |
| OpenCode | 12 | 37 | 35 | 11 events | 13 | Full | hooks 更多、native tools 更强 |
| Cursor | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | 需要在 `~/.claude.json` 设独立 agent data root |
| Codex CLI | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | |
| Gemini CLI | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | |
| Kimi Code | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | marketplace 内置 |
| Antigravity IDE | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | |
| Kiro | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | |
| GitHub Copilot CLI | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | |

### Agent 数据隔离(关键坑)

ECC 默认把所有 agent 数据存到 `~/.claude`。**同时用 Claude Code + Cursor 同一台机器时,必须给 Cursor 单独配 root,否则会互相覆盖**。

```bash
# ~/.claude/settings.json (Claude Code)
# ~/.cursor/agent-data   (Cursor,单独 root)
```

子路径:`$ECC_AGENT_DATA_HOME/session-data/`、`skills/learned/`、`session-aliases.json`、`metrics/`

---

## 关键设计理念

1. **Harness-native operator**:不是替换 Claude Code,而是"加层",让所有 harness 都遵循一套工作流。
2. **Instincts**:ECC 学到的东西会沉淀成"本能"(类似 OpenClaw 的 self-improvement),跨 session 复用。
3. **Memory optimization**:自动压缩 session 历史,减少 token 浪费。
4. **Continuous learning**:`/learn` 命令把 session 经验提炼成 skill / instinct。
5. **Security scanning**:AgentShield 每次 session 自动跑 102 条安全规则。
6. **Research-first**:写代码前先做研究,避免盲目实现。

---

## 卸载

ECC 只删自己安装过的文件,不会误伤其他内容:

```bash
# 反向 installer,自动清掉 hooks/settings/commands
ecc uninstall
```

如果叠装了多种方式,按 marketplace → npm → git clone 的逆序清理。

---

## 我的评估(2026-06-20)

✅ **优势**
- 覆盖面最广,日常 driver 配置首选
- 多 harness 兼容,切换工具不用重配
- 方法论完整(plan/quality/learn/promote)
- AgentShield 安全检查开箱即用

⚠️ **注意**
- 67 agents + 271 skills 体量大,小项目用可能"杀鸡用牛刀"
- Hooks 可能跟自定义 hook 冲突,先关掉 ECC hooks 再加自己的
- OpenCode 上阉割较多,如果主力是 OpenCode,可以考虑只装核心包

🎯 **适用场景**
- 日常主力 Claude Code / Cursor,想要完整工程化工作流
- 团队多人协作,需要统一方法论
- 已经在用 OpenClaw 想升级到更激进的方法论

📚 **相关**
- [[Superpowers 知识库]] — Jesse Vincent 的方法论框架,与 ECC 部分重叠但更轻量
- [[CodeGraph 知识库]] — 代码库知识图谱,可以跟 ECC 的 codemaps 互补

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 首次结构化（frontmatter + Obsidian 双向链接） |

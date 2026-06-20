---
path: Claude Code 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: ai-coding
tags: [claude-code, anthropic, terminal-agent, subagent, hooks]
created: 2026-06-20
updated: 2026-06-20
---

# Claude Code 学习知识库

> Claude Code 是 Anthropic 推出的 AI 编程助手,在终端里跑,直接读写代码、执行命令、管理项目。杰哥主力编程工具。

**当前版本**:Claude Code **2.1.81**
**安装路径**:`~/.npm-global/bin/claude`
**认证状态**:✅ 已认证
**更新日期**:2026-06-20(本文件大幅重写)

---

## 一、它是什么

Claude Code = Anthropic 官方 CLI 编程 agent。核心定位:

- **不是 IDE 插件**(虽然可以配合 VSCode/Cursor)
- **不是聊天框**(虽然也能聊)
- **是终端里的 AI 工程师** — 直接操作文件系统、执行 bash 命令、调用工具

跟 Cursor/Copilot 的本质区别:**Claude Code 是 agent**(能自主多步执行),它们是补全+聊天(被动响应)。

---

## 二、2026 重大更新(从 v1 到 v2.1.81)

2026 是 Claude Code 爆发年,**176 次 changelog 更新**,核心新能力:

| 能力 | 是什么 | 价值 |
|------|--------|------|
| **Routines** | 持久化执行流程(类似 saved prompt) | 一次定义,反复跑 |
| **`/ultrareview`** | 超深度 code review | 比 `/code-review` 更彻底 |
| **Auto Mode** | 自动模式(无询问直接执行) | 适合信任度高的任务 |
| **Ultrathink** | 深度思考模式(更多推理 token) | 复杂问题更准 |
| **Subagents** | spawn 子 agent(可 5 层嵌套) | 并行处理、独立上下文 |
| **Hooks** | 生命周期事件钩子 | 自动化 session 流程 |
| **Plugins** | 插件系统(2026 末推出) | 跨 harness 共享 |
| **`/context`** | 查看/管理当前 context | 解决"context 爆炸" |
| **Agent Teams** | 多 agent 协作(TeamCreate/TeamDelete 已被移除,改用 subagent) | 并行多 agent 工作 |

> ⚠️ **破坏性变化**:TeamCreate / TeamDelete 工具已移除,改用 subagent 模式。

---

## 三、安装与配置

### 1. 安装

```bash
# 官方推荐(自动检测系统)
curl -fsSL https://claude.ai/install.sh | bash

# 或 npm
npm install -g @anthropic-ai/claude-code
```

### 2. 环境变量常见问题

如果遇到 SSL/网络问题,在 `~/.zshrc` 加:

```bash
export NODE_TLS_REJECT_UNAUTHORIZED=0
export NODE_OPTIONS="--unhandled-rejections=strict"
```

然后 `source ~/.zshrc` 生效。

> ⚠️ 关代理或切回官方 API 后,记得**删掉这两行**(或加 `#` 注释),恢复正常安全验证。

### 3. GitHub Token

```bash
ghp_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

存在环境变量或 `~/.netrc` / `~/.git-credentials`。

### 4. 配置文件层级

| 级别 | 路径 | 用途 |
|------|------|------|
| 全局 | `~/.claude/CLAUDE.md` | 跨项目个人偏好 |
| 项目 | `<project>/CLAUDE.md` | 项目级指令(优先级高) |
| 局部 | `CLAUDE.local.md` | 机器特定(不进 git) |
| 技能 | `~/.claude/skills/` | 本地 skills |
| 插件 | `~/.claude/plugins/` | 已装插件 |
| Hooks | `~/.claude/settings.json` 的 hooks 字段 | 生命周期钩子 |

### 5. CLAUDE.md 实战

```markdown
# ~/.claude/CLAUDE.md
# 全局偏好
- 回答用中文,代码注释用英文
- 优先用 Python(ai-dev 环境)
- 不要在 PR 中包含我手写的 token
- 看到 ⭐ Insight 段就当元提示用

# <project>/CLAUDE.md
# 项目级
- 这是 PostgreSQL + Doris 混合架构项目
- 所有 SQL 必须 EXPLAIN 通过才能合并
- 测试覆盖率不能掉
```

**优先级**:局部 > 项目 > 全局。

---

## 四、核心命令速查

### 4.1 会话管理

| 命令 | 用途 |
|------|------|
| `claude` | 启动 TUI(默认) |
| `claude -c` | 恢复上次会话 |
| `claude -r` | 选择历史会话 |
| `claude --model <id>` | 指定模型启动 |
| `claude --agent <id>` | 指定 subagent |
| `claude --continue` | 续接 |

### 4.2 文件与项目

| 命令 | 用途 |
|------|------|
| `/init` | 初始化项目(分析代码生成 CLAUDE.md) |
| `/memory` | 编辑记忆文件 |
| `/context` | 查看 context 用量 |
| `/compact` | 压缩 context |
| `/clear` | 清空 context |
| `/add-dir <path>` | 添加工作目录 |

### 4.3 代码与工作流

| 命令 | 用途 |
|------|------|
| `/plan` | 进入计划模式(不写代码,只规划) |
| `/code-review` | 跑 code review |
| `/ultrareview` | **v2 新** 超深度 review |
| `/refactor` | 重构建议 |
| `/test` | 生成/跑测试 |
| `/build-fix` | 构建失败自动修复 |
| `/pr` | 创建 PR |

### 4.4 Skills / Plugins

| 命令 | 用途 |
|------|------|
| `/plugin install <name>` | 安装插件(从 marketplace) |
| `/plugin list` | 列出已装插件 |
| `/plugin marketplace add <repo>` | 添加 marketplace |
| `/skill list` | 列出 skills |
| `/skill show <name>` | 看 skill 详情 |
| `/skill create` | 元命令:创建新 skill |

### 4.5 Subagent

| 命令 | 用途 |
|------|------|
| `@explore` | 调用内置 Explore agent(代码库探索) |
| `@general` | 调用 General agent(复杂任务) |
| `@scout` | 调用 Scout agent(外部研究) |
| `Task` tool | 手动 spawn subagent |

### 4.6 Routines(2026 新)

```bash
# 创建一个 routine
/routine create code-review-precommit \
  --trigger "before commit" \
  --steps "/code-review, /test"

# 触发
/routine code-review-precommit
```

Routines 是**持久化的多步执行流**,类似 saved prompt,但能跨 session 复用。

---

## 五、Subagent 系统(2026 关键能力)

### 5.1 内置 Subagent

| Agent | 权限 | 用途 |
|-------|------|------|
| **Explore** | 只读 | 快速探索代码库(grep/glob/Read) |
| **General** | 全部 | 复杂研究+多步骤任务 |
| **Scout** | 只读 | 外部文档和依赖研究 |
| **Statusline** | 只读 | 获取 status 信息 |

### 5.2 自定义 Subagent

`~/.claude/agents/<name>.md`:

```markdown
---
name: postgres-expert
description: PostgreSQL 性能调优专家
tools: Read, Grep, Bash(psql 限定)
model: sonnet
---

你是 PostgreSQL 性能调优专家。
- 只用 EXPLAIN ANALYZE 分析慢查询
- 推荐索引时给完整 DDL
- 任何 DDL 改动先确认生产环境风险
```

调用:`@postgres-expert` 即可。

### 5.3 5 层嵌套(2026 末新能力)

Subagent 现在可以 spawn 自己的 subagent(最多 5 层深)。例:

```
Main
  └─ General(规划整体方案)
       └─ Explore(找相关代码)
       └─ Scout(查外部文档)
            └─ Explore(查依赖)
```

⚠️ **Token 消耗警告**:5 层嵌套能烧海量 token,日常用 2-3 层就够。

### 5.4 Subagent-driven Development(SDD)

**来自 obra/superpowers 哲学**:
1. Main agent 拿到任务
2. 拆解为多个独立子任务
3. 并行 spawn subagent 处理
4. Main 聚合结果
5. 派 code-reviewer 验证

> **效率提升**:相比单 agent 串行,SDD 模式大型特性提速 3-5x。

---

## 六、Hooks(2026 关键能力)

Hooks 让 Claude Code 在生命周期事件触发自动化操作。`~/.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo 'Bash about to run' >> ~/.claude/hooks.log"
          }
        ]
      }
    ],
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write $CLAUDE_TOOL_INPUT_FILE"
          }
        ]
      }
    ],
    "SessionStart": [
      {
        "hooks": [
          { "type": "command", "command": "echo 'Session started at $(date)' >> ~/.claude/sessions.log" }
        ]
      }
    ],
    "SessionEnd": [
      {
        "hooks": [
          { "type": "command", "command": "openclaw memory promote --auto" }
        ]
      }
    ]
  }
}
```

### 6.1 事件类型

| 事件 | 触发时机 |
|------|----------|
| `SessionStart` / `SessionEnd` | session 开始/结束 |
| `PreToolUse` / `PostToolUse` | 工具调用前后 |
| `UserPromptSubmit` | 用户提交 prompt 时 |
| `Stop` / `SubagentStop` | agent 停止时 |
| `Notification` | 通知触发 |

### 6.2 实用 Hook 模式

| 模式 | 用途 |
|------|------|
| 自动格式化 | PostToolUse(Edit) 跑 prettier/black |
| 安全审计 | PreToolUse(Bash) 检测危险命令 |
| 记忆保存 | SessionEnd 触发 memory promote |
| 日志记录 | 全事件写入结构化日志 |
| 通知推送 | Notification 推到飞书/Slack |

---

## 七、Settings.json 完整结构

```json
{
  "model": "sonnet",
  "agent": "general",
  "env": {
    "ANTHROPIC_API_KEY": "...",
    "DISABLE_TELEMETRY": "1"
  },
  "permissions": {
    "allow": [
      "Read",
      "Write(src/**)",
      "Bash(ls:*)",
      "Bash(git:*)"
    ],
    "deny": [
      "Bash(rm -rf:*)",
      "Bash(sudo:*)"
    ]
  },
  "mcpServers": {
    "codegraph": { "command": "codegraph", "args": ["mcp"] },
    "fetch": { "command": "npx", "args": ["-y", "@anthropic-ai/mcp-fetch"] }
  },
  "hooks": { /* 见上节 */ },
  "plugins": ["ecc@ecc", "superpowers@claude-plugins-official"]
}
```

---

## 八、MCP(Model Context Protocol)

Claude Code 是 MCP 的"标杆 harness"。通过 MCP 接入任何外部工具:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "postgresql://..." }
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_TOKEN": "..." }
    },
    "codegraph": { "command": "codegraph", "args": ["mcp"] }
  }
}
```

启动时自动加载,agent 可调用 `mcp__postgres__query` 等。

### 常用 MCP Servers

| Server | 能力 |
|--------|------|
| `@anthropic-ai/mcp-fetch` | 网页抓取 |
| `@modelcontextprotocol/server-postgres` | PG 查询 |
| `@modelcontextprotocol/server-github` | GitHub 操作 |
| `@modelcontextprotocol/server-filesystem` | 文件系统 |
| `@upstash/mcp-server` | Upstash KV/Redis |
| `codegraph` | 本地代码图谱(见 [[CodeGraph 知识库]]) |

---

## 九、Plugins(2026 末新)

跟 Skills 的区别:
- **Skill** = 提示词级别的能力(纯 markdown)
- **Plugin** = 包含 commands + agents + hooks + MCP servers + UI 的完整包

```bash
# 安装 plugin
/plugin marketplace add affaan-m/everything-claude-code
/plugin install ecc@ecc

# 列出
/plugin list
```

### 推荐 Plugins

| Plugin | 作用 |
|--------|------|
| `ecc@ecc` | Everything Claude Code(67 agents + 271 skills,见 [[ECC 知识库]]) |
| `superpowers@claude-plugins-official` | 7 阶段工作流方法论(见 [[Superpowers 知识库]]) |
| `codegraph` | 本地代码图谱 |

---

## 十、Skills(技能)

### 10.1 是什么

Skill = 一个目录,里面是 `SKILL.md`(纯 markdown 指令)+ 可选脚本。Claude Code 启动时自动加载,根据用户问题触发。

### 10.2 自带 Skills(Anthropic 官方)

| Skill | 用途 |
|-------|------|
| `docx` | Word 文档 |
| `pdf` | PDF 处理 |
| `pptx` | PPT 生成 |
| `xlsx` | Excel |
| `frontend-design` | 前端 UI 设计 |
| `web-artifacts-builder` | 单页 web app |
| `mcp-builder` | 创建 MCP server |
| `skill-creator` | 元 skill:创建新 skill |

### 10.3 自定义 Skill

```bash
# 用官方 skill-creator 元 skill
/skill create

# 手动创建
mkdir -p ~/.claude/skills/my-skill
cat > ~/.claude/skills/my-skill/SKILL.md << 'EOF'
---
name: my-skill
description: 当用户需要 X 时触发。关键词:"做X", "生成X"
---

# 做什么
[指令]
EOF
```

> 详细的 skill 设计哲学见 [[Skill 系统对比]](待建)

---

## 十一、工作流模式(2026 SOTA)

### 11.1 探索模式(Plan)

```
User: "我想重构 auth 模块"
→ Claude 进入 Plan 模式
→ spawn Explore agent 找相关代码
→ 输出详细计划(不写代码)
→ 用户确认
→ 切到 Build 模式执行
```

### 11.2 TDD 模式

```
User: "用 TDD 实现 X"
→ spawn 测试专家写失败测试
→ 跑测试(失败)
→ 切到 build,实现直到通过
→ refactor
```

### 11.3 Ultrareview 模式(2026 新)

```
User: "/ultrareview"
→ 启动多个 reviewer subagent 并行
→ 安全、性能、可读性、可维护性 4 个维度
→ 合并报告
```

### 11.4 Auto Mode 模式(2026 新)

```
User: "用 auto mode 把这个 PR 修到 CI 通过"
→ Claude 直接执行,不再询问
→ 高风险命令仍需确认
```

### 11.5 Routines 模式(2026 新)

```bash
/routine create my-pr-checklist --steps "/ultrareview, /test, /lint"
# 之后一个命令跑完
```

---

## 十二、性能优化技巧

### 12.1 Context 管理

- **`/context`** 查用量
- **`/compact`** 手动压缩
- **`/clear`** 切话题时清空
- **子任务用 subagent**:每个 subagent 独立 context,主 agent 保持干净

### 12.2 模型选择

| 任务 | 推荐模型 |
|------|---------|
| 日常开发 | Sonnet(性价比) |
| 复杂架构/规划 | Opus(深度) |
| 简单补全/文档 | Haiku(速度) |

### 12.3 工具策略

```json
{
  "permissions": {
    "allow": [
      "Read",
      "Bash(git:*)",
      "Bash(npm test:*)",
      "Edit(src/**)"
    ]
  }
}
```

**最小授权原则** — 只允许必要工具,降低误操作风险。

---

## 十三、常见问题

### Q1: Claude Code 启动很慢
- 检查 `~/.claude/plugins/` 是否有不用的 plugin
- 用 `claude --no-plugins` 临时跳过
- 看 `~/.claude/hooks.log` 是否有慢 hook

### Q2: Token 消耗过大
- 启用 `/compact` 周期压缩
- 大任务用 subagent 分担
- 用 `/clear` 切话题
- 看是否启用了 Routines(可能无意义消耗)

### Q3: 工具调用被拒绝
- 查 `settings.json` 的 `permissions.deny`
- 临时用 `--dangerously-skip-permissions`(慎用)
- 调整 hook 规则

### Q4: 如何离线工作
- 本地模型走 [[OpenCode 知识库]](同协议)
- 缓存的 context 可以续接
- MCP server 本地跑(如 CodeGraph)

### Q5: Subagent 嵌套层数
- 最多 5 层(2026 末新)
- 日常用 2-3 层,token 烧得起
- 深层嵌套要严格规划,否则爆炸

---

## 十四、与其他工具的协同

杰哥当前技术栈:

```
┌─────────────────────────────────┐
│  OpenClaw(Gateway + Skills)     │  ← 顶层调度
├─────────────────────────────────┤
│  Claude Code / OpenCode         │  ← 编程 agent
│  + ECC / Superpowers / CodeGraph│  ← 增强插件
├─────────────────────────────────┤
│  Hermes Agent(自我学习)         │  ← 长期记忆/沉淀
└─────────────────────────────────┘
```

详细关系见 [[AI 编程工具全景图]]。

---

## 十五、参考资料

- **官方文档**:[https://code.claude.com/docs](https://code.claude.com/docs)
- **Changelog**:[https://code.claude.com/docs/en/changelog](https://code.claude.com/docs/en/changelog)
- **MCP Spec**:[https://modelcontextprotocol.io](https://modelcontextprotocol.io)
- **ClawHub 插件市场**:通过 `claude plugin marketplace` 命令

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 大幅重写:从 29 行 stub 扩到 400+ 行,加 2026 新特性(Routines/Auto Mode/Ultrathink/Ultrareview/Subagent-5 层/Plugins/Hooks 全套) |
| 更早 | 环境变量 + git token + CLAUDE.md 文件清单 |

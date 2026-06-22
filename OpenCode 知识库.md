---
path: OpenCode 知识库.md
workspace: ~/Documents/obsidian/ai-study/
category: ai-coding
tags: [opencode, multi-provider, terminal-agent]
created: 2026-06-20
updated: 2026-06-20
---

# OpenCode知识库

> OpenCode 是一个开源的 AI 编程助手，可在终端、桌面或 IDE 中使用。支持任意 LLM Provider，配置灵活。

**官网**：[https://opencode.ai](https://opencode.ai)  
**GitHub**：[https://github.com/anomalyco/opencode](https://github.com/anomalyco/opencode)（160K ⭐）

---

## 安装方式

### 方式一：安装脚本（推荐）

```bash
curl -fsSL https://opencode.ai/install | bash
```

### 方式二：npm

```bash
npm install -g opencode-ai
```

### 方式三：Homebrew（macOS/Linux）

```bash
brew install anomalyco/tap/opencode
```

> ⚠️ 官方 Homebrew formula 更新较慢，建议用安装脚本或 npm。

### 方式四：其他平台

| 平台 | 命令 |
|------|------|
| Arch Linux | `sudo pacman -S opencode` |
| Windows | `choco install opencode` 或 `scoop install opencode` |
| Docker | `docker run -it --rm ghcr.io/anomalyco/opencode` |
| Mise | `mise use -g github:anomalyco/opencode` |

### 桌面 App

从 [opencode.ai/download](https://opencode.ai/download) 下载 macOS/Windows/Linux 的 DMG/EXE/RPM 包。

---

## 快速入门

### 1. 配置 API Key

```bash
opencode auth login
```

按提示选择 Provider，输入 API Key。

**推荐**：[OpenCode Zen](https://opencode.ai/zen) — 团队精选的编程友好模型列表。

### 2. 进入项目

```bash
cd /path/to/project
opencode
```

### 3. 初始化项目

```bash
/init
```

OpenCode 会分析项目结构，生成 `AGENTS.md` 文件，帮助理解代码风格和项目架构。

### 4. 开始使用

```
今天 AI 圈有什么新东西？
帮我解释一下这个函数
添加一个新功能
```

---

## 配置文件

### 配置位置（优先级从低到高）

1. **远程组织配置**（`.well-known/opencode`）
2. **全局配置**（`~/.config/opencode/opencode.jsonc`）
3. **自定义路径**（`OPENCODE_CONFIG` 环境变量）
4. **项目配置**（项目根目录的 `opencode.json`）
5. **MDM 强制配置**（最高优先级，管理员控制）

### 配置示例

```json
{
  "$schema": "https://opencode.ai/config.json",

  "model": "minimax/MiniMax-M2.7",

  "provider": {
    "minimax": {
      "options": {
        "apiKey": "YOUR_API_KEY",
        "baseURL": "https://api.minimaxi.com/anthropic/v1"
      },
      "models": {
        "MiniMax-M2.7": { "name": "MiniMax M2.7" },
        "MiniMax-M2.5-Lightning": { "name": "MiniMax M2.5 Lightning" }
      }
    }
  },

  "agent": {
    "build": {
      "mode": "primary",
      "model": "minimax/MiniMax-M2.7",
      "permission": { "edit": "allow", "bash": "allow" }
    },
    "plan": {
      "mode": "primary",
      "model": "minimax/MiniMax-M2.5-Lightning",
      "permission": { "edit": "deny", "bash": "deny" }
    }
  }
}
```

### 配置格式

- 支持 JSON 和 JSONC（带注释的 JSON）
- 支持 `$schema` 自动补全

---

## Agent 系统

OpenCode 有两种 Agent 类型：

### Primary Agents（主 Agent）

用 **Tab 键**切换，`Tab` 循环切换。

| Agent | 权限 | 用途 |
|-------|------|------|
| **Build** | 全部允许 | 默认开发模式，文件编辑 + 命令执行 |
| **Plan** | 全部询问 | 规划模式，只分析不修改，适合设计讨论 |

### Subagents（子 Agent）

用 `@名字` 手动调用，或让主 Agent 自动调用。

| Agent | 权限 | 用途 |
|-------|------|------|
| **General** | 全部允许 | 复杂研究 + 多步骤任务 |
| **Explore** | 只读 | 快速探索代码库、搜索关键词 |
| **Scout** | 只读 | 外部文档和依赖研究 |

### 自定义 Agent

```bash
opencode agent create \
  --path ~/.config/opencode/agents/coder.json \
  --description "代码审查专家" \
  --mode subagent \
  --permissions read,edit,glob,grep \
  --model minimax/MiniMax-M2.7
```

---

## CLI 命令

### 常用命令

```bash
# 进入 TUI 界面
opencode

# 非交互模式运行
opencode run "say hello"
opencode run "解释这个函数"

# 继续上次会话
opencode --continue

# 指定模型
opencode run "hello" --model minimax/MiniMax-M2.7

# 指定 Agent
opencode run "hello" --agent build
```

### Agent 管理

```bash
# 列出所有 Agent
opencode agent list

# 登录 Provider
opencode auth login

# 查看已登录的 Provider
opencode auth list
```

### Web 服务

```bash
# 启动 Web 服务（远程访问）
opencode web --port 4096 --hostname 0.0.0.0

# 附加 TUI 到远程服务
opencode attach http://10.20.30.40:4096
```

---

## OpenCode vs OpenClaw

| 维度 | OpenCode | OpenClaw |
|------|----------|----------|
| **定位** | 终端编程助手 | 个人 AI 工作站 |
| **核心能力** | 代码编辑、bash、文件操作 | 聊天、Skills、Channel 集成 |
| **多 Agent** | ✅ 内置多 Agent 配置 | ✅ 支持 spawn/子会话 |
| **Skills 系统** | ❌ | ✅ 完整 Skills 生态 |
| **飞书/Slack 集成** | ❌ | ✅ |
| **记忆系统** | ❌ | ✅ MEMORY.md + 每日记录 |
| **使用场景** | 编程、代码审查、调试 | 工作流自动化、生活助手 |

**总结**：OpenCode 专注写代码，OpenClaw 专注管你的日常。两个可以同时用。

---

## 常见问题

### Q: baseURL 怎么填？

格式必须带 `/v1`，例如：

```json
"baseURL": "https://api.minimaxi.com/anthropic/v1"
```

不带 `/v1` 会返回 404。

### Q: 安装后命令找不到？

如果 `opencode` 命令找不到，手动软链到 PATH 目录：

```bash
ln -sf ~/.npm-global/bin/opencode ~/.local/bin/opencode
```

### Q: 如何自定义主题和快捷键？

```bash
# 查看主题列表
opencode theme

# 配置文件
~/.config/opencode/tui.json
```

---

## 参考链接

- 官方文档：https://opencode.ai/docs
- GitHub：https://github.com/anomalyco/opencode
- OpenCode Zen：https://opencode.ai/zen
- Models.dev（Provider 列表）：https://models.dev

---

_最后更新：2026-05-24_
---

## OpenCode 2026 更新（05-24 之后）

> 本节补充 2026-05-24 之后的 OpenCode 更新。

### 关键数据更新

| 指标 | 2026-05 | 2026-06(当前) |
|------|---------|---------------|
| GitHub Stars | 160K | **240K+** |
| Provider 数量 | 50+ | **75+**(含 OpenCode Zen) |
| 活跃贡献者 | 150+ | 200+ |
| 桌面平台 | macOS/Win/Linux | 同上 + **Tauri 重构版**(更轻) |

> ⭐ 增速飞快(1 个月 +80K),OpenCode 已成 Claude Code 头号替代品。

### 2026 新增能力

#### 1. OpenCode Zen(2026 旗舰)

团队精选的"编程友好"模型列表,内置在 OpenCode 里:

```bash
# 一行接入
opencode auth login --provider opencode-zen
# 自动获取所有 Zen 模型 access

# 看 Zen 模型列表
opencode models --provider opencode-zen
```

**优势**:
- 跳过各家 API key 配置
- 模型经过编程场景 benchmark 筛选
- 价格比裸 API 略低(团队协议)
- 包含:Claude Sonnet/Opus、GPT-4o、Gemini 2.5、DeepSeek-V3、Qwen3-Coder、Mistral Coder

#### 2. Subagent 增强

```bash
# 自定义 subagent
opencode agent create \
  --path ~/.config/opencode/agents/postgres-expert.json \
  --description "PostgreSQL 专家" \
  --mode subagent \
  --permissions read,edit,bash(psql:*) \
  --model opencode-zen/sonnet

# 调用
"用 @postgres-expert 优化这条慢 SQL"
```

**2026 末支持 subagent 嵌套**(跟 Claude Code 5 层对齐)。

#### 3. Web 远程模式(2026 重写)

```bash
# 启动 Web server
opencode web --port 4096 --hostname 0.0.0.0

# 客户端 attach
opencode attach http://server-ip:4096
```

支持:
- 浏览器端 TUI(任何设备)
- 团队成员共享一个 server
- 会话持久化在 server

#### 4. Desktop App 重构

Tauri 重构,内存占用从 400MB 降到 80MB,启动速度 +5x。

### 已知坑(2026-06)

| 坑 | 解决 |
|---|------|
| `baseURL` 不带 `/v1` → 404 | 强制带 `/v1` |
| 跟 Claude Code 同时跑会冲突 plugin 路径 | 用 `--profile opencode` 隔离 |
| Desktop 首次启动慢 | 等待 30 秒,后续秒开 |
| Web 模式没 MCP 集成 | 等 2026 H2 |

### 与 Claude Code 的实际选择

| 场景 | 选 OpenCode | 选 Claude Code |
|------|------------|---------------|
| 用非 Claude 模型(便宜/国产) | ✅ | ❌ |
| 用最新 2026 特性(Routines/Ultrareview) | ⚠️ 滞后 | ✅ |
| 跑生产级严肃项目 | ⚠️ 够用 | ✅ |
| 学习/实验多模型 | ✅ | ❌ |
| 需要丰富 plugin 生态 | ⚠️ 起步 | ✅ |
| macOS 原生体验 | ✅ Tauri | ✅ |

**典型配置组合**:
- Claude Code 主力(严肃项目)
- OpenCode 备用(模型对比/跑国产模型)

---

_原 OpenCode 文档 2026-05-24 + 2026-06-20 增量(OpenCode Zen/Subagent 增强/Web 重构)_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 补充 2026-05-24 之后的更新（OpenCode Zen、Subagent 增强） |

---

## 📚 相关文档

- [[AI 编程工具全景图]] — OpenCode 在 7 工具中的位置
- [[Claude Code 知识库]] — 同类工具对比
- [[VS Code 知识库]] — VSCode 集成方式
- [[OpenClaw 知识库]] — OpenClaw 也可调度 OpenCode

_OpenCode 适合多 provider 场景,Claude Code 适合生产严肃项目。_

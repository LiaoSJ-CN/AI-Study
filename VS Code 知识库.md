---
path: VS Code 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: tool
tags: [vscode, editor, ai-extension, copilot]
created: 2026-06-20
updated: 2026-06-20
---

# 在 Mac mini 上安装 VS Code 说明文档

## 安装方式

VS Code 在 macOS 上有两种安装方式，推荐使用 **Homebrew** 安装。

### 方式一：Homebrew 安装（推荐）

1. 打开终端（Terminal）
2. 运行以下命令：

```bash
brew install --cask visual-studio-code
```

3. 等待安装完成，VS Code 会自动安装到 `/Applications` 目录

**优势：** 方便后续更新，一条命令搞定

### 方式二：官网下载安装

1. 访问官网：https://code.visualstudio.com/
2. 点击下载 macOS 版本（Apple Silicon 或 Intel，根据你的 Mac mini 芯片选择）
3. 下载完成后，打开 `.dmg` 文件
4. 将 VS Code 图标拖拽到 Applications 文件夹

---

## 首次启动配置

### 1. 安装 code 命令行工具

打开 VS Code，按 `Cmd+Shift+P`，输入并选择：

```
Shell Command: Install 'code' command in PATH
```

这样可以在终端直接用 `code` 命令打开文件或文件夹。

### 2. 常用命令

```bash
# 打开当前目录
code .

# 打开指定文件
code filename.js

# 打开指定文件夹
code /path/to/project
```

---

## 推荐安装的扩展

| 扩展名 | 用途 |
|--------|------|
| Chinese (Simplified) | 简体中文语言包 |
| GitLens | 增强 Git 功能 |
| Prettier | 代码格式化 |
| ESLint | JavaScript/TypeScript 代码检查 |
| Python | Python 开发支持 |
| Docker | Docker 容器管理 |

安装方式：点击左侧扩展图标（四个方块），搜索扩展名，点击安装。

---

## 常用快捷键

| 快捷键 | 功能 |
|--------|------|
| `Cmd + P` | 快速打开文件 |
| `Cmd + Shift + P` | 命令面板 |
| `Cmd + ,` | 打开设置 |
| `Cmd + B` | 显示/隐藏侧边栏 |
| `Cmd + \` | 拆分编辑器 |
| `Cmd + /` | 注释/取消注释 |
| `Option + ↑/↓` | 移动当前行 |
| `Option + Shift + ↑/↓` | 复制当前行 |

---

## 卸载方法

### Homebrew 卸载

```bash
brew uninstall --cask visual-studio-code
```

### 手动卸载

1. 将 Applications 中的 VS Code 拖入废纸篓
2. 删除配置目录（可选）：

```bash
rm -rf ~/.vscode
rm -rf ~/Library/Application\ Support/Code
```

---

## 常见问题

**Q: 安装后打不开？**  
A: 前往「系统设置」→「隐私与安全性」，允许从 App Store 和被认可的开发者下载的应用。

**Q: 如何更新 VS Code？**  
A: Homebrew 安装：`brew upgrade --cask visual-studio-code`；官网安装：VS Code 会自动检测更新。

**Q: M系列芯片和 Intel 版本有什么区别？**  
A: M系列（Apple Silicon）选择 ARM64 版本，Intel Mac mini 选择 x64 版本。选错也能运行，但性能会有损耗。

**Q: GitLens
A: dbffc76d-88f3-4657-9cc3-c8beb49b3777

---

*文档生成时间：2026-03-06*

---

## AI 编程扩展（2026 增量）

> 本节补充 VS Code AI 生态集成方式。

### 主流 AI 扩展

| 扩展 | 厂商 | 能力 |
|------|------|------|
| **GitHub Copilot** | GitHub/OpenAI | 行内补全 + Chat + Agent Mode |
| **Claude Code for VSCode** | Anthropic | 终端 agent 集成到 IDE |
| **Cursor** | Cursor Inc. | VSCode 改造的 AI-first IDE |
| **Codeium/Windsurf** | Codeium | 免费 Copilot 替代 |
| **Tabby** | TabbyML | 开源自托管 AI 补全 |
| **Continue.dev** | Continue | 开源 Copilot 替代,接任意模型 |

### Claude Code for VSCode 集成

```bash
# 安装扩展
code --install-extension anthropic.claude-code

# 配置 ~/.claude/settings.json 同步
# VSCode 扩展跟 CLI 共享:
# - Skills
# - Plugins(ECC/Superpowers)
# - Hooks
# - MCP servers
# - CLAUDE.md
```

**核心优势**:在 VSCode 里直接用 Claude Code 全套 subagent/hooks/MCP,不用切终端。

### Cursor 集成(可选)

Cursor 是 VSCode 改造版,深度 AI 化:

```bash
# 下载
brew install --cask cursor

# 配置同步 Cursor + Claude Code
# ~/.claude.json 同时支持两个 IDE
# 注意:Cursor 跑 ECC 时需独立 agent-data root
```

### MCP 集成(2026 重点)

```json
// .vscode/mcp.json
{
  "servers": {
    "codegraph": {
      "command": "codegraph",
      "args": ["mcp"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres"],
      "env": { "DATABASE_URL": "postgresql://..." }
    }
  }
}
```

VSCode 1.96+ 原生支持 MCP,可在 IDE 里直接调用外部工具。

### 调试配置

```json
// .vscode/launch.json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Python: 当前文件",
      "type": "debugpy",
      "request": "launch",
      "program": "${file}",
      "console": "integratedTerminal"
    }
  ]
}
```

### 推荐扩展(AI 时代 2026)

| 扩展 | 用途 |
|------|------|
| `anthropic.claude-code` | Claude Code 集成 |
| `github.copilot` | Copilot(配合 Chat) |
| `github.copilot-chat` | Copilot Chat |
| `continue.continue` | 开源 AI 助手 |
| `ms-python.python` | Python 必备 |
| `ms-python.black-formatter` | 格式化 |
| `dbaeumer.vscode-eslint` | JS/TS lint |
| `esbenp.prettier-vscode` | 格式化 |
| `eamodio.gitlens` | Git 增强 |
| `pkief.material-icon-theme` | 主题 |
| `tamasfe.even-better-toml` | TOML 支持 |

### 设置同步

```bash
# 开启 Settings Sync(跨设备同步)
# macOS:Cmd+Shift+P → "Turn on Settings Sync"
# 登录 GitHub 账号,选同步内容:
#   ✓ Settings
#   ✓ Keybindings  
#   ✓ Extensions
#   ✓ Snippets
#   ✓ UI State
```

---

_原 VSCode 安装文档 + 2026-06-20 增量(AI 生态集成)_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 补充 AI 编程扩展（Copilot/Claude Code/Continue） |

---

## 📚 相关文档

- [[Claude Code 知识库]] — Claude Code for VSCode 集成
- [[OpenCode 知识库]] — OpenCode 也支持 VSCode 集成
- [[AI 编程工具全景图]] — VSCode + 各 AI 工具的协同方式
- [[OpenClaw 知识库]] — OpenClaw 调试 VSCode 集成

_VSCode 是 2026 AI 编程的主战场，3 大扩展(Copilot/Claude Code/Continue)同时存在。_

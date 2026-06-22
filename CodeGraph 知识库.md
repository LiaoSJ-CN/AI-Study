---
path: CodeGraph 知识库.md
workspace: ~/Documents/obsidian/ai-study/
category: ai-coding
tags: [codegraph, mcp, knowledge-graph, local-search]
created: 2026-06-20
updated: 2026-06-20
---

# CodeGraph知识库

> CodeGraph 是一个本地代码知识图谱,把代码库预索引成"符号-调用-依赖"图谱,让 AI 编程 agent 用查询代替 grep/Read,大幅减少 token 消耗。

**仓库**：[github.com/colbymchenry/codegraph](https://github.com/colbymchenry/codegraph)
**作者**：Colby McHenry
**特性**：100% local、自动同步、节省 token & tool calls
**更新时间**：2026-06-20

---

## 核心定位

> **不是"画代码地图"的可视化工具,是 agent 用的"代码搜索引擎"**。

### 解决什么问题

Claude Code 探索代码库时,会 spawn Explore agents 用 grep/glob/Read 扫文件,**每扫一次烧 token、每读一次烧 tool call**。一个中型项目探索 5-10 次就是几百万 token。

CodeGraph 把整个项目预索引成知识图谱(符号关系 + 调用图 + 代码结构),agent 直接查图谱,瞬间返回。

### 跟其他方案对比

| 方案 | 做法 | 缺点 |
|------|------|------|
| Claude Code 默认 | grep/glob/Read 扫文件 | 烧 token、慢 |
| GitDiagram / FalkorDB | 画架构图给人看 | 不可被 agent 查询 |
| ECC codemaps | 输出 markdown 文档 | 静态、不实时更新 |
| **CodeGraph** | **预索引图谱 + MCP 暴露给 agent** | ✅ 实时、自动、agent 原生 |

---

## 核心特性

| 特性 | 说明 |
|------|------|
| **Pre-indexed** | 一次性建图,后续秒级查询 |
| **Auto-sync** | 文件改动后自动更新图谱(agent 编辑时也同步) |
| **100% local** | 数据不外传,MCP server 本地跑 |
| **MCP-native** | 通过 MCP server 把工具暴露给 agent,带 usage guidance |
| **Token 节省** | 探索代码库的 token 消耗减少 50%+ |
| **Tool call 节省** | 替代多次 grep/Read,1 次 query 顶 10 次 Read |
| **多语言** | 支持主流语言(TS/JS/Python/Go/Java/Rust…) |

---

## 安装方式

### 一键安装脚本(按平台)

| 平台 | 命令 |
|------|------|
| macOS / Linux | `curl -fsSL https://codegraph.dev/install.sh \| bash` |
| Windows (PowerShell) | `irm https://codegraph.dev/install.ps1 \| iex` |
| npm 通用 | `npm install -g codegraph` |

> ⚠️ installer 把 `codegraph` 放进 PATH,但**不会修改当前 shell**。装完后开新终端,再跑下一步。

### 三步接入

```bash
# 1. 装 CLI(见上)
codegraph --version

# 2. 新终端,跑 installer 检测并配置 agent
codegraph install
# 交互式选项:target、location、permissions
# 支持:Claude Code、Cursor、Codex CLI、opencode、Hermes Agent、Gemini CLI、Antigravity IDE、Kiro

# 3. 在项目里初始化
codegraph init
# 自动建 .codegraph/ 目录,首跑建完整索引,后续自动同步
```

### 配置选项

```bash
codegraph install \
  --target auto|all|none|claude,cursor,... \
  --location global|local \
  --yes \
  --no-permissions \
  --print-config <id>
```

- `--target` 指定哪些 agent 要接(默认 `auto` 自动检测)
- `--location` 全局(写 `~/.claude.json`)还是项目内(写 `.claude/settings.json`)
- `--no-permissions` 跳过权限配置
- `--print-config` 输出当前配置,不写入

### 配置示例(手动)

全局 `~/.claude.json`:

```json
{
  "mcpServers": {
    "codegraph": {
      "command": "codegraph",
      "args": ["mcp"]
    }
  }
}
```

可选 `~/.claude/settings.json`(给某些命令 auto-allow):

```json
{
  "permissions": {
    "allow": [
      "mcp__codegraph__*"
    ]
  }
}
```

---

## MCP 工具(MCP functions)

CodeGraph 通过 MCP server 暴露 4 个核心工具 + 1 个初始化响应:

| 工具 | 用途 |
|------|------|
| `codegraph_explore` | 浏览项目结构(模块/目录/顶层入口) |
| `codegraph_search` | 按名字/类型搜符号(function/class/interface…) |
| `codegraph_callers` | 反向查询:谁调用了这个函数/类 |
| `codegraph_node` | 查单个节点详情(签名、参数、位置、文档) |

**MCP initialize 响应**会自动给 agent 发"使用指引"(在 `src/mcp/server-instructions.ts` 里写死,是 single source of truth),告诉 agent 何时用哪个工具。

> ⚠️ subagent 和非 MCP harness 看不到 MCP 自带指引。installer 会额外写一段 marker-fenced section 到 agent 的 instructions 文件,指向 CLI 等价命令(`codegraph explore` / `codegraph node`)。

---

## 日常使用流程

```bash
# 在项目里,代码改动后强制同步
codegraph sync

# 查看状态
codegraph status

# 反初始化(删除 .codegraph/)
codegraph uninit

# 反安装(从所有 agent 移除 MCP 配置)
codegraph uninstall
```

agent 启用 MCP 后,行为自动变化:

1. 探索代码库 → 自动调 `codegraph_explore` / `codegraph_search`
2. 找调用关系 → 自动调 `codegraph_callers`
3. 看具体符号 → 自动调 `codegraph_node`
4. 不再疯狂 grep/Read

---

## 故障排查

| 问题 | 原因 | 解决 |
|------|------|------|
| **MCP server 连不上** | 项目没初始化 | 跑 `codegraph init`,确认 `.codegraph/` 存在 |
| **MCP server 连不上** | 路径配错 | 重跑 `codegraph install` 改写配置 |
| **符号找不到** | 还没同步完 | 等几秒,或手动 `codegraph sync` |
| **符号找不到** | 语言不支持 | 查 `Supported Languages` 列表 |
| **符号找不到** | 路径被排除 | 检查 `.gitignore`、`node_modules`、`dist` 等默认排除 |
| **agent 不用 CodeGraph** | MCP server 没加载 | **重启 agent**(MCP server 是 agent 启动时加载) |
| **agent 不用 CodeGraph** | 项目没初始化 | 跑 `codegraph init` |

---

## 与其它工具的协同

| 组合 | 效果 |
|------|------|
| CodeGraph + Claude Code | 默认探索场景的 token 减半 |
| CodeGraph + ECC | 跟 ECC 的 `update-codemaps` 互补(CodeGraph 实时,ECC codemap 静态) |
| CodeGraph + Superpowers | Subagent-driven 阶段直接 query 图谱,不烧 subagent token |
| CodeGraph + OpenCode | OpenCode 12 events hook,可以接 CodeGraph 的 sync 事件 |

---

## 关键设计理念

1. **Local-first**:数据全在 `.codegraph/` 目录,无云端依赖,适合私密项目。
2. **MCP-native**:走标准 MCP 协议,任何支持 MCP 的 harness 都能用。
3. **Marker-fenced instructions**:非 MCP harness 走 fallback,写一段 fenced section 到 instructions 文件,清晰标识哪些是 CodeGraph 加的。
4. **One install, all projects**:一次全局 `codegraph install` 永久生效,新项目只跑 `codegraph init`。
5. **Single source of truth**:MCP 用法指引只写在 `src/mcp/server-instructions.ts`,避免分散。

---

## 工具评估(2026-06-20)

✅ **优势**
- **解决真问题**:Claude Code 探索代码库的 token 浪费是公认的痛
- **MCP-native**:跟整个 AI 编程生态走同一个协议
- **本地优先**:适合对数据敏感的开发者(处理敏感项目、商业代码)
- **多 harness**:不只是 Claude Code,Cursor/Codex/OpenCode/Hermes/Kiro 都能用
- **集成成本低**:三步装好,后续全自动

⚠️ **注意**
- 项目初始化**首次建图**对超大 monorepo 可能慢(几分钟到十几分钟)
- 索引语言支持需要查 `Supported Languages`,冷门语言可能 fallback
- `node_modules`/`dist` 默认排除,要看清楚
- 改动后**等几秒**再让 agent 查询,否则可能看到旧状态

🎯 **适用场景**
- 经常让 Claude Code 探索中型/大型代码库
- 多人协作,需要一致的代码知识图谱
- 本地代码不让外传
- 多 harness 切换(Claude Code ↔ Cursor ↔ Codex)

📚 **相关**
- [[ECC 知识库]] — ECC 的 `update-codemaps` 是静态版,CodeGraph 是动态版
- [[Superpowers 知识库]] — Superpowers 的 subagent 阶段配合 CodeGraph 能省大量 token

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 首次结构化（frontmatter + Obsidian 双向链接） |

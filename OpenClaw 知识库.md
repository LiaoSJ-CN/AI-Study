---
path: OpenClaw 知识库.md
workspace: ~/Documents/obsidian/ai-study/
category: ai-coding
tags: [openclaw, gateway, runtime, workboard, mcp]
created: 2026-06-20
updated: 2026-06-20
---

# OpenClaw 知识库

> OpenClaw 是一款 AI 助手运行时，把所有聊天、Agent、技能、记忆、自动化任务统一到一个 Gateway。本文件是命令清单 + 工作区结构 + v2026.6.8 用法的整合版。

**当前版本**：OpenClaw **2026.6.8 (844f405)**
**Gateway 状态**：LaunchAgent loaded · running · pid 754
**Dashboard**：[http://127.0.0.1:18789/](http://127.0.0.1:18789/)
**完整文档**：[https://docs.openclaw.ai/cli](https://docs.openclaw.ai/cli)
**整合时间**：2026-06-20
**整合来源**：原 `OpenClaw命令清单.md`（v2026.3.28）+ 原 `openclaw学习.md`

---

## 快速索引

| 分类 | 命令 |
|------|------|
| [Gateway 网关](#gateway-网关) | `gateway` `tui` `chat` `logs` `dashboard` `health` |
| [配置](#配置) | `config` `configure` `setup` `onboard` `crestodian` |
| [模型 / 推理](#模型--推理) | `models` `infer` |
| [技能](#技能) | `skills` |
| [插件](#插件-v20266-新增) | `plugins` |
| [渠道](#渠道) | `channels` |
| [消息](#消息) | `message` |
| [会话](#会话) | `sessions` |
| [代理 / 节点](#代理--节点) | `agents` `acp` `nodes` `pairing` `devices` `node` |
| [MCP](#mcp-v20266-新增) | `mcp` |
| [记忆](#记忆-v20266-新增) | `memory` |
| [定时任务](#定时任务) | `cron` |
| [安全](#安全) | `secrets` `approvals` `security` `hooks` `exec-policy` |
| [备份 / 迁移](#备份--迁移-v20266-新增) | `backup` `migrate` |
| [代理调用](#代理调用-v20266-新增) | `agent` `proxy` |
| [更新](#更新) | `update` |
| [系统](#系统) | `system` `doctor` `health` `status` |
| [其他](#其他) | `docs` `qr` `completion` `uninstall` `reset` `directory` `dns` `sandbox` `clawbot` `commitments` |

---

## Gateway 网关

### `openclaw gateway [command]`
运行、查看和管理 WebSocket Gateway（默认端口 18789）

| 子命令 | 说明 |
|--------|------|
| 无子命令 | 直接启动 Gateway（前台运行） |
| `gateway status` | 查看 Gateway 运行状态 |
| `gateway stop` | 停止 Gateway |
| `gateway restart` | 重启 Gateway |
| `gateway run` | 启动并替换占用端口的进程 |

**常用选项：**
```
--port <端口>          Gateway WebSocket 端口（默认 18789）
--bind <模式>          绑定模式: loopback / lan / tailnet / auto / custom
--auth <模式>          认证模式: none / token / password / trusted-proxy
--force               杀掉占用端口的进程后再启动
--dev                 开发模式（隔离状态，端口 19001）
--allow-unconfigured  允许在不强制 gateway.mode=local 下启动
--compact             alias for --ws-log compact
--cli-backend-logs    只显示 CLI backend 日志
--raw-stream          把模型流事件记到 jsonl
```

> 💡 在 macOS 上，Gateway 由 LaunchAgent 管理（`~/Library/LaunchAgents/ai.openclaw.gateway.plist`）。日常用 `launchctl` 或 `openclaw gateway restart`，不要直接 kill。

### `openclaw tui` / `openclaw chat`
打开终端 UI 连接 Gateway（`chat` 是 `tui --local` 的别名）

```bash
openclaw tui
openclaw tui --message "你好"
openclaw tui --session <key>
openclaw chat
```

### `openclaw logs`
查看 Gateway 日志

```bash
openclaw logs
openclaw logs --lines 100
```

### `openclaw dashboard`
打开 Control UI 网页控制台（带当前 token 自动登录）

### `openclaw health`
获取 Gateway 详细健康状态

---

## 配置

### `openclaw configure`
交互式配置向导（修改模型、Gateway、channels、plugins、skills、健康检查）

```bash
openclaw configure
```

### `openclaw onboard`
交互式初始化（Gateway + 工作区 + 技能）

```bash
openclaw onboard
openclaw onboard --flow quickstart
openclaw onboard --flow advanced
```

### `openclaw setup`
初始化本地配置和工作区（baseline）

### `openclaw crestodian` ⭐v2026.6
交互式 **setup and repair assistant**，比 `doctor` 更主动，会引导修复问题

### `openclaw config <子命令>`

| 子命令 | 说明 | 示例 |
|--------|------|------|
| `config get <路径>` | 读取配置值 | `openclaw config get agents.defaults.model.primary` |
| `config set <路径> <值>` | 设置配置值（value 模式） | `openclaw config set gateway.port 19001` |
| `config patch --file <json5>` | JSON5 一次合并写入（推荐用于复杂改动） | `openclaw config patch --file ./openclaw.patch.json5 --dry-run` |
| `config patch --stdin` | 从 stdin 读 patch | |
| `config unset <路径>` | 删除配置值 | |
| `config file` | 显示配置文件路径 | |
| `config schema` | 输出 JSON Schema | |
| `config validate` | 验证配置文件 | |
| 无子命令 | 启动交互式 setup（可加 `--section gateway,channels`） | `openclaw config --section gateway` |

---

## 模型 / 推理

### `openclaw models <子命令>`

| 子命令 | 说明 |
|--------|------|
| `models list` | 列出所有可用模型 |
| `models status` | 查看模型配置状态（支持 `--json` / `--plain`） |
| `models aliases` | 管理模型别名 |
| `models auth` | 管理模型认证 profile |
| `models fallbacks` | 管理文本模型降级列表 |
| `models image-fallbacks` | **v2026.6 新增** 管理图片模型降级列表 |
| `models scan` | 扫描 OpenRouter 免费模型（工具 + 图片能力） |
| `models set <id>` | 设置默认模型 |
| `models set-image <id>` | **v2026.6 新增** 设置图片模型 |

### `openclaw infer|capability <子命令>` ⭐v2026.6
provider-backed 推理命令的稳定 CLI 入口（`capability` 是 `infer` 的别名）

| 子命令 | 说明 |
|--------|------|
| `infer audio` | 音频转写 |
| `infer embedding` | embedding 提供商 |
| `infer image` | 图片生成与描述 |
| `infer model` | 文本推理与模型目录 |
| `infer tts` | 文字转语音 |
| `infer video` | 视频生成与描述 |
| `infer inspect <id>` | 检查某个 canonical capability |
| `infer list` | 列出所有 canonical capability id 与支持的传输 |

---

## 技能

### `openclaw skills <子命令>`

| 子命令 | 说明 |
|--------|------|
| `skills list` | 列出所有可用技能 |
| `skills check` | 检查技能就绪/可见/缺失依赖 |
| `skills info <名称>` | 查看技能详情 |
| `skills install <url/name>` | 从 ClawHub / git / 本地目录安装 |
| `skills update` | 更新 ClawHub 已装技能 |
| `skills search <关键词>` | 在 ClawHub 搜索 |
| `skills verify` | **v2026.6 新增** 用 ClawHub 验证技能安全性 |
| `skills workshop` | **v2026.6 新增** 管理 Skill Workshop 提案（create/update/revise/list/inspect/apply/reject/quarantine） |

> 本地共 76 个 skill（47 ready），关键已装：agent-reach / openclaw-self-improvement / skill-vetter / 1password / data-model-designer / data-standard-manager / aihot / cli-anything / gog / 智谱 GLM / last30days-official / 多个 Anthropic 前端类。

---

## 插件 ⭐v2026.6 新增

OpenClaw 2026.6 把插件系统正式独立出来：

### `openclaw plugins <子命令>`

| 子命令 | 说明 |
|--------|------|
| `plugins list` | 列出已发现的插件 |
| `plugins install <spec>` | 安装插件（支持 path / archive / npm spec / git repo / `clawhub:package` / marketplace） |
| `plugins uninstall` | 卸载插件 |
| `plugins enable` / `disable` | 启用/禁用 |
| `plugins update` | 更新已装插件和 tracked hook packs |
| `plugins search` | 搜索 ClawHub 插件包 |
| `plugins marketplace` | 检查 Claude-兼容的 plugin marketplace |
| `plugins registry` | 检查/重建持久化插件注册表 |
| `plugins inspect <id>` | 查看插件详情 |
| `plugins doctor` | 报告插件加载问题 |
| `plugins build` | 生成简单 tool plugin 元数据 |
| `plugins init` | 创建一个简单的 tool plugin 项目 |
| `plugins validate` | 验证 plugin 元数据 |

---

## 渠道

### `openclaw channels <子命令>`

| 子命令 | 说明 |
|--------|------|
| `channels list` | 列出已配置的渠道（`--all` 看可安装目录） |
| `channels add` | 添加/更新渠道账号 |
| `channels login` | 登录渠道 |
| `channels logout` | 登出渠道 |
| `channels remove` | 禁用/删除渠道账号 |
| `channels status` | 看 Gateway 渠道状态（`status --deep` 看本地） |
| `channels logs` | 从 gateway 日志看渠道日志 |
| `channels capabilities` | 看 provider 能力（intents/scopes + features） |
| `channels resolve` | 把用户名解析为渠道 ID |

---

## 消息

### `openclaw message <子命令>`

| 子命令 | 说明 | 示例 |
|--------|------|------|
| `message send` | 发送消息 | `openclaw message send --channel telegram --target @xxx --message "hi"` |
| `message read` | 读取最近消息 | |
| `message search` | 搜索消息 | |
| `message delete` | 删除消息 | |
| `message edit` | 编辑消息 | |
| `message react` | 添加/删除 reaction | |
| `message pin` / `pins` | 置顶/查看置顶 | |
| `message broadcast` | 广播消息到多个目标 | |
| `message poll` | 发送投票 | |
| `message member` | 成员管理 | |
| `message ban` / `kick` | 封禁/踢出成员 | |
| `message role` | 角色管理 | |
| `message permissions` | 查看权限 | |
| `message thread` | 线程管理 | |
| `message emoji` | Emoji 操作 | |
| `message sticker` | 贴纸操作 | |

---

## 会话

### `openclaw sessions [options]`

```bash
openclaw sessions list
openclaw sessions list --active 30      # 只看最近 30 分钟活跃
openclaw sessions list --all-agents     # 跨所有 agent 汇总
openclaw sessions list --json           # JSON 格式
openclaw sessions list --limit all      # 不限条数
openclaw sessions cleanup               # 清理会话存储
```

---

## 代理 / 节点

### `openclaw agents <子命令>`

| 子命令 | 说明 |
|--------|------|
| `agents list` | 列出已配置的代理 |
| `agents add` | 添加新的隔离 agent |
| `agents delete` | 删除 agent 并清理 workspace/state |
| `agents bind` | 添加路由绑定 |
| `agents unbind` | 移除路由绑定 |
| `agents bindings` | 查看路由绑定列表 |
| `agents set-identity` | 更新 agent 身份（name/theme/emoji/avatar） |

### `openclaw acp`
运行 ACP 桥接（让 IDE / 其他 agent 通过 ACP 接入）

```bash
openclaw acp --session agent:main:main
openclaw acp --reset-session
```

### `openclaw nodes <子命令>`

| 子命令 | 说明 |
|--------|------|
| `nodes list` | 列出节点 |
| `nodes approve` | 审批待定配对请求 |
| `nodes camera` | 从配对节点捕获摄像头 |
| `nodes canvas` | 捕获/渲染节点 Canvas |
| `nodes describe` | 查看节点能力列表 |

### `openclaw pairing`

| 子命令 | 说明 |
|--------|------|
| `pairing list` | 列出待处理配对请求 |
| `pairing approve <代码>` | 批准配对码 |

### `openclaw devices`

| 子命令 | 说明 |
|--------|------|
| `devices list` | 列出已配对设备 |
| `devices approve` | 审批待定设备 |
| `devices clear` | 清除已配对设备 |

### `openclaw node`

| 子命令 | 说明 |
|--------|------|
| `node run` | 前台运行节点主机 |
| `node install` | 安装节点服务（LaunchAgent） |
| `node restart` | 重启节点服务 |
| `node status` | 查看节点状态 |

---

## MCP ⭐v2026.6 新增

### `openclaw mcp <子命令>`

| 子命令 | 说明 |
|--------|------|
| `mcp config` | 查看/管理 MCP 配置 |
| `mcp bridge` | 把 MCP server 桥接到 channel（如让飞书消息触发 MCP tool） |

> 配置 MCP server 推荐走 `openclaw mcp config`，自动写到 `~/.openclaw/mcp.json` 等位置。

---

## 记忆 ⭐v2026.6 新增（独立子系统）

记忆子系统 v2026.6 从 `doctor` 里独立出来,有完整 CLI：

### `openclaw memory <子命令>`

| 子命令 | 说明 |
|--------|------|
| `memory search <query>` | 搜索记忆文件 |
| `memory inspect` | 查看记忆内容详情 |
| `memory index` | 重建记忆索引 |
| `memory promote` | **核心** 排名短期召回,可选把 top entries 写入 MEMORY.md |
| `memory promote-explain` | 解释某个候选 promotion 的打分构成 |
| `memory rem-harness` | 预览 REM 反思 / candidate truths / deep promotion(不写) |
| `memory rem-backfill` | 把历史 grounded REM 摘要写入 DREAMS.md |
| `memory status` | 看记忆子系统状态(`--json` 机器可读) |

**记忆工作流**：
1. 每次对话自动产生短期 recall,累积在 `memory/YYYY-MM-DD.md`
2. 每周一 `memory promote` 自动把高价值内容提炼到 `MEMORY.md`
3. 候选评分公开透明:`memory promote-explain` 看具体打分原因

> `memory-core` 插件是默认装的。检查是否启用：`openclaw memory status`

---

## 定时任务

### `openclaw cron <子命令>`

| 子命令 | 说明 |
|--------|------|
| `cron list` | 列出所有定时任务 |
| `cron add` | 添加任务 |
| `cron edit` | 编辑任务（patch 字段） |
| `cron rm` | 删除任务 |
| `cron enable` / `disable` | 启用/停用 |
| `cron run` | 立即运行任务（调试） |
| `cron runs` | 查看任务运行历史 |
| `cron get <jobId>` | 把单个任务输出 JSON |
| `cron show <jobId>` | 显示任务详情 |
| `cron status` | 调度器状态 |

**定时任务结构**（`cron add` 时填）：
```json
{
  "name": "每日心跳",
  "schedule": { "kind": "cron", "expr": "0 2 * * *", "tz": "Asia/Shanghai" },
  "payload": { "kind": "systemEvent", "text": "..." },
  "sessionTarget": "main",
  "enabled": true
}
```

**⚠️ 升级提示**：从旧版本升级后跑 `openclaw doctor --fix` 自动归一化 cron job 存储格式。

---

## 安全

### `openclaw secrets <子命令>`

| 子命令 | 说明 |
|--------|------|
| `secrets configure` | 交互式密钥配置（provider + SecretRef 映射 + 预检） |
| `secrets reload` | 重新解析密钥并原子热更新 |
| `secrets audit` | 审计明文密钥、未解析引用、优先级漂移 |
| `secrets apply` | 应用已生成的密钥计划 |

> 推荐部署：file-provider SecretRef（`~/.openclaw/secrets.json`，600 权限）。

### `openclaw approvals`

| 子命令 | 说明 |
|--------|------|
| `approvals get` | 获取当前审批快照 |
| `approvals set <文件>` | 用 JSON 文件替换审批规则 |
| `approvals allowlist` | 编辑 agent 白名单 |

### `openclaw security <子命令>`

| 子命令 | 说明 |
|--------|------|
| `security audit` | 本地审计常见安全 foot-guns |
| `security audit --deep` | **v2026.6 新增** 包含最佳努力 live Gateway 探测 + 插件安全审计收集器 |
| `security audit --deep --token <token>` | 带 token 跑深度审计 |

### `openclaw hooks <子命令>`

| 子命令 | 说明 |
|--------|------|
| `hooks list` | 列出所有钩子 |
| `hooks enable` / `disable` | 启用/停用 |
| `hooks info` | 钩子详情 |
| `hooks check` | 检查钩子状态（eligibility） |

### `openclaw exec-policy <子命令>` ⭐v2026.6
同步请求的 exec policy 与 host approvals

```bash
openclaw exec-policy show
openclaw exec-policy sync
```

---

## 备份 / 迁移 ⭐v2026.6 新增迁移命令

### `openclaw backup <子命令>`

| 子命令 | 说明 |
|--------|------|
| `backup create` | 创建本地状态备份归档 |
| `backup verify` | 验证备份归档完整性 |

### `openclaw migrate <子命令>` ⭐v2026.6
从其他 agent 系统导入状态（用于跨平台迁移）

```bash
openclaw migrate --from <system> --source <path>
```

---

## 代理调用 ⭐v2026.6 新增

### `openclaw agent`
运行一次 agent turn 经 Gateway

```bash
openclaw agent --to +15555550123 --message "Run summary" --deliver
openclaw agent --message "hello"
```

### `openclaw proxy <子命令>` ⭐v2026.6
OpenClaw debug proxy,捕获并检查流量

```bash
openclaw proxy start --port 8888
openclaw proxy inspect --last 10
```

### `openclaw commit` & `openclaw commitments <子命令>` ⭐v2026.6
管理推断的 follow-up commitments（agent 自动识别你可能要跟进的事）

```bash
openclaw commitments list
openclaw commitments add --text "..."
openclaw commitments done <id>
```

---

## 更新

### `openclaw update`

| 选项 | 说明 |
|------|------|
| `update` | 检查并安装更新 |
| `update --dry-run` | 预览更新（不实际修改） |
| `update --channel stable` | 切换到稳定版（默认） |
| `update --channel beta` | 切换到 Beta 版 |
| `update --no-restart` | 更新后不重启 Gateway |

**升级经验**（来自 MEMORY）：
- launchd 管理的 gateway 下 `update.run` 走 managed-service handoff 会失灵（SIGUSR1 不是退出信号）
- 可行流程：写 launcher 轮询 PID → `nohup launcher.sh &; disown` → `launchctl stop ai.openclaw.gateway` → launchd KeepAlive=true 自动拉起新版本
- 升级示例：v2026.6.6 → v2026.6.8（plist 已重装）

---

## 系统

### `openclaw doctor`
诊断 + 修复 config、Gateway、插件、渠道问题

```bash
openclaw doctor           # 检查
openclaw doctor --fix     # 自动修复
openclaw doctor --force   # 强制
```

### `openclaw status`
显示渠道健康状态、最近会话、Gateway 状态（`status --deep` 看本地深入信息）

### `openclaw health`
获取 Gateway 详细健康（`status` 的升级版，含 latency/认证/插件兼容性）

### `openclaw system <子命令>`

| 子命令 | 说明 |
|--------|------|
| `system event` | 触发系统事件 |
| `system heartbeat` | 心跳控制 |
| `system presence` | 列出系统存在状态 |

---

## 其他

### `openclaw docs [查询]`
搜索 OpenClaw 官方文档

```bash
openclaw docs agent memory
```

### `openclaw qr`
生成 iOS 配对二维码

```bash
openclaw qr
openclaw qr --remote
```

### `openclaw completion <shell>`
生成 Shell 补全脚本

```bash
openclaw completion zsh > ~/.zshrc   # 注意:会覆盖,zshrc 已有内容要先备份
openclaw completion bash > ~/.bashrc
```

### `openclaw reset`
重置本地配置/状态（保留 CLI）

```bash
openclaw reset --dry-run
openclaw reset --non-interactive
```

### `openclaw uninstall`
卸载 Gateway 服务（保留 CLI）

### `openclaw sandbox <子命令>`

| 子命令 | 说明 |
|--------|------|
| `sandbox list` | 列出沙箱容器 |
| `sandbox recreate` | 重建容器 |
| `sandbox explain` | 解释沙箱工具策略 |

### `openclaw dns <子命令>`
CoreDNS 服务发现

### `openclaw directory <子命令>`

| 子命令 | 说明 |
|--------|------|
| `directory self` | 显示当前账号 |
| `directory peers` | 通讯录 |
| `directory groups` | 群组目录 |

### `openclaw clawbot <子命令>` ⭐v2026.6
Legacy clawbot 命令别名（向后兼容）

---

## 工作区结构

OpenClaw 的提示词由以下文件组成，位于 `~/.openclaw/workspace/`：

| 文件 | 作用 |
|------|------|
| `USER.md` | 记录使用者基本信息（姓名、时区、偏好），用于个性化交互 |
| `IDENTITY.md` | 定义 AI 自身身份（名字、气质、emoji、头像） |
| `SOUL.md` | AI 核心准则与人格特质（真诚、个性、自主钻研等） |
| `AGENTS.md` | 多智能体规则、群聊礼仪、权限策略 |
| `TOOLS.md` | 工具使用指导（cron.add、web_search 等），可加具体 Prompt 规则 |
| `MEMORY.md` | 长期记忆，每次对话注入作为上下文 |
| `HEARTBEAT.md` | 定时检查任务清单（心跳执行的内容） |
| `memory/YYYY-MM-DD.md` | 每日对话原始记录 |
| `skills/` | 本地开发的 skills |

---

## 常用路径

```
Dashboard  : http://127.0.0.1:18789/
状态目录   : ~/.openclaw/
工作区     : ~/.openclaw/workspace/
默认配置   : ~/.openclaw/openclaw.json
密钥文件   : ~/.openclaw/secrets.json  (600 权限)
Plugin 数据: ~/.openclaw/plugins/
Sessions   : ~/.openclaw/agents/<agent>/sessions/
Skills 安装: ~/.npm-global/lib/node_modules/openclaw/skills/
Gateway plist: ~/Library/LaunchAgents/ai.openclaw.gateway.plist  (macOS LaunchAgent)
```

---

## 常用场景速查

| 场景 | 命令 |
|------|------|
| 启动 Gateway | `openclaw gateway` |
| 查看状态 | `openclaw status` |
| 健康检查 | `openclaw doctor` |
| 自动修复 | `openclaw doctor --fix` |
| 深度安全审计 | `openclaw security audit --deep` |
| 交互修复 | `openclaw crestodian` |
| 安装技能 | `openclaw skills install <name>` |
| 配置向导 | `openclaw configure` |
| 看文档 | `openclaw docs <query>` |
| 查看日志 | `openclaw logs --lines 100` |
| 升级 | `openclaw update` |
| 升级预览 | `openclaw update --dry-run` |
| 发送消息 | `openclaw message send --channel telegram --target @xxx --message "hi"` |
| 列出会话 | `openclaw sessions list` |
| 列出定时任务 | `openclaw cron list` |
| 列出渠道 | `openclaw channels list` |
| 查看模型 | `openclaw models list` |
| 查看技能 | `openclaw skills list` |
| 列出插件 | `openclaw plugins list` |
| 搜索 ClawHub | `openclaw skills search <query>` |
| 记忆搜索 | `openclaw memory search <query>` |
| 记忆晋升 | `openclaw memory promote` |

---

## v2026.6 → v2026.6.8 升级要点

**新增命令**：`plugins`、`mcp`、`memory`、`infer|capability`、`exec-policy`、`migrate`、`agent`、`proxy`、`commitments`、`clawbot`、`crestodian`、`chat`

**新增子命令**：
- `models image-fallbacks` / `models set-image`
- `skills verify` / `skills workshop`
- `cron get` / `cron show` / `cron status`
- `security audit --deep`
- `config patch` / `config unset`
- `agents set-identity`（参数扩展）

**重要破坏性变化**：
- 老的 `/brainstorm`、`/write-plan` 等 slash command 已被 skill 替代（影响 Superpowers 等插件用户）
- cron job 存储格式可能变了，跑 `doctor --fix` 归一化
- MEMORY.md promotion 候选改用 `memory promote-explain` 看分数

---

## 已装 Skills 概览（截至 2026-06-20）

**核心三件套**：`agent-reach` `openclaw-self-improvement` `skill-vetter`

**常用类**：`1password`（密钥管理）`gog`（Google Workspace）`aihot`（AI 资讯）`cli-anything`（CLI 工具生成）`智谱 GLM`（多模型）

**数据/编程类**：`data-model-designer`（维度+宽表建模）`data-standard-manager`（数据标准 CRUD）`last30days-official`（HN/X/Tavily 三渠道）

**前端类**：多个 Anthropic 官方 skill（`frontend-design` 等）

完整 76 个 skill 中 47 个 ready,其余需要额外依赖。

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 整合 `OpenClaw命令清单.md` (v2026.3.28) + `openclaw学习.md`,升级到 **v2026.6.8**,补全新增命令（plugins/mcp/memory/infer/exec-policy/migrate/agent/proxy/commitments/crestodian/chat/clawbot） |
| 2026-03-30 | 初始 `OpenClaw命令清单.md` 基于 v2026.3.28 创建 |
| 更早 | `openclaw学习.md` 工作区结构 + 常用命令简表 |

---

_完整文档：[https://docs.openclaw.ai/cli](https://docs.openclaw.ai/cli) — 任何命令加 `--help` 看详细用法_
---

## Workboard 任务调度系统 ⭐v2026.6

> Workboard 是 OpenClaw 2026.6 启用的**任务卡片 + Worker 调度系统**，类似 Jira/Trello 的轻量版，但深度集成 OpenClaw Agent 调度。

**设计目标**：把"复杂多步任务"拆解成可追踪、可编排、可审计的卡片系统。

**状态**：✅ 已启用插件：70/96。

---

### 1. 核心概念

| 概念 | 说明 |
|------|------|
| **Card（卡片）** | 任务的最小单位，有状态、负责人、依赖 |
| **Board（看板）** | 卡片的命名空间（默认 `default`） |
| **Status（状态）** | 卡片生命周期：triage → todo → running → completed/failed/blocked |
| **Worker（工人）** | 执行卡片的 agent（主 agent 或 subagent） |
| **Dispatch（派发）** | 把 ready 卡片路由给空闲 worker |
| **Heartbeat（心跳）** | Worker 长任务中定期汇报 |
| **Proof（证据）** | 完成后附带的测试/截图/日志证明 |
| **Dependency（依赖）** | 卡片间的前置关系（parent → child） |
| **Decompose（分解）** | 把大卡片自动拆成子卡片 |
| **Notify（通知）** | 卡片事件（completed/failed/stale）的订阅推送 |

### 2. 卡片生命周期

```
triage(待审)
  ↓ 人工/自动 specify
todo(待办)
  ↓ 依赖满足 + schedule 到时
ready(就绪)
  ↓ dispatch
running(执行中)
  ├─ heartbeat 周期续期
  ├─ block 阻塞
  └─ 子卡片被 decompose 出去
completed(成功) / failed(失败) / blocked(阻塞)
```

### 3. 核心子命令速查

| 子命令 | 用途 |
|--------|------|
| `openclaw workboard create <title>` | 新建卡片 |
| `openclaw workboard list` | 列出卡片（`--status` `--agent` `--board` 过滤） |
| `openclaw workboard show <id>` | 卡片详情 |
| `openclaw workboard dispatch` | 派发 ready 卡片给 worker |

**create 完整选项**：
```bash
openclaw workboard create "实现 OAuth 登录" \
  --agent main \
  --board feature-auth \
  --priority high \
  --labels "auth,backend" \
  --notes "接入飞书 OAuth 2.0,需要 PKCE flow" \
  --status todo
```

**list 过滤**：
```bash
openclaw workboard list --status running
openclaw workboard list --agent main
openclaw workboard list --board feature-auth --priority urgent
openclaw workboard list --limit 10
```

### 4. 完整工具清单（27 个）

| 工具 | 用途 | 何时用 |
|------|------|--------|
| `workboard_create` | 创建卡片 | 接到新需求 |
| `workboard_list` | 列出卡片 | 查工作流状态 |
| `workboard_read` | 读卡片详情（含 comments/attempts/proof） | 接卡片时 |
| `workboard_claim` | 认领卡片（拿到 token） | worker 开始干 |
| `workboard_heartbeat` | 续期 claim | 长任务中（避免 stale） |
| `workboard_complete` | 完成卡片（带 summary + proof + artifacts） | 干完时 |
| `workboard_release` | 释放 claim（暂停/移交） | 让出资源 |
| `workboard_reclaim` | 释放过期 claim 让其他人接 | 救场 |
| `workboard_block` | 阻塞卡片（带原因） | 等外部信息 |
| `workboard_unblock` | 解除阻塞（带补充） | 恢复 |
| `workboard_specify` | 细化卡片（从 triage → todo） | 任务清楚后 |
| `workboard_decompose` | 拆解为子卡片 | 大任务拆分 |
| `workboard_link` | 关联父→子卡片 | 显式依赖 |
| `workboard_promote` | 把 dependency-ready 卡片推到 ready | 解锁 |
| `workboard_reassign` | 改 assignee + reset failure | 换人/救场 |
| `workboard_comment` | 加评论 | 沟通 |
| `workboard_proof` | 附证据（test/截图/日志） | 验证完成 |
| `workboard_attachment_*` | 加/读/删附件 | 截图/文档 |
| `workboard_worker_log` | 记 worker 日志（level + message） | 排错 |
| `workboard_runs` | 查历史运行 | 审计 |
| `workboard_notify_*` | 订阅/取消/读 事件 | webhook/push |
| `workboard_boards` | 看/建/删/归档 board | 多项目隔离 |
| `workboard_board_create` | 建 board | 新项目 |
| `workboard_protocol_violation` | 标记 worker 违规（没 complete/block 就停） | 异常检测 |

### 5. 实战：3 步接一张卡片

```bash
# 1. 看 ready 卡片
openclaw workboard list --status ready

# 2. 接卡片（拿到 claim token）
TOKEN=$(openclaw workboard claim <card-id>)

# 3. 干活 + 续期
# 干活...
openclaw workboard heartbeat <card-id> --token $TOKEN
# 又干活...
openclaw workboard heartbeat <card-id> --token $TOKEN
# 干完 + 证据
openclaw workboard complete <card-id> --token $TOKEN \
  --summary "实现完成" \
  --proof '{"status":"passed","label":"test","command":"npm test"}' \
  --artifacts '[{"label":"screenshot","path":"./auth-flow.png"}]'
```

### 6. 实战：自动拆解大任务

```bash
# 1. 创建大卡片
BIG=$(openclaw workboard create "实现完整 BI 模块" --priority high --status todo)

# 2. 拆成子卡片
openclaw workboard decompose $BIG \
  --child "设计 schema" \
  --child "实现 API" \
  --child "前端集成" \
  --child "E2E 测试"

# 3. 自动派发
openclaw workboard dispatch --board default
```

### 7. 实战：失败恢复

```bash
# 看到 failed 卡片
openclaw workboard list --status failed

# 查看失败原因
openclaw workboard show <card-id>
# 找到 worker_log 里的错误

# 改派 + 重置
openclaw workboard reassign <card-id> --agent main --reset-failures
```

### 8. 持久化存储

| 路径 | 内容 |
|------|------|
| `~/.openclaw/agents/<agent>/workboard.json` | 卡片元数据 |
| `~/.openclaw/agents/<agent>/workboard-attachments.kv` | 附件（plugin SQLite KV） |
| `~/.openclaw/agents/<agent>/workboard-notify.kv` | 通知订阅 |
| `~/.openclaw/agents/<agent>/workboard-runs/` | 历史 run 记录 |

> 跨设备/agent 共享：把 `workboard.json` 加 git 或 `openclaw backup create`。

### 9. 跟 sessions_spawn 的关系

Workboard **不替代** `sessions_spawn`，而是**互补**：

| 场景 | 用 Workboard | 用 sessions_spawn |
|------|-------------|-------------------|
| 简单一次性并行 | ❌ 太重 | ✅ 直接 spawn |
| 多步骤 + 依赖 + 审计 | ✅ 必用 | ❌ 缺追踪 |
| 长任务 + 心跳 | ✅ heartbeat | ❌ 没机制 |
| 失败重试 | ✅ reassign | ❌ 手动 |
| 跨 session 状态 | ✅ 持久化 | ❌ 内存里 |

**判断标准**：
- 任务 < 5 分钟、简单 → `sessions_spawn`
- 任务 > 5 分钟、复杂、有依赖 → **Workboard 卡片**

### 10. 推荐配置

**默认 board**：`default`（1 个 agent，0 个 board 多余）

**何时建多 board**：
- 多项目并行（每个项目一个 board）
- 多人协作（每人一个 board）
- 多 agent 隔离（每个 agent 一个 board）

```bash
# 建新 board
openclaw workboard boards  # 看现有
# CLI 当前没 boards 命令（plugin 工具用 workboard_boards）
```

### 11. 实战建议

1. **小任务别用**：< 2 步的直接 `sessions_spawn`
2. **decompose 别过细**：5-10 个子卡片上限，否则调度开销 > 收益
3. **proof 必带**：完成卡片时不带 `proof` 容易被 reviewer 退
4. **heartbeat 周期 60s**：长任务每分钟续一次，避免 stale 触发 reclaim
5. **block 原因要细**：block 时 `reason` 写清楚，否则 7 天后没人记得为什么阻塞

---

_2026-06-20 加入 OpenClaw 知识库（Workboard 是 v2026.6 启用的独立子系统）_

---

## 📚 相关文档

- [[AI 编程工具全景图]] — OpenClaw 在 7 工具中的位置
- [[Claude Code 知识库]] — OpenClaw 主力调用的编程 agent
- [[Hermes 知识库]] — OpenClaw 的自我学习替代方案
- [[Skill 系统对比]] — OpenClaw Skills vs 其他 3 套

_OpenClaw 是 7 工具的"总入口"，Workboard 是它内置的任务调度子系统。_

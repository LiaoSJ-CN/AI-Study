---
path: Hermes 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: ai-coding
tags: [hermes, nous-research, self-evolution, gepa]
created: 2026-06-20
updated: 2026-06-20
---

# Hermes知识库

> Hermes Agent 是 NousResearch 开源的 AI Agent 框架，定位是「会自我成长的 Agent」。用得越久越聪明，自动沉淀 skill、优化记忆、跨会话召回历史。

---

## 一、是什么

- **定位**：自我成长型 AI Agent，专注长期记忆与自动学习闭环
- **对比 OpenClaw**：OpenClaw 是自托管 AI Agent 网关（多渠道消息路由），Hermes 是会自我进化的 Agent
- **核心能力**：内置学习闭环、自动沉淀 skill、优化记忆、跨会话召回历史、对用户建立越来越深的画像
- **GitHub**：https://github.com/NousResearch/hermes-agent
- **官网文档**：https://hermes-agent.nousresearch.com/docs

---

## 二、安装

### 官方一键安装（推荐）

```bash
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash
```

安装过程自动检测：Python 3.11、uv、Git、Node.js、ripgrep、ffmpeg。

### 国内镜像安装（网络不佳时）

```bash
# 使用国内镜像克隆
git clone https://gitcode.com/GitHub_Trending/he/hermes-agent.git ~/.hermes/hermes-agent

# 或 ghfast.top 镜像
git clone https://ghfast.top/https://github.com/NousResearch/hermes-agent.git ~/.hermes/hermes-agent

# 设置 PyPI 镜像
export UV_INDEX_URL=https://mirrors.aliyun.com/pypi/simple/

# Playwright 浏览器镜像
export PLAYWRIGHT_DOWNLOAD_HOST=https://npmmirror.com/mirrors/playwright
```

### 国内一键脚本（推荐国内用户）

```bash
curl -fsSL https://raw.githubusercontent.com/itech001/theaiera/main/scripts/install-cn.sh | bash
```

自动切换：PyPI → 阿里云镜像，npm → npmmirror，Playwright → npmmirror。

### 手动安装步骤

```bash
# 1. 安装 uv
curl -LsSf https://astral.sh/uv/install.sh | sh

# 2. 创建虚拟环境
python3.11 -m venv ~/.hermes/hermes-agent/venv
source ~/.hermes/hermes-agent/venv/bin/activate

# 3. 安装核心依赖
uv pip install hermes-agent

# 4. 安装浏览器工具（可选）
cd ~/.hermes/hermes-agent && npx playwright install chromium

# 5. 配置 API Key（写入 .env 文件）
echo 'KIMI_API_KEY=sk-xxx' >> ~/.hermes/.env
```

---

## 三、配置

### 配置文件位置

| 文件 | 用途 |
|------|------|
| `~/.hermes/config.yaml` | 主配置（模型、终端、显示等） |
| `~/.hermes/.env` | API Key 和密钥（不会进入版本控制） |

### 基本配置命令

```bash
# 查看当前配置
hermes config show

# 编辑配置文件
hermes config edit

# 设置配置项（config.yaml）
hermes config set model.default kimi-k2.6
hermes config set model.provider kimi-coding
hermes config set model.base_url https://api.kimi.com/coding

# 运行诊断
hermes doctor

# 查看版本
hermes --version
```

### 配置模型（交互式向导）

```bash
# 进入模型配置向导
hermes model
```

### 国内常用模型配置（Kimi / Moonshot）

**1. 写入 .env 文件**

```bash
echo 'KIMI_API_KEY=sk-kimi-xxx' >> ~/.hermes/.env
```

**2. 设置 config.yaml**

```bash
hermes config set model.default kimi-k2.6
hermes config set model.provider kimi-coding
hermes config set model.base_url https://api.kimi.com/coding
```

**3. 显式配置辅助模型（解决标题生成 404 问题）**

```bash
hermes config set auxiliary.title_generation.provider kimi-coding
hermes config set auxiliary.title_generation.model kimi-k2.6
hermes config set auxiliary.title_generation.base_url https://api.kimi.com/coding/v1
hermes config set auxiliary.title_generation.api_key sk-kimi-xxx
```

**其他国内模型**

| 模型 | provider | API Key 环境变量 | base_url |
|------|----------|------------------|----------|
| Kimi Code | `kimi-coding` | `KIMI_API_KEY` | `https://api.kimi.com/coding` |
| Kimi 国内 | `kimi-coding-cn` | `KIMI_API_KEY` | `https://api.moonshot.cn/v1` |
| MiniMax 国内 | `minimax-cn` | `MINIMAX_CN_API_KEY` | `https://api.minimaxi.com/v1` |
| 智谱 GLM | `zai` | `GLM_API_KEY` | `https://api.z.ai/api/paas/v4` |
| 火山方舟 | `custom` | 自定义 | `https://ark.cn-beijing.volces.com/api/coding` |

---

## 四、重新打开与交互方式

### 命令行重新打开

关闭命令行窗口后，重新打开终端输入：

```bash
# 方式 1：经典 REPL 对话模式（默认）
hermes

# 方式 2：现代 TUI 界面（带侧边栏、会话列表、更美观）
hermes --tui

# 方式 3：恢复上次对话
hermes -c

# 方式 4：恢复指定名称的会话
hermes -c "会话名称"

# 方式 5：单次查询（不进入交互，问完即走）
hermes -z "帮我查下深圳天气"
```

### Web 交互界面

Hermes 自带 Web Dashboard，可以在浏览器里管理配置和聊天：

```bash
# 启动 Web 仪表盘（默认 http://127.0.0.1:9119）
hermes dashboard

# 指定端口
hermes dashboard --port 8080

# 不自动打开浏览器
hermes dashboard --no-open

# 启用内置聊天 Tab（在浏览器里对话）
hermes dashboard --tui
```

Dashboard 功能：
- 查看和编辑配置
- 管理 API Key
- 浏览历史会话
- 启用/禁用工具集
- （加 `--tui` 后）直接在浏览器里和 Hermes 对话

---

## 五、常用命令

### 核心命令

| 命令                            | 说明                           |
| ----------------------------- | ---------------------------- |
| `hermes` 或 `hermes chat`      | 启动对话（经典 REPL）                |
| `hermes --tui`                | 启动现代 TUI 界面                  |
| `hermes -c`                   | 恢复上次会话                       |
| `hermes -z "问题"`              | 单次查询，非交互式                    |
| `hermes model`                | 配置模型（交互式向导）                  |
| `hermes config show`          | 显示当前配置                       |
| `hermes config edit`          | 编辑配置文件                       |
| `hermes config set KEY VALUE` | 设置配置项                        |
| `hermes doctor`               | 运行诊断，检查环境                    |
| `hermes doctor --fix`         | 自动修复可修复的问题                   |
| `hermes status`               | 查看整体状态（含 API Key、平台、Gateway） |
| `hermes --version`            | 查看版本                         |
| `hermes update`               | 升级到最新版本                      |
| `hermes dashboard`            | 启动 Web 仪表盘                   |
| `hermes gateway setup`        | 配置消息网关                       |
| `hermes gateway restart`      | 重启 Gateway 服务                |
| `hermes logs`                 | 查看日志                         |

### 会话内命令（在对话中输入 `/` 开头）

| 命令 | 说明 |
|------|------|
| `/model provider/model-id` | 切换模型（当前会话） |
| `/compress` | 压缩会话上下文 |
| `/usage` | 查看 token 使用情况 |
| `/reload-mcp` | 重载 MCP 配置 |
| `/memory` | 查看记忆状态 |
| `/skills` | 搜索/安装技能 |
| `/tools` | 管理工具集 |
| `/cron` | 管理定时任务 |
| `/title 名称` | 手动命名会话 |
| `/new` 或 `/reset` | 开启新会话 |
| `/quit` | 退出 |

### 工具集管理

```bash
hermes tools list          # 列出所有工具及状态
hermes tools enable NAME   # 启用工具集
hermes tools disable NAME  # 禁用工具集
```

### 技能系统

```bash
hermes skills list              # 列出已安装技能
hermes skills search QUERY    # 搜索技能
hermes skills install ID      # 安装技能
hermes skills update          # 更新技能
```

### 会话管理

```bash
hermes sessions list        # 列出会话
hermes sessions browse      # 交互式选择
hermes sessions export OUT  # 导出为 JSONL
hermes sessions rename ID   # 重命名
hermes sessions delete ID   # 删除
```

### 定时任务

```bash
hermes cron list                    # 列出任务
hermes cron create "0 9 * * *"    # 创建定时任务
hermes cron pause/resume ID         # 暂停/恢复
hermes cron remove ID               # 删除
```

### 用户配对授权（消息平台）

```bash
hermes pairing list                    # 查看待处理请求
hermes pairing approve weixin CODE     # 批准微信配对
hermes pairing approve feishu CODE     # 批准飞书配对
hermes pairing revoke weixin USER_ID   # 撤销授权
```

---

## 六、消息平台接入

### 飞书接入

```bash
# 进入网关配置
hermes gateway setup

# 选择飞书配置方式：
# 1. 自动创建飞书机器人
# 2. 输入已有应用的 AppID 和 AppSecret

# 配置完成后重启 Gateway
hermes gateway restart
```

**用户授权方式**：
- 私聊：推荐 `pairing`（用户发送配对码，管理员执行 `hermes pairing approve`）
- 群聊：可选 `open`（开放）、`allowlist`（白名单）

### 微信接入

```bash
hermes gateway setup
# 选择 Weixin 配置
# 配置 AccountID、Token、BaseURL

hermes gateway restart
```

**用户授权**：
- 私聊策略 `WEIXIN_DM_POLICY=pairing`
- 用户首次对话会收到配对码
- 管理员执行 `hermes pairing approve weixin <CODE>`

### 其他支持平台

- 飞书 ✅
- 微信 ✅
- Telegram
- Discord
- Slack
- WhatsApp
- 钉钉
- QQ

---

## 七、从 OpenClaw 迁移

安装时自动检测 OpenClaw 旧数据，询问是否迁移：

- skill 配置
- 记忆数据
- API Key 配置
- 消息平台配置

**手动迁移命令**：
```bash
hermes claw migrate
```

---

## 八、高频踩坑与解决

### 1. API Key 必须写入 .env，不能用 config set

❌ 错误：
```bash
hermes config set KIMI_API_KEY sk-xxx   # 无效！
```

✅ 正确：
```bash
echo 'KIMI_API_KEY=sk-xxx' >> ~/.hermes/.env
```

### 2. 辅助模型（标题生成等）返回 404

**现象**：消息回复后出现 `Auxiliary title generation failed: HTTP 404`

**原因**：auto-detect 无法找到可用的辅助模型后端

**解决**：显式配置辅助模型参数：
```bash
hermes config set auxiliary.title_generation.provider kimi-coding
hermes config set auxiliary.title_generation.model kimi-k2.6
hermes config set auxiliary.title_generation.base_url https://api.kimi.com/coding/v1
hermes config set auxiliary.title_generation.api_key sk-kimi-xxx
hermes gateway restart
```

### 3. provider 名称导致认证失败

**现象**：`Non-retryable error: Could not resolve authentication method`

**原因**：`kimi-coding-cn` 等 provider 名称与内部认证逻辑不匹配

**解决**：使用 `kimi-coding`（国际）或显式配置辅助模型

### 4. /model 命令只能在会话外使用

❌ 错误：在 Hermes 对话内用 `/model` 添加新 Provider

✅ 正确：退出后用 `hermes model`（终端）重新配置

### 5. 安装后命令找不到

```bash
source ~/.zshrc   # macOS 刷新 PATH
# 或
hash -r
```

### 6. 配置修改后未生效

**Gateway 配置**（消息平台、模型等）修改后必须重启：
```bash
hermes gateway restart
```

**CLI 配置**（工具集、皮肤等）需要新开会话：
```bash
# 按 /quit 退出，重新运行 hermes
```

### 7. WSL2 网关掉线

用 tmux 前台运行代替 systemd：
```bash
tmux new -s hermes
hermes gateway run
```

---

## 九、排错命令速查

```bash
# 诊断所有问题
hermes doctor

# 查看当前配置
hermes config show

# 查看日志（含错误详情）
hermes logs

# 重新配置模型
hermes model

# 重新配置网关
hermes gateway setup

# 重启网关
hermes gateway restart

# 查看 MCP 配置
hermes mcp list

# 重载 MCP
/reload-mcp   # 会话内命令

# 查看 token 使用
/usage        # 会话内命令

# 压缩上下文
/compress     # 会话内命令

# 查看配对请求
hermes pairing list
```

---

## 十、与 OpenClaw 的关系

两者可以**混用**，各有各的位置：

| | OpenClaw | Hermes Agent |
|---|---|---|
| 定位 | 自托管 AI Agent 网关 | 自我成长型 Agent |
| 上手难度 | 简单，Web 可视化 | 命令行，但功能强大 |
| 适合场景 | 多平台消息路由、日常任务自动化 | 长期记忆、自动学习、复杂任务沉淀 |
| 数据迁移 | → | 支持从 OpenClaw 一键迁移 |

**推荐用法**：OpenClaw 负责消息网关和日常任务，Hermes 负责需要深度学习和记忆的复杂任务。

---

## 十一、资源链接

- GitHub：https://github.com/NousResearch/hermes-agent
- 官方文档：https://hermes-agent.nousresearch.com/docs
- 一键安装脚本：https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh
- 国内镜像一键脚本：https://raw.githubusercontent.com/itech001/theaiera/main/scripts/install-cn.sh

---

## Hermes Agent 2026 版本演进

> 本节补充 Hermes Agent 2026-04 最新状态。

### 版本演进时间线

| 日期 | 版本 | 关键变化 |
|------|------|----------|
| 2026-02-25 | v0.1 | Nous Research 首发 |
| 2026-03-15 | v0.4 | 第一个稳定 API |
| 2026-04-08 | **v0.8.0** | 1000+ PRs，多 agent kanban |
| 2026-04-13 | v2026.4.13 | Tool Gateway 引入 |
| 2026-04-16 | **v2026.4.16** | **当前最新**，salvaged Tool Gateway |
| 2026-04-末 | v0.10（"Surface Release"） | 桌面 app、/undo、模糊模型选择 |

> **当前推荐**：`v2026.4.16` 或更新（**不要用 v0.1-v0.4**，API 不稳定）

### 三个里程碑 Release（2026）

#### 1. The Tenacity Release（v0.6-v0.8）
- **durable multi-agent Kanban** — 持久化多 agent 看板
- **persistent /goal** — 长期目标管理
- **Checkpoints v2** — 增强 checkpoint 机制
- **gateway auto-resume** — Gateway 断线自动恢复
- **no_agent cron mode** — 不开 agent 也跑 cron
- **Google Chat 集成** — 新增渠道
- **provider plugins** — Provider 插件化
- **7 个 i18n locales** — 国际化

#### 2. The Velocity Release（v0.8-v0.9）
- **major run_agent.py refactor** — 核心 agent 引擎重写
- **expanded multi-agent Kanban** — Kanban 增强
- **faster cold starts** — 冷启动速度 +50%
- **session_search** — 免费+即时会话搜索
- **promptware defenses** — 提示注入防御
- **Bitwarden Secrets Manager** — Bitwarden 集成
- **新 image generation providers** — 图像生成扩展

#### 3. The Surface Release（v0.10，2026 末）
- **Native 桌面 app** — Tauri/Electron 桌面端
- **浏览器 admin panel** — Web 管理面板
- **远程 gateway 连接** — 客户端-服务器模式
- **简体中文桌面 UI** — 中文本地化
- **更轻默认 skills** — 默认 skill 精简
- **NVIDIA 信任 skills** — 官方合作 skills
- **模糊模型选择器** — 自然语言选模型
- **/undo** — 操作撤销

### 关键新能力

#### 1. GEPA 集成（Genetic-Pareto）

Hermes 集成了 **UC Berkeley + Stanford + MIT + Databricks** 联合开发的 **GEPA** prompt 优化框架：

- 走 `hermes-agent-self-evolution` 副仓库
- DSPy + GEPA 联合优化 skill
- 自动用 Genetic-Pareto 算法找出最优 prompt

**效果**：用一段时间后，Hermes 的 skill 会自动变得更好用（自我进化）。

#### 2. Hermes Workspace（第三方 GUI）

`hermes-workspace`（500+ stars，2026-Q2）— 第三方 Web GUI：

- Chat 对话
- Terminal 终端
- Memory browser 记忆浏览
- Skills manager 技能管理

> 跟 Hermes 官方 CLI 互补，不冲突。

#### 3. Awesome Hermes Agent

`awesome-hermes-agent` 社区资源列表：https://github.com/0xNyk/awesome-hermes-agent

### 数据快照（2026-04-末）

| 指标 | 数值 |
|------|------|
| **GitHub stars** | **43,700** |
| **发布天数** | < 60 天 |
| **Stars/天** | ~720（顶级） |
| **License** | MIT |
| **Merged PRs** | 1,000+ |
| **贡献者** | 100+ |

### 升级路径

```bash
# 升级到 v2026.4.16
hermes update

# 或重新装
curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash

# 国内镜像（不变）
curl -fsSL https://raw.githubusercontent.com/itech001/theaiera/main/scripts/install-cn.sh | bash
```

### 与 OpenClaw 的关系（2026 更新）

| 维度 | OpenClaw | Hermes Agent |
|------|----------|--------------|
| 定位 | 自托管 AI 网关 | 自我成长型 agent |
| 最新版本 | 2026.6.8 | v2026.4.16 |
| GitHub stars | 内部 | 43.7K（外部） |
| 上手 | 简单（Web 可视化） | 命令行 |
| 学习闭环 | ❌（人工 promote） | ✅（GEPA 自动） |
| 多 agent | ✅（sessions_spawn） | ✅（Kanban） |
| 数据迁移 | → Hermes | 支持从 OpenClaw 一键迁移 |

**2026 推荐用法**（在原推荐基础上更新）：

- **OpenClaw** 跑消息入口 + 日常 skill 调用
- **Hermes** 跑需要深度学习/自我进化的项目
- **两者可同时跑**（数据不冲突）

### 待观察

- v0.10 Surface Release 桌面 app 稳定性
- GEPA 自动优化的实际收益
- 是否会有 Hermes Pro / Cloud 商业版

---

_原 Hermes 知识库 + 2026-06-20 增量（v2026.4.16 最新版 + 三个里程碑 release + GEPA 集成）_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 更新到 v2026.4.16 + 三个里程碑 release + GEPA 集成 |

---

## 📚 相关文档

- [[OpenClaw 知识库]] — 与 Hermes 互补的网关
- [[AI 编程工具全景图]] — Hermes 在 7 工具中的位置
- [[Claude Code 知识库]] — Hermes 可调度的编程 agent
- [[Skill 系统对比]] — Hermes Skills vs 其他 3 套

_Hermes 是 7 工具里最"自我成长"的，靠 GEPA 持续优化 skill。_

---
path: Skill 系统对比.md
workspace: ~/Documents/obsidian/ai-study/
category: comparison
tags: [skills, openclaw, claude-code, ecc, hermes]
created: 2026-06-20
updated: 2026-06-20
---

# Skill 系统对比

> 把 OpenClaw Skills、Claude Code Skills、ECC Skills、Hermes Skills 四套并存系统放到一起对比,说清"这四套的关系、区别、什么时候用谁"。

**更新日期**:2026-06-20

---

## 一句话对比

| 系统 | 存储位置 | 触发方式 | 谁在用 |
|------|----------|----------|--------|
| **OpenClaw Skills** | `~/.openclaw/skills/` + `~/.npm-global/lib/node_modules/openclaw/skills/` | `openclaw skills` 命令 | OpenClaw + 所有 harness |
| **Claude Code Skills** | `~/.claude/skills/` + `/plugin install` | `/skill` 命令 | Claude Code 原生 |
| **ECC Skills** | 装 ECC 时自动注入 271 个 | 跟 Claude Code 触发 | 跨 harness 统一 |
| **Hermes Skills** | `~/.hermes/skills/` | `/skills` 命令 | Hermes Agent |

---

## 文件结构对比

### OpenClaw Skill

```
~/.openclaw/skills/
└── my-skill/
    ├── SKILL.md           # 必需:技能描述
    ├── scripts/            # 可选:辅助脚本
    │   └── helper.py
    └── references/         # 可选:参考资料
        └── api.md
```

`SKILL.md` 范例:
```markdown
---
name: my-skill
description: 简述何时触发,关键词:"关键词1", "关键词2"
---

# 做什么
[具体指令]
```

### Claude Code Skill

```markdown
---
name: my-skill
description: ...
allowed-tools: Read, Grep
---

# 指令
```

> 结构跟 OpenClaw 几乎一样。**可以互相拷贝**(改改 description)。

### ECC Skill

跟 OpenClaw/Claude Code 同结构,但在安装 ECC 时一次性注入 271 个到:
- `~/.claude/skills/`(Claude Code)
- `~/.opencode/skills/`(OpenCode)
- Cursor/Codex 等对应目录

### Hermes Skill

```bash
~/.hermes/skills/
└── my-skill/
    └── SKILL.md
```

类似结构,但 Hermes 多了 `learning_status.md` 记录 skill 的"学习进度"。

---

## 安装方式

| 系统 | 命令 |
|------|------|
| OpenClaw | `openclaw skills install <name>` |
| Claude Code | `/plugin install <plugin>` (Skills 通过 plugin 分发) |
| ECC | 自动,装 ECC 就有 271 个 |
| Hermes | `hermes skills install <id>` |

---

## 数量级(2026-06)

| 系统 | 数量 | 来源 |
|------|------|------|
| OpenClaw | 76 个本地(47 ready) | ClawHub + 本地 |
| Claude Code 官方 | 10+ | Anthropic 官方 |
| ECC | 271 | affaan-m/everything-claude-code |
| Hermes | 看用户安装 | 社区 |

---

## 实战:同一个 Skill 三处共享

假设你写了一个「PostgreSQL 调优」skill,要让 4 个系统都能用:

```bash
# 1. 写一次
mkdir -p ~/.openclaw/skills/postgres-tune
cat > ~/.openclaw/skills/postgres-tune/SKILL.md << 'EOF'
---
name: postgres-tune
description: PostgreSQL 性能调优,关键词:"慢查询", "调优", "EXPLAIN"
---

# 做什么
1. 先 EXPLAIN ANALYZE
2. 找 Seq Scan 改 Index Scan
3. 推荐索引时给完整 DDL
4. 任何 ALTER 都要先备份
EOF

# 2. 软链到 Claude Code
ln -sf ~/.openclaw/skills/postgres-tune ~/.claude/skills/postgres-tune

# 3. 软链到 Hermes
ln -sf ~/.openclaw/skills/postgres-tune ~/.hermes/skills/postgres-tune

# 4. 装到 ECC(作为 rule 或继承)
cp -r ~/.openclaw/skills/postgres-tune ~/.claude/skills/postgres-tune-ecc
```

> 4 个 harness 同时拥有同一个 skill,改一处全更新。

---

## 跨 harness 兼容技巧

### 1. 通用 description 写法

```markdown
---
description: 触发条件(不依赖特定工具)
---

# 写跨平台指令,不用 Bash 假设
```

避免:
- ❌ "调用 openclaw tools"  (OpenClaw 专用)
- ✅ "用 shell 命令" (通用)

### 2. 工具权限写法

| 系统 | 语法 |
|------|------|
| OpenClaw | `permissions.allow: ["Bash(psql:*)"]` |
| Claude Code | `allowed-tools: Bash(psql:*)` |
| ECC | `tools: ["bash"]` (在 agent 配置里) |
| Hermes | `hermes tools enable postgres` |

写 skill 时只描述"做什么",不绑死工具名。

### 3. 元数据格式差异

| 系统 | metadata 位置 |
|------|---------------|
| OpenClaw | YAML frontmatter |
| Claude Code | YAML frontmatter |
| ECC | YAML frontmatter |
| Hermes | YAML + `learning_status.md` |

> 99% 兼容,只有 Hermes 多一个状态文件。

---

## 推荐策略

### 典型使用场景

- 76 个 OpenClaw skill(47 ready)
- 0 个 Claude Code skill(还没用 marketplace)
- 装 ECC 后会注入 271 个
- Hermes skill 还没单独装

### 实战建议

1. **OpenClaw skill 留作主仓**:`~/.openclaw/skills/` 是 single source of truth
2. **Claude Code 走软链** `ln -sf` 到 `~/.claude/skills/`
3. **ECC 装核心包** 用 ECC 自带的 271 个,自己不写
4. **Hermes skill 慎用** Hermes 有自己的学习系统,跟其他容易冲突

### 不要做的事

- ❌ 每个 harness 都装同一份独立 skill(4 份副本,改一处忘改其他)
- ❌ 在 skill 里 hardcode `/Users/<user>/...` 路径(换机器坏)
- ❌ 在 skill 里 hardcode 工具名(不能跨 harness)

---

## 调试常见问题

| 问题 | 原因 | 解决 |
|------|------|------|
| skill 没触发 | description 没关键词 | 加 trigger 词 |
| skill 触发但报错 | 工具权限不够 | 加 `allowed-tools` |
| skill 在 harness A 跑,harness B 不跑 | 目录没软链 | `ln -sf` |
| 改了 skill 不生效 | harness 缓存 | 重启 harness |

---

## 相关文档

- [[OpenClaw 知识库]] — `openclaw skills` 命令详解
- [[Claude Code 知识库]] — Claude Code skill/plugin 体系
- [[ECC 知识库]] — ECC 271 个 skill
- [[Hermes 知识库]] — Hermes 自我学习
- [[Superpowers 知识库]] — Superpowers 的 7 阶段 skill 套件
- [[AI 编程工具全景图]] — 7 工具整体关系

---

_新建于 2026-06-20,补"Skill 系统对比"框架性缺失_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 首次结构化（frontmatter + Obsidian 双向链接） |

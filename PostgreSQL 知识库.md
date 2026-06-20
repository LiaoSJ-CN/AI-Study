---
path: PostgreSQL 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: data
tags: [postgresql, oltp, pgvector, sql]
created: 2026-06-20
updated: 2026-06-20
---

# PostgreSQL知识库

> 更新时间：2026-04-06
> 适用：macOS (Apple Silicon / Intel)

---

## 安装方式对比

| 方式 | 优点 | 缺点 | 推荐场景 |
|------|------|------|----------|
| **Homebrew** | 版本灵活、可多版本共存、与系统集成好 | 需熟悉命令行 | **开发者首选，推荐** |
| **Postgres.app** | 安装极简、GUI 操作 | 功能有限、不支持多版本 | 轻度使用 |
| **EDB 安装包** | 带图形管理工具 | 版本固定、升级麻烦 | 需要 pgAdmin |
| **MacPorts** | 隔离性强 | 社区较小 | 特殊需求 |

**推荐：Homebrew**，社区活跃、版本管理灵活、开发环境最常用。

---

## 方式一：Homebrew 安装（推荐）

### 1. 检查 Homebrew

```bash
brew --version
```

未安装的话，先装 Homebrew：
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### 2. 安装 PostgreSQL

```bash
brew update
brew doctor          # 检查环境，可选
brew install postgresql@16    # 可改成 @15 / @14 指定版本
```

> macOS Sonoma 自带 PostgreSQL 14，无需额外安装，可通过以下命令确认：
> `which psql` 或 `psql --version`

### 3. 启动服务

```bash
# 启动（开机自启）
brew services start postgresql@16

# 启动（手动，不自启）
brew services start postgresql@16 --no-autostart

# 检查状态
brew services list
```

### 4. 初始化数据库

首次安装需要创建数据库集群：

```bash
# PostgreSQL 14+（macOS 内置）
initdb /usr/local/var/postgres@16 -E utf8

# 或使用数据目录
initdb /opt/homebrew/var/postgresql@16 -E utf8
```

### 5. 创建数据库和用户

```bash
# 以 postgres 用户身份操作（类 Unix 系统）
psql postgres

# 在 psql 命令行里：
CREATE DATABASE mydb;           -- 创建数据库
CREATE USER myuser WITH PASSWORD 'password123';  -- 创建用户
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser; -- 授权
\q                              -- 退出
```

### 6. 连接数据库

```bash
# 默认连接本机
psql -U myuser -d mydb -h localhost

# 或通过 socket 连接
psql mydb
```

### 7. 常用管理命令

```bash
brew services stop postgresql@16   # 停止
brew services restart postgresql@16 # 重启
brew services list                 # 查看状态
brew uninstall postgresql@16       # 卸载
```

### 8. 多版本共存

```bash
brew install postgresql@14
brew install postgresql@16

# 不同端口：
# @14 端口 5432
# @16 端口 5433（自动分配）

# 手动指定端口启动
pg_ctl -D /opt/homebrew/var/postgresql@14 start -o "-p 5433"
```

---

## 方式二：Postgres.app（极简）

### 1. 下载

官网下载：https://postgresapp.com/

### 2. 安装

双击 `.dmg` → 拖拽 `Postgres.app` 到 Applications 文件夹

### 3. 启动

打开 Postgres.app，状态栏显示绿色即运行中

### 4. 连接

点击"Open psql"，Terminal 自动打开并连接

---

## 配置优化（Homebrew）

### 配置文件位置

```bash
/opt/homebrew/var/postgresql@16/postgresql.conf  # Homebrew 安装
/usr/local/var/postgres/postgresql.conf           # 系统自带
```

### 常用参数调整

```conf
# 最大连接数
max_connections = 100

# 共享内存（macOS 推荐较小值）
shared_buffers = 128MB

# WAL 写入模式（macOS 推荐）
wal_level = minimal
fsync = off          # 开发环境可关闭提升性能，生产环境勿动
synchronous_commit = off  # 同上

# 查询超时
statement_timeout = 30s
```

修改后重启生效：
```bash
brew services restart postgresql@16
```

---

## 客户端工具推荐

| 工具 | 类型 | 特点 |
|------|------|------|
| **pgcli** | CLI | 自动补全、语法高亮，比 psql 更友好 |
| **DBeaver** | GUI | 免费、全平台、支持所有主流数据库 |
| **TablePlus** | GUI | 轻量、快速、界面好看（付费） |
| **DataGrip** | GUI | JetBrains 出品，功能强大（付费） |

安装 pgcli：
```bash
brew install pgcli
pgcli -U myuser -d mydb
```

---

## 常见问题

### Q: psql: error: connection to server on socket "/tmp/.s.PGSQL.5432" failed?
PostgreSQL 服务未启动，运行 `brew services start postgresql@16`

### Q: FATAL: role "xxx" does not exist?
用户不存在，用 postgres 超级用户创建：
```bash
psql postgres
CREATE ROLE myuser LOGIN PASSWORD 'password123';
```

### Q: 如何修改 postgres 用户密码？
```bash
psql postgres
ALTER USER postgres WITH PASSWORD 'newpassword';
```

### Q: Apple Silicon 安装后找不到路径？
Homebrew 在 Apple Silicon Mac 上路径为 `/opt/homebrew/`，注意区分：
```bash
/opt/homebrew/bin/psql      # Apple Silicon
/usr/local/bin/psql         # Intel Mac
```

---

## 快速参考

```bash
# 一行安装
brew install postgresql@16

# 一行启动
brew services start postgresql@16

# 一行连接
psql postgres
```

---

_来源：[Tiger Data](https://www.tigerdata.com/learn/how-to-install-postgresql-on-macos)、[Atlassian](https://www.atlassian.com/data/admin/how-to-start-postgresql-server-on-mac-os-x)_

---

## PostgreSQL 17/18 新特性与升级

> 本节补充 PostgreSQL 17/18 关键变化（基于 @16 安装文档扩展）。

### 版本时间线

| 版本 | 发布时间 | 状态(2026-06) |
|------|----------|----------------|
| 16 | 2023-09 | 已落后一个版本 |
| 17 | 2024-09 | 上一代主流 |
| **18** | 2025-09 | **当前主流推荐** |
| 19 | 2026 H2 | 开发中 |

### 升级到 18(推荐生产环境)

```bash
# Homebrew 升级路径
brew services stop postgresql@16
brew install postgresql@18

# 1. 数据迁移(用 pg_upgrade,停机 < 5 分钟)
chown -R postgres:postgres /opt/homebrew/var/postgresql@18
su postgres -c "/opt/homebrew/opt/postgresql@18/bin/pg_upgrade \
  -b /opt/homebrew/opt/postgresql@16/bin \
  -B /opt/homebrew/opt/postgresql@18/bin \
  -d /opt/homebrew/var/postgresql@16 \
  -D /opt/homebrew/var/postgresql@18"

# 2. 启动
brew services start postgresql@18
```

### PG 18 关键新特性

| 特性 | 价值 |
|------|------|
| **逻辑复制改进** | 双向复制 + 冲突检测,异地多活更容易 |
| **`json_table` 增强** | JSON 当表查,性能提升 3x |
| **B-tree 索引去重优化** | 索引大小减少 30-50% |
| **分区表并发优化** | 多核扩展性提升 |
| **pgvector 0.8 集成** | 向量搜索性能 + HNSW 优化 |
| **异步 I/O 改进** | 顺序扫描速度 +30% |

### PG 17 关键新特性(2024-09)

| 特性 | 价值 |
|------|------|
| **逻辑订阅 DDL 复制** | 不用手工 apply schema 变更 |
| **`MERGE` 语句完善** | UPSERT 不再需要 CTE 模拟 |
| **`jsonb` 表达式索引** | JSON 字段也能 B-tree |
| **流式 I/O 接口** | pg_dump 备份快 2x |
| **vacuum 内存自动调节** | 减少人工调参 |

### 关键扩展(2026)

| 扩展 | 用途 |
|------|------|
| **pgvector 0.8** | 向量搜索(AI 必备) |
| **PostGIS 3.5** | 地理空间 |
| **pg_stat_statements** | 慢查询分析 |
| **pg_cron** | 定时任务 |
| **pg_partman** | 自动分区管理 |
| **pgBackRest** | 备份恢复(比 pg_dump 强) |

```bash
# 装 pgvector(2026 AI 项目必备)
brew install pgvector

# 启用
psql mydb -c "CREATE EXTENSION vector;"

# 实战
CREATE TABLE docs (
  id SERIAL PRIMARY KEY,
  content TEXT,
  embedding vector(1536)
);

CREATE INDEX ON docs USING hnsw (embedding vector_cosine_ops);
-- HNSW 索引 + cosine 距离,亿级向量毫秒级检索
```

### 性能调优(2026 实战)

```conf
# 18 推荐配置(macOS M 系列)
max_connections = 100
shared_buffers = 4GB                    # 物理内存的 25%
effective_cache_size = 12GB              # 物理内存的 75%
work_mem = 64MB
maintenance_work_mem = 1GB
wal_buffers = 64MB

# 异步 I/O(vacuum/analyze 加速)
maintenance_io_concurrency = 10
effective_io_concurrency = 200

# pgvector 优化(超过 100 万向量)
hnsw.ef_search = 100
hnsw.max_connections = 16
```

### 推荐备份方案(2026)

```bash
# pgBackRest(企业级)
brew install pgbackrest

# 配置
# /etc/pgbackrest.conf
[global]
repo1-path=/var/lib/pgbackrest
repo1-retention-full=7

[mydb]
pg1-path=/opt/homebrew/var/postgresql@18

# 全量备份
pgbackrest backup --type=full --stanza=mydb

# 恢复
pgbackrest restore --stanza=mydb --type=time --target="2026-06-20 12:00:00"
```

---

_原 PostgreSQL 安装文档 2026-04-06 + 2026-06-20 增量(17/18 升级 + pgvector 实战)_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 补充 PostgreSQL 17/18 新特性与升级 |

---

## 📚 相关文档

- [[Apache Doris 知识库]] — OLAP 场景对比
- [[NL2SQL 知识库]] — 让 AI 写 SQL 查 PG
- [[AI 数据智能开源项目 知识库]] — Vanna/Chat2DB 等 PG 集成
- [[OpenClaw 知识库]] — OpenClaw 可调 PG MCP server

_PG 适合 OLTP/事务，pgvector 0.8 是 AI 时代必备。_

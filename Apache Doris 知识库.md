---
path: Apache Doris 知识库.md
workspace: ~/Documents/lsj-Obsidian/AI学习/
category: data
tags: [doris, olap, lakehouse, iceberg]
created: 2026-06-20
updated: 2026-06-20
---

# Apache Doris macOS (Mac mini) 安装指南

> **⚠️ 重要提示**：Doris 官方推荐在 Linux 环境运行。macOS 仅适合本地开发测试，生产环境请使用 Ubuntu/CentOS。

---

## 方法一：Docker 快速部署（推荐）

从 Doris 2.1.8 开始支持 Docker 一键部署，最简单快捷。

### 前置条件

1. **安装 Docker Desktop**
   ```bash
   # 下载地址：https://www.docker.com/products/docker-desktop/
   # 或使用 Homebrew
   brew install --cask docker
   ```

2. **解决 Mac Docker 路径问题**
   ```bash
   sudo ln -s /Applications/Docker.app/Contents/Resources/bin/docker /usr/local/bin/docker
   ```

3. **启动 Docker Desktop**
   打开 Docker Desktop 应用，等待状态栏图标显示 "Engine running"

### 安装步骤

#### Step 1: 下载启动脚本

```bash
# 创建 Doris 工作目录
mkdir -p ~/doris && cd ~/doris

# 下载官方启动脚本
curl -O https://doris.apache.org/files/start-doris.sh

# 赋予执行权限
chmod 755 start-doris.sh
```

#### Step 2: 启动 Doris 集群

```bash
# 启动 Doris 3.0.8 版本
bash start-doris.sh -v 3.0.8
```

> 首次启动会拉取镜像，耗时约 5-10 分钟

#### Step 3: 验证安装

```bash
# 检查 FE（前端）状态
mysql -uroot -P9030 -h127.0.0.1 -e 'SELECT `host`, `join`, `alive` FROM frontends()'

# 预期输出：
# +-----------+------+-------+
# | host      | join | alive |
# +-----------+------+-------+
# | 127.0.0.1 | true | true  |
# +-----------+------+-------+

# 检查 BE（后端）状态
mysql -uroot -P9030 -h127.0.0.1 -e 'SELECT `host`, `alive` FROM backends()'

# 预期输出：
# +-----------+-------+
# | host      | alive |
# +-----------+-------+
# | 127.0.0.1 | 1     |
# +-----------+-------+
```

#### Step 4: 连接测试

```bash
# 使用 MySQL 客户端连接
mysql -uroot -P9030 -h127.0.0.1

# 创建测试数据库
CREATE DATABASE demo;
USE demo;

# 创建测试表
CREATE TABLE mytable(
    k1 TINYINT,
    k2 DECIMAL(10, 2) DEFAULT "10.05",
    k3 CHAR(10) COMMENT "string column",
    k4 INT NOT NULL DEFAULT "1" COMMENT "int column"
) COMMENT "my first table"
DISTRIBUTED BY HASH(k1) BUCKETS 1
PROPERTIES ("replication_num" = "1");

# 插入测试数据
INSERT INTO mytable VALUES
(1, 0.14, 'a1', 20),
(2, 1.04, 'b2', 21),
(3, 3.14, 'c3', 22),
(4, 4.35, 'd4', 23);

# 查询数据
SELECT * FROM mytable;
```

---

## 方法二：二进制包手动安装

适用于需要自定义配置的场景。

### 前置条件

- **Java 17**（必须）
- **MySQL 客户端**（用于连接 Doris）

```bash
# 安装 Java 17
brew install openjdk@17

# 添加到 PATH
echo 'export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 验证
java -version

# 安装 MySQL 客户端
brew install mysql-client
```

### 安装步骤

#### Step 1: 下载 Doris 二进制包

```bash
# 创建工作目录
mkdir -p ~/doris && cd ~/doris

# 下载最新稳定版（以 3.0.8 为例）
# ARM64 (Apple Silicon M1/M2/M3)
curl -LO https://apache-doris-releases.oss-accelerate.aliyuncs.com/apache-doris-3.0.8-bin-arm64.tar.xz

# 或 x86_64 (Intel)
# curl -LO https://apache-doris-releases.oss-accelerate.aliyuncs.com/apache-doris-3.0.8-bin-x64.tar.xz

# 解压
tar -xvf apache-doris-3.0.8-bin-arm64.tar.xz

# 重命名方便操作
mv apache-doris-3.0.8-bin-arm64 apache-doris
```

#### Step 2: 系统环境配置

```bash
# 修改文件描述符限制
echo '* soft nofile 1000000' | sudo tee -a /etc/security/limits.conf
echo '* hard nofile 1000000' | sudo tee -a /etc/security/limits.conf

# 修改虚拟内存区域
echo 'vm.max_map_count=2000000' | sudo tee -a /etc/sysctl.conf
sudo sysctl -w vm.max_map_count=2000000
```

#### Step 3: 配置并启动 FE（Frontend）

```bash
cd ~/doris/apache-doris

# 编辑 FE 配置文件
vim fe/conf/fe.conf

# 添加以下内容：
JAVA_HOME=/opt/homebrew/opt/openjdk@17
priority_networks=127.0.0.1/32
```

```bash
# 启动 FE
fe/bin/start_fe.sh --daemon

# 查看日志确认启动成功
tail -f fe/log/fe.log
```

#### Step 4: 配置并启动 BE（Backend）

```bash
# 编辑 BE 配置文件
vim be/conf/be.conf

# 添加以下内容：
JAVA_HOME=/opt/homebrew/opt/openjdk@17
priority_networks=127.0.0.1/32
```

```bash
# 启动 BE
be/bin/start_be.sh --daemon

# 查看日志
tail -f be/log/be.log
```

#### Step 5: 注册 BE 到集群

```bash
# 连接 Doris
mysql -uroot -P9030 -h127.0.0.1

# 在 MySQL 客户端中执行：
ALTER SYSTEM ADD BACKEND "127.0.0.1:9050";

# 退出后验证 BE 状态
mysql -uroot -P9030 -h127.0.0.1 -e "SHOW BACKENDS;"
```

---

## 常用管理命令

```bash
# 停止 Doris
~/doris/apache-doris/fe/bin/stop_fe.sh
~/doris/apache-doris/be/bin/stop_be.sh

# 重启 Doris
~/doris/apache-doris/fe/bin/stop_fe.sh && ~/doris/apache-doris/fe/bin/start_fe.sh --daemon
~/doris/apache-doris/be/bin/stop_be.sh && ~/doris/apache-doris/be/bin/start_be.sh --daemon

# 查看进程
ps aux | grep doris

# 查看端口占用
lsof -i :9030  # FE 查询端口
lsof -i :8030  # FE HTTP 端口
lsof -i :9060  # BE 端口
```

---

## Web UI 访问

Doris 提供 Web 管理界面：

- **地址**：http://127.0.0.1:8030
- **默认用户**：root（无密码）

---

## 常见问题

### Q1: Docker 提示 "Error: Docker environment not detected"

```bash
# 创建软链接
sudo ln -s /Applications/Docker.app/Contents/Resources/bin/docker /usr/local/bin/docker
```

### Q2: 报错 "error getting credentials"

```bash
# 编辑 Docker 配置
vim ~/.docker/config.json

# 删除 credsStore 字段（仅开发环境）
```

### Q3: FE 启动失败，日志显示端口被占用

```bash
# 查找占用 9030 端口的进程
lsof -i :9030

# 杀掉进程后重新启动
kill -9 <PID>
```

### Q4: BE 启动后 Alive 状态为 false

```bash
# 检查 BE 日志
tail -f ~/doris/apache-doris/be/log/be.WARNING

# 常见原因：
# 1. 虚拟内存不足 - 执行 sudo sysctl -w vm.max_map_count=2000000
# 2. 端口冲突 - 检查 9050/9060/8040/8060 端口
# 3. FE 未启动 - 先确保 FE 正常运行
```

### Q5: Mac M1/M2/M3 芯片兼容性问题

Doris 3.0+ 已原生支持 ARM64，但建议使用 Docker 方式部署以避免依赖问题。

---

## 参考资源

- [Apache Doris 官方文档](https://doris.apache.org/docs/3.x/gettingStarted/quick-start/)
- [Doris GitHub](https://github.com/apache/doris)
- [Docker Desktop for Mac](https://www.docker.com/products/docker-desktop/)

---

*文档生成时间：2026-03-05*

---

## Doris 3.x/4.x 版本演进与新特性

> 本节补充 Doris 3.x/4.x 的关键变化（基于 3.0.8 安装文档扩展）。

### 4.1 重大版本演进

| 版本 | 发布时间 | 核心特性 |
|------|----------|----------|
| 3.0 | 2024-04 | 引入 **compute-storage decoupled** 模式,云原生架构 |
| 3.1 | 2024-12 | 异步物化视图增强,湖仓一体能力扩展 |
| 3.2 | 2025-06 | 优化器重写,查询性能提升 2-3x |
| **4.0** | 2026-02 | 多模态数据湖原生支持 |
| **4.1** | 2026-04 | 完整支持 **Iceberg V3** 湖仓生命周期 |

> ⚠️ 3.0.8 安装步骤仍然有效,但建议**生产环境直接装 4.1**(2026-06 之前最稳的版本是 4.0.2)。

### 4.x 关键新概念

#### 1. Compute-Storage Decoupled(计算存储分离)

3.0 引入的云原生架构,4.x 已成熟:

```
传统 coupled 模式(3.0 前):
  ┌──────────┐
  │ FE + BE  │  紧耦合
  │ + 本地盘 │
  └──────────┘

Decoupled 模式(3.0+/4.x):
  ┌──────────┐     ┌──────────────┐
  │ Compute  │ ──→ │ Object Store │
  │ Cluster  │     │ (S3/OSS/HDFS)│
  └──────────┘     └──────────────┘
  ↓ 多个 cluster 并存,物理隔离
```

**优势**:
- 多个计算集群读同一份存储,物理隔离
- 读写负载分离
- 弹性扩缩容
- 成本优化(冷热分层)

#### 2. Lakehouse 架构(4.x 重点)

Doris 4.x = **数据湖 + 数据仓库** 统一引擎:

| 能力 | 说明 |
|------|------|
| **湖格式读写** | 原生支持 Iceberg V3、Paimon、Hudi、Delta Lake |
| **数据湖写入** | 4.0 起,可以把数据写回数据湖(以前只能读) |
| **统一 SQL** | 一套 SQL 同时查湖和仓,无需数据搬迁 |
| **Catalog 集成** | Hive Metastore / AWS Glue / Unity Catalog |
| **异步物化视图** | 跨湖和仓的物化视图,自动刷新 |

#### 3. Iceberg V3 完整支持(4.1)

NetEase 等大厂已用 Doris 4.1 跑完整 Iceberg V3 湖仓生命周期:
- 实时摄取 → 增量 ETL → 物化视图 → 数据服务,一站式

### 安装建议(2026-06 更新)

**生产环境**:
```bash
# 推荐 4.0.2(最稳)
bash start-doris.sh -v 4.0.2

# 想用 Iceberg V3 完整支持:4.1+
bash start-doris.sh -v 4.1.0
```

**Lakehouse 场景**:在 `fe.conf` 加:
```conf
# 启用 S3 catalog
s3_endpoint = https://s3.amazonaws.com
s3_region = us-east-1
s3_ak = YOUR_ACCESS_KEY
s3_sk = YOUR_SECRET_KEY

# 启用 Hive Metastore
hive_metastore_url = thrift://hms-host:9083
```

**macOS 开发**:跟 3.x 一致,优先 Docker。

### 常用 4.x 命令

```sql
-- 查 Iceberg 表(4.x 原生)
SELECT * FROM iceberg_catalog.db.table;

-- 写回数据湖(4.0+ 新能力)
INSERT INTO iceberg_catalog.db.target_table
SELECT * FROM internal_db.source_table;

-- 异步物化视图(3.1+)
CREATE MATERIALIZED VIEW mv_sales
REFRESH ASYNC EVERY (INTERVAL 1 HOUR)
AS SELECT region, SUM(amount) FROM sales GROUP BY region;
```

### 迁移 3.x → 4.x

```bash
# 1. 备份 3.x 元数据
mysqldump -uroot -P9030 -h127.0.0.1 doris_meta > meta_backup.sql

# 2. 停 3.x,起 4.0(原地升级)
fe/bin/stop_fe.sh
be/bin/stop_be.sh
# 替换二进制包
bash start-doris.sh -v 4.0.2

# 3. 验证
mysql -uroot -P9030 -h127.0.0.1 -e "SHOW BACKENDS;"
```

> 4.0 向后兼容 3.x 数据格式,通常不用 export/import。

### 参考资源(2026)

- 4.1 Iceberg V3 实战:https://doris.apache.org/blog/doris-4.1-iceberg-v3-lakehouse
- Lakehouse 概览:https://doris.apache.org/docs/dev/lakehouse/lakehouse-overview
- VeloDB NetEase 案例:https://doris.apache.org/blog/netease-unified-lakehouse

---

_Doris 安装文档 2026-03-05 原文 + 2026-06-20 增量更新_

---

## 📝 更新记录

| 日期 | 内容 |
|------|------|
| 2026-06-20 | 补充 Doris 3.x/4.x 版本演进与新特性 |

---

## 📚 相关文档

- [[PostgreSQL 知识库]] — 同类 OLTP 数据库对比
- [[NL2SQL 知识库]] — 让 AI 自动写 SQL 查询 Doris
- [[AI 数据智能开源项目 知识库]] — 包含 DB-GPT、Chat2DB 等 Doris 集成工具
- [[OpenClaw 知识库]] — 用 OpenClaw 调度 Doris 查询

_Doris 适合 OLAP 场景，跟 PG 互补（OLTP）。_

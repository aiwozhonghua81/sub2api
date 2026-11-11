<!-- 中文说明开始 -->
# 📖 中文详细说明（sub2api）

<!-- 徽章区开始 -->
<p align="center">
  <a href="https://github.com/aiwozhonghua81/sub2api"><img alt="GitHub" src="https://img.shields.io/badge/GitHub-aiwozhonghua81-181717?style=flat-square&logo=github&logoColor=white"></a>
  <img alt="最近提交" src="https://img.shields.io/github/last-commit/aiwozhonghua81/sub2api?style=flat-square&label=%E6%9C%80%E8%BF%91%E6%8F%90%E4%BA%A4">
  <img alt="主语言" src="https://img.shields.io/github/languages/top/aiwozhonghua81/sub2api?style=flat-square&label=%E4%B8%BB%E8%AF%AD%E8%A8%80">
  <img alt="星标" src="https://img.shields.io/github/stars/aiwozhonghua81/sub2api?style=flat-square&label=%E6%98%9F%E6%A0%87">
  <img alt="仓库体积" src="https://img.shields.io/github/repo-size/aiwozhonghua81/sub2api?style=flat-square&label=%E4%BB%93%E5%BA%93%E4%BD%93%E7%A7%AF">
  <img alt="许可证" src="https://img.shields.io/github/license/aiwozhonghua81/sub2api?style=flat-square&label=%E8%AE%B8%E5%8F%AF%E8%AF%81">
</p>
<p align="center">
  <img alt="中文说明" src="https://img.shields.io/badge/%E4%B8%AD%E6%96%87%E8%AF%B4%E6%98%8E-%E5%B7%B2%E5%AE%8C%E6%95%B4%E8%A6%86%E7%9B%96-brightgreen?style=flat-square">
  <img alt="给新手看" src="https://img.shields.io/badge/%E7%BB%99%E6%96%B0%E6%89%8B%E7%9C%8B-%E4%B8%8D%E7%94%A8%E6%87%82%E4%BB%A3%E7%A0%81-blue?style=flat-square">
  <img alt="配图" src="https://img.shields.io/badge/%E9%85%8D%E5%9B%BE-%E5%AE%9E%E6%8B%8D%E6%88%AA%E5%9B%BE%E4%B8%8E%E5%A4%B4%E5%9B%BE-ff69b4?style=flat-square">
</p>
<p align="center">
  <img src="assets/logo-icon.png" alt="sub2api 头图" width="880">
</p>
<!-- 徽章区结束 -->

> **阅读顺序说明**：本文件最上面这一大段是**中文详细说明**，用大白话写给第一次接触这个项目的人；下面从 `---` 之后开始，是**上游项目原封不动的原始 README**，一字未删、未改写。
>
> 只想看原项目的官方说明，可以直接跳到下半部分。

---

## 1️⃣ 这是什么？

一句话说清楚：**一个"AI API 收费站"——把各家 AI 订阅（ChatGPT / Claude / DeepSeek 等）的额度集中起来，变成一张张 API Key 发给用户用，同时精确记账、限流、调度。**

先解释几个名词：

| 名词 | 大白话解释 |
|------|-----------|
| **AI API 网关** | 站在用户和上游 AI 之间的一层"翻译 + 调度 + 记账"的服务 |
| **上游账号** | 你自己持有的 ChatGPT / Claude / DeepSeek 订阅账号（通过 OAuth 或 API Key 接入） |
| **API Key** | 网关发给下游用户的访问凭证，用户拿它去调用 AI |
| **分组（Group）** | 一组账号的集合 + 一组价格规则。用户可以按分组拿不同类型的 Key |
| **粘性会话** | 同一个用户的连续对话尽量固定落在同一个上游账号上，避免上下文断掉 |
| **BPS（Basispoints）** | 百分比的万分之一单位。这里指按"基点"精度做模型级路由和计费 |

打个比方：**这就像一个地铁换乘站**。上游有好几条线路（不同厂商的 AI），用户不用知道哪条线在跑，只管在闸口刷一张卡（API Key）进站；换乘站负责告诉你去哪个站台、帮你排队、算好这趟车花了多少钱、票太多的时候拦一下。

它是**自建**的——你自己部署一台服务器，账号和钱都在你手里。

| 关键点 | 说明 |
|--------|------|
| **技术栈** | 后端 Go 1.27 + Gin + Ent；前端 Vue 3.4 + Vite + TailwindCSS |
| **依赖** | PostgreSQL 15+（数据）、Redis 7+（缓存 / 队列） |
| **支持协议** | OpenAI Responses / Chat Completions、Anthropic Messages、Gemini、SSE、WebSocket |
| **默认端口** | `8080` |
| **当前版本** | v2.10.0 |
| **许可证** | GNU LGPL v3.0（宽松copyleft，可商用） |
| **平台** | Linux VPS（推荐）、Mac / Windows 客户端、Apple Silicon macOS 26（原生 container）、K8s / K3s |

---

## 2️⃣ 适合谁用？能解决什么问题？

### ✅ 典型使用者

| 你是谁 | 你会得到什么 |
|--------|-------------|
| **AI 中转站站长** | 想把自己的 ChatGPT / Claude 订阅额度变成对外可售的 API 服务，这是本项目的主要用户群 |
| **小团队 / 工作室** | 内部统一用一个入口调 AI，不用每个人各自去搞账号和 Key，还能统一记账 |
| **多账号持有者** | 有多个订阅账号，需要智能调度、粘性会话、故障转移，避免单账号被打爆 |
| **要精确算钱的人** | Token 级用量追踪、成本计算、账号成本倍率，一笔一笔算得清 |
| **想在 Codex / Claude Code 里用的人** | 新人入门文档里有专篇教你怎么接进 Codex（Cockpit Tools） |

### 🎯 它能帮你解决的具体麻烦

1. **「我有几个 ChatGPT 订阅，但每个工具都要单独配一遍账号。」**
   → 网关统一收口，工具里只填一个 `base_url` 和一把 Key。

2. **「用户拿走了 Key，我完全不知道他用了多少、欠了多少。」**
   → Token 级追踪 + 分组定价 + 内置支付（EasyPay / 支付宝 / 微信 / Stripe），用户自己充值。

3. **「一个账号被猛刷，其它用户全被拖累。」**
   → 用户级 / 账号级 / Key 级三层并发限制 + 可配置的等待队列 + 冷却 + 故障转移。

4. **「上游挂了，我希望能自动切下一个账号，用户无感。」**
   → 智能调度 + 粘性会话 + failover，一个账号报错自动换下一个。

5. **「我想在 Codex（OpenAI 协议）里用 DeepSeek，但不想改客户端配置。」**
   → 支持 Responses ↔ Chat Completions 转换，配好模型映射后，切换 API Key 分组就能换底层模型。

6. **「生图、扫描这些贵操作把磁盘打满 / 把并发打爆。」**
   → 内置图片 HTTPS 中转、磁盘和并发保护、结构化输出校验。

### ❌ 不适合谁

- 只是**个人用一下**、不需要给别人分发的（那直接用官方客户端更省事）
- 不愿意**自己准备服务器和账号**的（这是自建项目，不是 SaaS）
- 对**上游服务条款**不敏感的（用订阅账号做分发可能违反 Anthropic 等厂商的条款，风险自负）

---

## 3️⃣ 核心功能

| # | 功能 | 用大白话说 |
|---|------|-----------|
| 1 | **多账号管理** | 支持 OAuth 和 API Key 两种类型的上游账号，可批量导入导出 |
| 2 | **API Key 分发** | 给用户发 Key，可分组、可设并发上限、可设等待队列 |
| 3 | **精确计费** | Token 级用量追踪、成本计算、账号成本倍率、多维度统计 |
| 4 | **智能调度** | 自动挑最合适的账号，支持粘性会话（同一用户固定账号） |
| 5 | **三层并发控制** | 用户级 / 账号级 / API Key 级独立限制，Key 级支持排队等待 |
| 6 | **速率限制** | 请求数和 Token 数都可限速 |
| 7 | **内置支付系统** | EasyPay 易支付、支付宝官方、微信官方、Stripe，用户自助充值，**不用另搭支付服务** |
| 8 | **Web 管理后台** | 监控、账号管理、分组、用户、Key、渠道监控、用量统计、日志 |
| 9 | **DeepSeek / Codex 适配** | Responses ↔ Chat Completions 转换、工具调用历史兼容、上下文压缩兼容 |
| 10 | **Codex ticket 管理** | 后台采集票据、注入、模型选择、账号状态展示 |
| 11 | **Mihomo 出口管理** | 采集出口统一管理、票据刷新策略、节点状态操作；业务代理和采集出口分开配 |
| 12 | **Excel / BPS 路由** | 模型级 BPS 路由、内嵌图片 HTTPS 中转、磁盘与并发保护、结构化输出校验 |
| 13 | **降智运维** | 给账号配"测试题 + 预期答案 + 测试模型 + 推理强度 + Cron"，自动判题并留运营记录 |
| 14 | **OAuth 首次质量规则** | 新 OAuth 账号自动复制质量运维规则，冷启动探索限定到 OAuth 账号 |
| 15 | **多协议接入** | OpenAI Responses / Chat Completions、Anthropic Messages、Gemini、SSE、WebSocket |
| 16 | **渠道监控** | 多版本渠道监控面板（v2 / v3），带安全默认值 |
| 17 | **密钥保险箱** | 凭据加密存储（`credential-encryption.md`），有独立的 Token Guard 组件 |
| 18 | **iframe 嵌入** | 管理后台可嵌入外部系统（比如工单系统） |
| 19 | **结构化日志** | JSON 格式、分段耗时、健康检查，滚动压缩归档 |
| 20 | **自动迁移** | 首次启动自动连库、跑 SQL 迁移、生成管理员、写出 `config.yaml` |

---

## 4️⃣ 怎么快速上手？

> ⚠️ 下面的命令全部来自本仓库真实文件（`deploy/README.md`、`deploy/docker-deploy.sh`、`deploy/install.sh`、`deploy/docker-compose.local.yml`、`deploy/.env.example`、`Makefile`），不是编的。

### 前置条件

| 依赖 | 版本 | 必需？ |
|------|------|--------|
| Linux VPS | — | ✅ 正式部署推荐 |
| Docker + Docker Compose | — | ✅ Docker 部署方式需要 |
| PostgreSQL | 15+ | ✅ 容器方式自动带 |
| Redis | 7+ | ✅ 容器方式自动带 |
| Go | 1.27.0 | ❌ 只在源码编译时需要 |
| Node.js + pnpm | — | ❌ 只在源码编译时需要 |
| Bash | 4+ | ⚠️ `install.sh` 硬性要求（用关联数组做多语言提示） |

### 方式 A：一键 Docker 部署（最推荐）

```bash
# 下载并运行准备脚本
curl -sSL https://raw.githubusercontent.com/ranxi2001/sub2api/production/deploy/docker-deploy.sh | bash

# 或者先下载再跑
curl -sSL https://raw.githubusercontent.com/ranxi2001/sub2api/production/deploy/docker-deploy.sh -o docker-deploy.sh
chmod +x docker-deploy.sh
./docker-deploy.sh
```

脚本干了什么：

- 下载 `docker-compose.local.yml` 和 `.env.example`
- **自动生成安全密钥**：`JWT_SECRET`、`TOTP_ENCRYPTION_KEY`、`POSTGRES_PASSWORD`
- 生成 `.env` 文件并写入这些密钥
- 创建数据目录 `data/`、`postgres_data/`、`redis_data/`
- **把生成的凭据打印出来**（`POSTGRES_PASSWORD`、`JWT_SECRET` 等）——记下来

跑完脚本后：

```bash
# 启动服务
docker compose -f docker-compose.local.yml up -d

# 看日志
docker compose -f docker-compose.local.yml logs -f sub2api

# 管理员账号密码是自动生成的，在日志里找这一行
docker compose -f docker-compose.local.yml logs sub2api | grep "Generated admin"

# 打开管理后台
# http://localhost:8080
```

### 方式 B：手动 Docker 部署

```bash
# 1. 拉代码
git clone --branch production https://github.com/ranxi2001/sub2api.git
cd sub2api/deploy

# 2. 配环境变量
cp .env.example .env
chmod 600 .env
nano .env          # 至少要设 POSTGRES_PASSWORD

# 3. 生成安全密钥（推荐）
JWT_SECRET=$(openssl rand -hex 32)
TOTP_ENCRYPTION_KEY=$(openssl rand -hex 32)
echo "JWT_SECRET=${JWT_SECRET}" >> .env
echo "TOTP_ENCRYPTION_KEY=${TOTP_ENCRYPTION_KEY}" >> .env

# 4. 建数据目录
mkdir -p data postgres_data redis_data

# 5. 启动
docker compose -f docker-compose.local.yml up -d

# 6. 看日志 + 拿管理员账号
docker compose -f docker-compose.local.yml logs -f sub2api
docker compose -f docker-compose.local.yml logs sub2api | grep "Generated admin"

# 7. 访问
# http://localhost:8080
```

### 两种 compose 文件怎么选？

| 文件 | 数据存哪 | 迁移难度 | 适合谁 |
|------|---------|---------|--------|
| `docker-compose.local.yml` | 本地目录（`./data`、`./postgres_data`、`./redis_data`） | ✅ 简单，整个目录 `tar` 走就行 | **生产、需要经常备份/迁移** |
| `docker-compose.yml` | Docker 命名卷（`/var/lib/docker/volumes/`） | ⚠️ 得用 docker 命令搬 | 简单摆弄、不需要迁移 |

### 方式 C：二进制安装（生产服务器 + systemd）

```bash
curl -fsSL https://raw.githubusercontent.com/ranxi2001/sub2api/production/deploy/install.sh | sudo bash -s -- install
```

安装脚本做的事（`deploy/install.sh`）：

- 安装到 `/opt/sub2api`
- 配置目录 `/etc/sub2api`
- 服务名 `sub2api`，运行用户 `sub2api`
- 装 systemd unit：`deploy/sub2api.service`
- 默认监听 `0.0.0.0:8080`
- 可选安装 Mihomo Codex 出口侧车（`MIHOMO_CODEX_SUBSCRIPTION_URL` 供给时会自动装内核；**内核不会替你创建或购买订阅**）

配套文件：`deploy/install-datamanagementd.sh` + `deploy/sub2api-datamanagementd.service` + `deploy/DATAMANAGEMENTD_CN.md`（数据管理组件）。

### 方式 D：源码编译（开发者）

```bash
# 后端
cd backend
make build          # 产出 bin/server（不嵌前端）
make build-embed    # 产出 bin/server（嵌入前端）
make test-unit      # 单元测试
make test-integration

# 前端
cd frontend
pnpm install
pnpm build
```

根目录 `Makefile` 也有 `build` / `test` / `test-backend` / `test-frontend` 等目标。

### 方式 E：Apple Silicon Mac（macOS 26 原生 container）

```bash
./apple-container.sh init
./apple-container.sh up
./apple-container.sh status
./apple-container.sh logs app -f
```

用 Apple 原生 named volume，不依赖 Docker。**不提供开机自启**，主机重启后要手动 `./apple-container.sh up` 再跑一遍。详见 `deploy/APPLE_CONTAINER.md`。

### 方式 F：Kubernetes / K3s

见 `deploy/kubernetes/README.md`，需要预置配置和 Secrets。

### 首次初始化管理员

| 情况 | 结果 |
|------|------|
| `ADMIN_EMAIL` 和 `ADMIN_PASSWORD` **都留空** | 自动生成随机登录邮箱和密码，去日志里 `Generated admin` 那行找 |
| 显式指定 | 邮箱必须符合登录格式，密码必须 **8–72 字节** |
| 已有管理员或已有用户的部署 | **跳过**创建和这项校验 |

### 配置 API Key 并发等待队列

当某个 API Key 设了大于 0 的 `concurrency_limit`，达到上限后的新请求会**在原连接上等待**空闲槽位，而不是立刻报错。等待策略是**全局配置**，进程启动时读取：

```yaml
gateway:
  api_key_queue:
    max_waiting: 5
    timeout_seconds: 30
```

| 环境变量 | 默认值 | 说明 |
|----------|--------|------|
| `GATEWAY_API_KEY_QUEUE_MAX_WAITING` | `5` | 每个受限 Key 允许额外等待的请求数；`0` 关闭 Key 排队 |
| `GATEWAY_API_KEY_QUEUE_TIMEOUT_SECONDS` | `30` | 单个请求最长等待秒数，必须为正整数 |

返回码约定：

| 情况 | HTTP 码 | 错误标识 |
|------|---------|---------|
| 关闭排队时达到上限 | `429` | `gateway_concurrency_limit` |
| 队列已满 | `429` | `api_key_queue_full` |
| 等待超时 | `429` | `api_key_queue_timeout` |
| Key 换组 / 平台 / 计费模式变化 | `503` | `API_KEY_GROUP_CHANGED` |
| WebSocket 鉴权或权限失败 | `1008`（关闭连接） | — |
| 容量或临时服务错误 | `1013`（关闭连接） | — |

> ⚠️ 改了 Compose `.env` 后**需要重建容器**才能更新环境变量。调大等待时间时要确认客户端和反向代理的首字节超时够长。
> ⚠️ 升级自动应用 `237_add_api_key_concurrency_limit.sql`，旧 Key 默认为 `0`。**回退二进制不会撤销数据库新增字段**。

---

## 5️⃣ 常见问题 / 注意事项

### ❓ 会踩的坑

| 现象 | 原因 | 怎么办 |
|------|------|--------|
| **起不来，提示 `POSTGRES_PASSWORD is required`** | `.env` 里没设数据库密码 | `cp .env.example .env` 后填 `POSTGRES_PASSWORD`；compose 里这行写的是 `${POSTGRES_PASSWORD:?...}`，没设就直接退出 |
| **登不进管理后台** | 管理员账号是自动生成的，你没记住 | `docker compose logs sub2api \| grep "Generated admin"` 找出来 |
| **`install.sh` 报 `This installer must be run with Bash 4.0 or later`** | 用了老版本的 bash / macOS 自带 bash 3.2 | 换 Bash 4+ 解释器跑，或直接用 Docker 方式 |
| **改了 `.env` 不生效** | 环境变量是容器启动时注入的 | `docker compose up -d --force-recreate` 重建容器，不是 `restart` |
| **请求排队很久然后 429** | Key 级并发上限太低，等待队列也满了 | 调 `concurrency_limit`、调大 `GATEWAY_API_KEY_QUEUE_MAX_WAITING` / `..._TIMEOUT_SECONDS` |
| **同一个 Key 换组后报 `503 API_KEY_GROUP_CHANGED`** | Key 的分组、平台或计费模式在等待期间变了 | 客户端重试即可；也可能需要重新拉取模型列表 |
| **回退版本后行为异常** | 数据库迁移不可逆 | `237_add_api_key_concurrency_limit.sql` 加的字段**回退二进制不会撤销**，字段留着不影响，但别指望完全回到旧状态 |
| **生图 / 大文件把磁盘撑爆** | 缓存没清理 | 打开图片 HTTPS 中转，配置磁盘保护参数 |
| **「降智」了，不知道为什么** | 降智是账号、路由、上游限制、请求配置共同作用的结果，不是单一开关 | 用「降智运维」给账号配测试题 + 预期答案 + 测试模型 + 推理强度 + Cron，去「运营记录」看回答原文和判题理由 |
| **开了 BPS 但请求走了别的路** | BPS 是按账号和模型维度启用的 | 只有账号被勾选的模型才强制走 BPS；不支持的能力由 BPS 返回错误，**不会静默回退**原 Codex 通道 |
| **Release 成功但线上没变** | Release 只是打包发布 | 线上状态要**单独验证**，别把"发版成功"当"部署完成" |

### ⚠️ 必须知道的注意事项

1. **上游服务条款风险** —— 用订阅账号做 API 分发可能违反 Anthropic 等厂商的服务条款。**务必先读上游协议，风险自负。**
2. **不要提交敏感信息** —— 不要把 Token、OAuth 导出、ticket、代理凭据或生产配置写进 PR 或仓库。
3. **`.env` 权限** —— `chmod 600 .env`，别让它变成 644。
4. **数据迁移用 local 版** —— `docker-compose.local.yml` 的数据全在 `./` 下，整个 `deploy/` 目录 `tar` 走就能搬家；命名卷版本得用 docker 命令搬。
5. **`.env` 优先级** —— Docker 部署下 `env_file` 注入的是**真实环境变量**，只改 `config.yaml` 压不住它。
6. **许可证是 LGPL v3.0** —— 允许商用和闭源衍生（需保留 LGPL 部分声明），但**上游 README 明确写了「无商业授权」声明**，两者要一起看。具体以 `LICENSE` 文件为准。
7. **本 fork 的分支纪律** —— `production` 分支是唯一权威，**不会用上游默认分支或 tag 直接覆盖生产历史**。涉及生产分支的修复应先过 CI 再合并。
8. **网站与文档** —— 平台站 `https://tosky.io/`，使用文档 `https://tosky.io/docs/`。

---

## 6️⃣ 目录结构速览

```
sub2api/
├── backend/                       ← Go 后端
│   ├── cmd/
│   │   ├── server/                # 主服务入口
│   │   ├── jwtgen/                # JWT 密钥生成工具
│   │   ├── profit-preview/        # 收益预览
│   │   └── cleanup-ingress-reject-logs/  # 清理入站拒绝日志
│   ├── internal/
│   │   ├── config/                # 配置加载
│   │   ├── domain/                # 领域模型
│   │   ├── handler/               # HTTP 处理器（auth / api_key / admin / ...）
│   │   ├── middleware/            # 中间件（鉴权、限流、日志、请求捕获）
│   │   ├── service/               # 业务服务（account / billing / quality / prism ...）
│   │   ├── repository/            # 数据访问层
│   │   ├── model/                 # 数据库模型
│   │   ├── platform/liveattestation/  # 活体校验
│   │   ├── mihomo/                # Mihomo 出口管理
│   │   ├── reauthruntime/         # OAuth 重新授权运行时
│   │   ├── requestcapture/        # 请求抓包（调试用）
│   │   ├── securityaudit/         # 安全审计
│   │   ├── serverless/            # 无服务器模式
│   │   ├── payment/               # 支付系统
│   │   ├── setup/                 # 首次自动安装逻辑
│   │   ├── server/                # 服务器装配
│   │   ├── web/                   # Web 路由挂载
│   │   └── util/                  # 工具函数
│   ├── ent/                       # Ent ORM schema 与生成代码
│   ├── migrations/                # SQL 迁移文件（按数字顺序）
│   ├── pkg/                       # 公共库（含 pluginapi 插件 API）
│   ├── resources/model-pricing/   # 模型定价表
│   ├── scripts/                   # 后端脚本
│   ├── Dockerfile / Makefile      # 后端构建与测试
│   └── go.mod / go.sum
├── frontend/                      ← Vue 3 前端
│   ├── src/
│   │   ├── api/                   # API 客户端封装
│   │   ├── components/            # 通用组件
│   │   ├── features/              # 业务功能模块（渠道监控 v2/v3 等）
│   │   ├── views/                 # 页面（admin / user / auth / settings）
│   │   ├── stores/                # Pinia 状态管理
│   │   ├── router/                # 路由
│   │   ├── i18n/                  # 多语言
│   │   └── composables/           # 组合式函数
│   ├── public/                    # 静态资源
│   ├── package.json / vite.config.ts / tailwind.config.js
│   └── audit.json                 # 前端依赖审计配置
├── deploy/                        ← 部署资产
│   ├── docker-deploy.sh           # ★ 一键 Docker 部署脚本（推荐）
│   ├── install.sh                 # ★ 二进制一键安装脚本（systemd）
│   ├── install-datamanagementd.sh # 数据管理组件安装脚本
│   ├── docker-compose.local.yml   # ★ 本地目录存储版 compose（推荐生产）
│   ├── docker-compose.yml         # 命名卷存储版 compose
│   ├── docker-compose.dev.yml     # 开发用
│   ├── docker-compose.standalone.yml
│   ├── .env.example               # ★ 环境变量模板
│   ├── config.example.yaml        # config.yaml 示例
│   ├── sub2api.service             # systemd unit
│   ├── sub2api-datamanagementd.service
│   ├── apple-container.sh          # Apple Silicon 原生 container 部署脚本
│   ├── APPLE_CONTAINER.md
│   ├── install-mihomo-codex.sh     # Mihomo Codex 出口侧车安装
│   ├── mihomo-codex.service / mihomo-codex.config.example.yaml
│   ├── Caddyfile                  # Caddy 反代配置
│   ├── EDGE_SECURITY.md           # 反代 / CDN / WAF / 可信代理加固
│   ├── DOCKER.md                  # Docker Hub 镜像说明
│   ├── kubernetes/                # K8s 部署（含 README）
│   ├── copilot-sdk/               # Copilot SDK 接入
│   └── tests/                     # 部署脚本的安全 / 环境变量测试
├── docs/                          ← 大量中文运维文档
│   ├── 新人入门.md                 # ★ 第一次部署看这个
│   ├── 新人入门-使用Sub2API.md      # ★ 接入 Codex
│   ├── PAYMENT_CN.md              # 支付配置
│   ├── excel-bps.md               # Excel / BPS 说明
│   ├── codex-780-native-mint.md   # 780 打票
│   ├── account-quality-operations.md  # 账号质量运维
│   ├── account-token-guard.md / account-token-guard-v2.md
│   ├── mihomo-managed / MIHOMO_MANAGED_CN.md
│   ├── channel-monitor-v2-safe-defaults.md
│   ├── priority-scheduling.md     # 优先级调度
│   ├── credential-encryption.md   # 凭据加密
│   ├── legal/                     # 法务文档
│   └── design/ plans/ specs/ images/
├── prism-adapter/                 ← Prism 适配器
├── skills/                        ← 运维技能库
├── tools/                         ← 工具脚本（OAuth 重授权 worker 等）
├── openspec/                      ← 变更提案（规格驱动流程）
├── assets/                        ← logo、社区群二维码、赞助码、合作伙伴 logo
├── .github/
│   ├── workflows/                 # CI、发布、安全扫描、矩阵构建
│   ├── release-tools/             # 发布工具（release_matrix.py 等）
│   └── ISSUE_TEMPLATE/            # Bug / 功能 / 文档 / 提问表单
├── Dockerfile / Dockerfile.goreleaser   # 多阶段 / goreleaser 构建
├── Makefile                       # 根构建入口
├── .goreleaser.yaml / .goreleaser.simple.yaml
├── README.md / README_CN.md / README_EN.md / README_JA.md   # 四语 README
├── CONTRIBUTING.md / DEV_GUIDE.md / CLA.md
└── LICENSE                        # LGPL v3.0
```

### 技术栈一览

| 组件 | 技术 |
|------|------|
| 后端 | Go 1.27.0, Gin, Ent |
| 前端 | Vue 3.4+, Vite 5+, TailwindCSS |
| 数据库 | PostgreSQL 15+ |
| 缓存 / 队列 | Redis 7+ |
| API 协议 | OpenAI Responses / Chat Completions、Anthropic Messages、Gemini、SSE、WebSocket |
| 调度与可靠性 | 粘性会话、并发控制、限流、故障转移、ticket 准入与冷却 |
| 观测与运维 | 结构化日志、请求分段耗时、健康检查、Mihomo 出口与 systemd |
| 交付与质量 | Docker Compose、GitHub Actions、Go 单元测试、前端类型检查与构建 |

---

## 7️⃣ 本仓库说明（Fork 信息）

| 项目 | 说明 |
|------|------|
| **本仓库** | `aiwozhonghua81/sub2api` |
| **上游项目** | [`ranxi2001/sub2api`](https://github.com/ranxi2001/sub2api) |
| **项目名** | Sub2API |
| **关系** | **Fork（派生仓库）** |
| **当前分支** | `production` |
| **当前版本** | v2.10.0（提交 `5ca3cca21`，2026-10-07） |
| **上游原始作者** | Wesley Liddick（`shaw-wei@foxmail.com` / `dev@sub2api.org`）—— 即 `Wei-Shaw/sub2api` 的作者 |
| **仓库描述** | 基于 Sub2API 由多位站长持续维护的独立分支，更加适合中转站长使用，高频更新+同步上游，扩展实用功能并发布生产版本 |
| **主页** | https://qm.qq.com/q/qtvhZvShEI |

### 三层上游关系（这仓库是"孙级" fork）

```
Wei-Shaw/sub2api              ← 原始项目（Wesley Liddick 维护，LGPL v3.0）
        │
        └── ranxi2001/sub2api  ← tosky.io 维护的独立生产分支（900+ stars，production 分支）
                    │
                    └── aiwozhonghua81/sub2api   ← 本仓库（本站长的分发镜像）
```

实测数据：

| 对比 | 结果 |
|------|------|
| `git rev-list --left-right --count upstream/production...production`（对 `ranxi2001/sub2api`） | `2151    0` —— **本地落后上游 2151 个提交，本地没有独占提交** |
| `git rev-list --left-right --count origin2/main...production`（对 `Wei-Shaw/sub2api` 的 main） | `1013    1124` —— 与原始项目已双向分叉 |
| 本地 HEAD | `5ca3cca21 chore: release v2.10.0`（2026-10-07） |
| 上游 HEAD（拉取时） | `8027cb579 Merge pull request #794 from ranxi2001/fix/trusted-oauth-relay-tier`（2026-11-11） |
| merge-base | `5ca3cca21`（即本 fork 的 HEAD 就是与上游的分叉点） |
| 本地提交作者里有没有本站长的提交 | **没有**。所有提交作者都是上游社区：`ranxi2001`、`Onefly`、`Wesley Liddick`、`shaw`、`erio`、`yangjianbo` 等 |

### 所以「本 fork 到底改了什么」？

**结论：本 fork 目前是 `ranxi2001/sub2api` 的 `production` 分支在 v2.10.0 时刻的忠实镜像，没有自己独占的代码提交。**

| 事实 | 说明 |
|------|------|
| **本地 = 上游 v2.10.0 快照** | merge-base 就是本地 HEAD，说明本地是在上游 v2.10.0 那个点切出来的，之后双方各自走 |
| **本地落后上游 2151 个提交** | 上游 `ranxi2001/sub2api` 从 2026-10-07 到 2026-11-11 期间又合了 2151 个提交（大量 Codex 修复、OAuth 重授权、渠道监控 v3 等） |
| **上游没有任何本地没有的独占提交** | 右侧计数为 0，本 fork 没有自研代码 |
| **「持续维护」「独立分支」指的是上游 `ranxi2001/sub2api`** | 仓库描述里的"由多位站长持续维护的独立分支"是 `ranxi2001/sub2api` 这个**第二层 fork** 的定位——它基于 `Wei-Shaw/sub2api` 按需同步上游、自己发布生产版本、扩展了 DeepSeek/Codex 适配、Codex ticket 管理、Mihomo 出口管理、Excel/BPS 等功能 |
| **本 fork 唯一新增的内容** | **README 顶部的这段中文详细说明**（以及仓库 description 里补的中文注解） |

### 这个 fork 的实际定位

- ✅ **本 fork 的价值**：给站长们一个**带完整中文说明的入口**。想看这个项目到底是干什么的、怎么部署、哪些坑要注意，**直接看 README 顶部的中文说明就行**，不用去翻几十个中文运维文档。
- ⚠️ **要最新版本请用上游**：`ranxi2001/sub2api` 的 `production` 分支仍在高频更新，**本 fork 停留在 v2.10.0，落后 2151 个提交**。生产环境请直接用上游的 Release 和镜像：
  - 镜像：`ghcr.io/ranxi2001/sub2api:latest`
  - Release：https://github.com/ranxi2001/sub2api/releases
  - 文档站：https://tosky.io/docs/
- ⚠️ **不要在本 fork 上改代码**：改了也合不回上游（本地没有独占提交基础），而且上游有严格的 CI 和贡献流程。
- ⚠️ **生产分支纪律**：`production` 分支是唯一权威，**不会用上游默认分支或 tag 直接覆盖本 fork 的生产历史**。

### 上游项目简介

> **Sub2API** 是一个 AI API 网关平台，用于分发和管理 AI 产品订阅的 API 配额。用户通过平台生成的 API Key 调用上游 AI 服务，平台负责**鉴权、计费、负载均衡和请求转发**。
>
> 它支持多账号管理、API Key 分发、Token 级精确计费、智能调度（粘性会话）、三层并发控制、可配置限流、内置支付系统（EasyPay / 支付宝 / 微信 / Stripe）、Web 管理后台，以及通过 iframe 嵌入外部系统。

### 本 README 的结构

```
第 1 行
├── <!-- 中文说明开始 -->
│   ├── 1️⃣ 这是什么？
│   ├── 2️⃣ 适合谁用 / 能解决什么问题
│   ├── 3️⃣ 核心功能
│   ├── 4️⃣ 怎么快速上手
│   ├── 5️⃣ 常见问题 / 注意事项
│   ├── 6️⃣ 目录结构速览
│   └── 7️⃣ 本仓库说明（Fork 信息）  ← 你正在看的最后一段
├── 
## 🖼️ 界面截图一览
下面这些图都是**这个仓库里原本就带的真实截图**，不是画的示意图：

**① 项目图标**

<img src="assets/logo-icon.png" alt="项目图标" width="820">

**② 管理后台功能检索**

<img src="assets/admin-feature-search.jpg" alt="管理后台功能检索" width="820">

**③ 模型选择器**

<img src="assets/gpt6-sol-luna-opus55-model-selector.jpg" alt="模型选择器" width="820">

<!-- 中文说明结束 -->
│
├── （分隔线）
│
└── ↑↑↑ 从这里开始，是上游项目 ranxi2001/sub2api 的原始 README，一字未改 ↑↑↑
```

上游 README 里还有**技术栈表格**、**贡献者列表**、**「重要提醒」**（服务条款风险、合规使用、免责声明、无商业授权声明）和**许可证说明**，本 fork 一并保留。**部署前请务必读完下半部分的「⚠️ 重要提醒」。**

---

<!-- 中文说明结束 -->

---

<div align="center">

<img src="assets/logo-icon.png" alt="Sub2API Logo" width="128" />

# Sub2API

[![Go](https://img.shields.io/badge/Go-1.27.0-00ADD8.svg)](https://golang.org/)
[![Vue](https://img.shields.io/badge/Vue-3.4+-4FC08D.svg)](https://vuejs.org/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15+-336791.svg)](https://www.postgresql.org/)
[![Redis](https://img.shields.io/badge/Redis-7+-DC382D.svg)](https://redis.io/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED.svg)](https://www.docker.com/)


**tosky.io 维护的 Sub2API 独立分支**

按需同步上游更新，扩展实用功能，独立发布生产版本。

[平台](https://tosky.io/) · [使用文档](https://tosky.io/docs/) · [版本下载](https://github.com/ranxi2001/sub2api/releases) · [问题反馈](https://github.com/ranxi2001/sub2api/issues)

中文 | [English](README_EN.md) | [日本語](README_JA.md)

</div>

## 社区交流群

欢迎正在部署、使用或维护 Sub2API 的朋友加入 QQ 群「中转技术交流」（群号 **1004036018**），交流部署实践、协议兼容、出口代理和功能改进。也欢迎参与问题复现、测试、文档补充和 Pull Request，一起维护这个独立分支。

<table>
<tr>
<td align="center" width="50%">
  <img src="assets/community/qq-relay-tech-group.png" alt="QQ 群：中转技术交流，群号 1004036018" width="71%" />
  <br />
  加入 QQ 群「中转技术交流」
</td>
<td align="center" width="50%">
  <img src="assets/community/coffee-reward.png" alt="支付宝和微信赞赏码：如果帮到大家可以打赏咖啡" width="100%" />
  <br />
  如果帮到大家可以打赏咖啡！
</td>
</tr>
</table>

二维码长期有效。需要长期留档、报告问题或讨论具体改动时，请使用 [Issues](https://github.com/ranxi2001/sub2api/issues)。

## 快速开始

新人部署、官方版替换、Excel / BPS 配置、780 / 292 打票边界和“降智”运维步骤，请阅读[新人入门文档](docs/新人入门.md)。

已经托管账号或完成部署，想在 Codex 中使用？请阅读[新人入门：使用 Sub2API 接入 Codex](docs/新人入门-使用Sub2API.md)，按步骤创建 API Key、配置 Cockpit Tools 并验证调用。


## 本仓库的维护方向

本仓库是 `ranxi2001/sub2api` 的独立生产 fork，基于 [Wei-Shaw/sub2api](https://github.com/Wei-Shaw/sub2api) 按需同步。应用功能、上游修复、Release 和生产验证都以 `production` 分支为准；不会用上游默认分支或 tag 直接覆盖本 fork 的生产历史。

- **DeepSeek 与 Codex 适配**：支持 Responses 到 Chat Completions 的转换、工具调用历史和上下文压缩兼容。配置模型映射后，可通过切换 API Key 分组使用 DeepSeek，沿用客户端配置。[操作教程](https://tosky.io/docs/?doc=deepseek-switch-group)
- **Codex ticket 管理**：提供后台采集、注入、模型选择及账号状态展示；相关开关和采集代理由管理员配置。
- **Mihomo 出口管理**：集成采集出口管理、票据刷新策略和节点状态操作，日常业务代理与采集出口分别配置。
- **Excel / Basispoints**：维护模型级 BPS 路由、内嵌图片 HTTPS 中转、磁盘和并发保护、结构化输出校验，以及工具历史和 transport 恢复。开启 BPS 且账号未因 403 自动关闭时，请求强制走 BPS；不支持的能力会由 BPS 返回错误，不再静默回退原 Codex 通道。
- **上游修复维护**：持续跟踪上游 Codex、Responses、工具调用、密文恢复和限流修复；先确认与本 fork 的行为差异，再按提交级别移植并补充回归测试。
- **独立发布与升级**：使用 `ranxi2001/sub2api` 的 Release、安装资源和容器镜像。版本变更见 [更新说明](https://github.com/ranxi2001/sub2api/releases)；Release 成功不代表生产服务已经部署，线上状态需要单独验证。

贡献代码时，请在独立分支中说明影响的请求路径、账号类型、配置默认值和兼容边界。涉及生产分支的修复应先进入原 PR head，通过 CI 后再合并；不要提交 Token、OAuth 导出、ticket、代理凭据或生产配置。

## 项目概述

Sub2API 是一个 AI API 网关平台，用于分发和管理 AI 产品订阅的 API 配额。用户通过平台生成的 API Key 调用上游 AI 服务，平台负责鉴权、计费、负载均衡和请求转发。

## 核心功能

- **多账号管理** - 支持多种上游账号类型（OAuth、API Key）
- **API Key 分发** - 为用户生成和管理 API Key
- **精确计费** - Token 级别的用量追踪和成本计算
- **智能调度** - 智能账号选择，支持粘性会话
- **并发控制** - 用户级、账号级和 API Key 级并发限制，支持可配置的 Key 等待队列
- **速率限制** - 可配置的请求和 Token 速率限制
- **内置支付系统** - 支持 EasyPay 易支付、支付宝官方、微信官方、Stripe，用户自助充值，无需独立部署支付服务（[配置指南](docs/PAYMENT_CN.md)）
- **管理后台** - Web 界面进行监控和管理
- **外部系统集成** - 支持通过 iframe 嵌入外部系统（如工单等），扩展管理后台功能

### 首次初始化管理员

全新自动安装时，`ADMIN_EMAIL` 和 `ADMIN_PASSWORD` 留空会生成随机登录邮箱和密码，可从首次启动日志中的 `Generated admin` 获取。显式指定时，邮箱必须符合登录格式，密码必须为 8–72 字节。已有管理员或已有用户的部署跳过创建与此项校验。详细配置见 [部署说明](deploy/README.md)。

## API Key 并发等待队列

当 API Key 设置了大于 `0` 的 `concurrency_limit` 时，达到上限后的新请求会在原连接上等待空闲槽位。默认值 `0` 不增加 Key 级并发限制。等待策略是全局配置，进程启动时读取：

```yaml
gateway:
  api_key_queue:
    max_waiting: 5
    timeout_seconds: 30
```

| 环境变量 | 默认值 | 说明 |
|----------|--------|------|
| `GATEWAY_API_KEY_QUEUE_MAX_WAITING` | `5` | 每个受限 Key 允许额外等待的请求数；`0` 关闭 Key 排队。 |
| `GATEWAY_API_KEY_QUEUE_TIMEOUT_SECONDS` | `30` | 单个请求最长等待秒数，必须为正整数。 |

- 等待名额按 Key 独立计算，不保证 FIFO；`concurrency_limit: 0` 的 Key 不进入队列。
- 队列用于 HTTP/SSE、OpenAI Responses WebSocket 每轮请求和 Live 创建的 Key 准入，与用户级、账号级等待限制独立。
- 等待期间复核 Key、用户及当前请求的模型/能力权限。Key 换组、平台或计费模式变化返回可重试的 `503` / `API_KEY_GROUP_CHANGED`。WebSocket 鉴权/权限失败以 `1008` 关闭，容量或临时服务错误以 `1013` 关闭。
- 关闭排队时达到上限返回 `429` / `gateway_concurrency_limit`；队列满返回 `429` / `api_key_queue_full`；等待超时返回 `429` / `api_key_queue_timeout`。
- 两个配置值必须为整数；负数、小数、非法字符串或超出范围会阻止启动。即使关闭排队，超时也必须为正数。
- 修改 Compose `.env` 后需要重建容器以更新环境变量。调大等待时间时，需要确认客户端及反向代理的首字节超时。
- 升级自动应用 `237_add_api_key_concurrency_limit.sql`，旧 Key 默认为 `0`。回退二进制不会撤销数据库新增字段。

## 技术栈

| 组件 | 技术 |
|------|------|
| 后端 | Go 1.27.0, Gin, Ent |
| 前端 | Vue 3.4+, Vite 5+, TailwindCSS |
| 数据库 | PostgreSQL 15+ |
| 缓存/队列 | Redis 7+ |
| API 协议 | OpenAI Responses / Chat Completions、Anthropic Messages、Gemini、SSE、WebSocket |
| 调度与可靠性 | 粘性会话、并发控制、限流、故障转移、ticket 准入与冷却 |
| 观测与运维 | 结构化日志、请求分段耗时、健康检查、Mihomo 出口与 systemd |
| 交付与质量 | Docker Compose、GitHub Actions、Go 单元测试、前端类型检查与构建 |

## 贡献与协作

欢迎围绕协议兼容、账号调度、Codex ticket、支付计费、管理后台和运维观测提交改进。高质量贡献应尽量保持边界清晰，并在 PR 中说明请求路径、状态变化、兼容性影响和验证证据。

开始前请阅读 [贡献指南](CONTRIBUTING.md)：包含最小复现、日志脱敏、开发环境、验证命令和 PR 流程。[提交 Issue](https://github.com/ranxi2001/sub2api/issues/new/choose) 时可选择 Bug、功能建议、文档或使用问题表单；提交 PR 时按模板填写行为变化和实际验证结果。

建议按以下方式提交：

- **协议与网关**：补充请求/响应样例，覆盖流式终止、工具调用、重试和上游错误映射。
- **调度与账号**：说明候选筛选、粘性状态、并发占用、冷却窗口和故障转移行为，避免改变幂等语义。
- **后台与配置**：同步前后端类型、默认值、权限边界和迁移兼容性。
- **运维与部署**：提供离线或 mock 验证，不在 PR 中写入 Token、ticket、代理凭据或生产配置。
- **验证与审查**：列出实际运行的测试、构建或脚本检查；未运行的检查不要标记为通过。

感谢已合并 PR 的贡献者（按本仓库已合并 PR 数量降序排列，数量相同时按 GitHub 用户名排序）：

<p>
  <a href="https://github.com/ranxi2001"><img src="https://avatars.githubusercontent.com/u/77790009?v=4" width="56" height="56" alt="Onefly" title="Onefly" /></a>
  <a href="https://github.com/akihitohyh"><img src="https://avatars.githubusercontent.com/u/79531840?v=4" width="56" height="56" alt="akihitohyh" title="akihitohyh" /></a>
  <a href="https://github.com/blackdm666"><img src="https://avatars.githubusercontent.com/u/67053678?v=4" width="56" height="56" alt="老黑" title="老黑" /></a>
  <a href="https://github.com/loserzero-7"><img src="https://avatars.githubusercontent.com/u/177290228?v=4" width="56" height="56" alt="loserzero-7" title="loserzero-7" /></a>
  <a href="https://github.com/psyche314"><img src="https://avatars.githubusercontent.com/u/180074435?v=4" width="56" height="56" alt="psyche314" title="psyche314" /></a>
  <a href="https://github.com/akayedi"><img src="https://avatars.githubusercontent.com/u/90334896?v=4" width="56" height="56" alt="akayedi" title="akayedi" /></a>
  <a href="https://github.com/EdmundMad0309"><img src="https://avatars.githubusercontent.com/u/122854730?v=4" width="56" height="56" alt="EdmundMad0309" title="EdmundMad0309" /></a>
  <a href="https://github.com/spake404"><img src="https://avatars.githubusercontent.com/u/123435269?v=4" width="56" height="56" alt="spake404" title="spake404" /></a>
  <a href="https://github.com/wuwu131452011"><img src="https://avatars.githubusercontent.com/u/165636850?v=4" width="56" height="56" alt="wuwu131452011" title="wuwu131452011" /></a>
  <a href="https://github.com/abcgoodwei"><img src="https://avatars.githubusercontent.com/u/35887090?v=4" width="56" height="56" alt="abcgoodwei" title="abcgoodwei" /></a>
  <a href="https://github.com/AI8888-SHOP"><img src="https://avatars.githubusercontent.com/u/297756662?v=4" width="56" height="56" alt="AI8888-SHOP" title="AI8888-SHOP" /></a>
  <a href="https://github.com/buluw"><img src="https://avatars.githubusercontent.com/u/45087912?v=4" width="56" height="56" alt="buluw" title="buluw" /></a>
  <a href="https://github.com/danvilig"><img src="https://avatars.githubusercontent.com/u/230506945?v=4" width="56" height="56" alt="danvilig" title="danvilig" /></a>
  <a href="https://github.com/LeeSssong"><img src="https://avatars.githubusercontent.com/u/37948462?v=4" width="56" height="56" alt="LeeSssong" title="LeeSssong" /></a>
  <a href="https://github.com/Mickey0811"><img src="https://avatars.githubusercontent.com/u/49522921?v=4" width="56" height="56" alt="Mickey0811" title="Mickey0811" /></a>
  <a href="https://github.com/mracry"><img src="https://avatars.githubusercontent.com/u/112537993?v=4" width="56" height="56" alt="mracry" title="mracry" /></a>
  <a href="https://github.com/Terry1321"><img src="https://avatars.githubusercontent.com/u/41000037?v=4" width="56" height="56" alt="Terry1321" title="Terry1321" /></a>
  <a href="https://github.com/yuanyuan19"><img src="https://avatars.githubusercontent.com/u/120552623?v=4" width="56" height="56" alt="yuanyuan19" title="yuanyuan19" /></a>
  <a href="https://github.com/zhoumooooo"><img src="https://avatars.githubusercontent.com/u/271002711?v=4" width="56" height="56" alt="zhoumooooo" title="zhoumooooo" /></a>
</p>

---


## ⚠️ 重要提醒

使用本项目前，请务必仔细阅读以下内容：

- **🚨 服务条款风险**：使用本项目可能违反 Anthropic 等上游服务商的服务条款。请在使用前仔细阅读相关服务商的用户协议，由此产生的一切风险由用户自行承担。
- **⚖️ 合规使用**：请在符合您所在国家或地区法律法规的前提下使用本项目，严禁将其用于任何违法违规用途。
- **📖 免责声明**：本项目仅供技术学习与研究使用，作者不对因使用本项目导致的账户封禁、服务中断、数据丢失或其他任何直接或间接损失承担责任。
- **🚫 无商业授权**：本项目从未授权任何个人或组织基于本项目开展任何形式的商业化运营。任何以本项目名义或基于本项目从事的商业行为均与本项目及其开发者无关，由此产生的一切纠纷、损失和法律责任由行为主体自行承担。

## 许可证

本项目基于 [GNU 宽通用公共许可证 v3.0](LICENSE)（或更高版本）授权。

Copyright (c) 2026 Wesley Liddick

---

<div align="center">

**如果觉得有用，请给个 Star 支持一下！**

</div>

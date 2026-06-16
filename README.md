<div align="center">

# immich-frps — frps 智能部署与管理面板

**一键部署 | Web 管理面板 | 智能优化引擎 | 大文件传输 | 监控仪表盘**

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![GitHub stars](https://img.shields.io/github/stars/attychen/immich-frps.svg)](https://github.com/attychen/immich-frps/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/attychen/immich-frps.svg)](https://github.com/attychen/immich-frps/issues)
[![Build Status](https://img.shields.io/github/actions/workflow/status/attychen/immich-frps/release.yml?branch=frps-panel)](https://github.com/attychen/immich-frps/actions)

</div>

---

## 📋 项目简介

`immich-frps` 是一个集 **frps 一键部署** 与 **Web 可视化管理面板** 于一体的综合解决方案。基于 [fatedier/frp](https://github.com/fatedier/frp)，专为 Immich 等大文件传输场景深度优化。

### 🎯 两大核心组件

| 组件 | 说明 |
|------|------|
| **deploy_frps.sh** | 交互式一键部署脚本，自动检测架构、自适应内存、动态调优网络参数 |
| **frps-panel** | Web 可视化管理面板，实时监控、智能优化、带宽管理、防火墙配置 |

---

## ✨ 功能特性

### 🚀 一键部署脚本
- ✅ 自动检测系统架构（amd64/arm64/armv7）
- ✅ 自适应内存，动态调整网络参数
- ✅ 交互式菜单界面，支持修改端口/用户名/密码
- ✅ 100GB 大文件传输优化
- ✅ 配置自动备份与恢复
- ✅ 多服务器管理

### 🖥️ Web 管理面板 (frps-panel)
- 📊 **监控仪表盘** — 实时流量、连接数、CPU/内存监控
- ⚡ **智能优化引擎** — 根据服务器配置自动计算最优 TCP 参数
- 📡 **带宽管理器** — 限速、流量统计、带宽分配
- 🛡️ **防火墙配置** — 可视化端口管理、IP 黑白名单
- 🔧 **服务端配置** — 在线编辑 frps 配置，热重载
- 🌐 **国际化** — 支持中文/英文切换
- 📋 **诊断工具** — 系统诊断、网络检测、性能分析

---

## 🚀 快速开始

### 方式一：一键部署脚本

```bash
git clone https://github.com/attychen/immich-frps.git
cd immich-frps
bash deploy_frps.sh
```

### 方式二：使用 Web 管理面板

```bash
# 克隆项目
git clone https://github.com/attychen/immich-frps.git
cd immich-frps

# 构建并运行（需要 Go 1.21+）
cd go-frp-panel
go build -o frps-panel ./cmd/server
./frps-panel

# 访问面板
# http://your-server:7500
```

### 方式三：Docker 部署

```bash
# frps 服务端
docker run -d \
  --name frps \
  --network host \
  -v /etc/frp:/etc/frp \
  attychen/frps:latest-amd64

# frpc 客户端
docker run -d \
  --name frpc \
  --network host \
  -v /etc/frp:/etc/frp \
  attychen/frpc:latest-amd64
```

---

## 📂 项目结构

```
immich-frps/
├── deploy_frps.sh          # 一键部署脚本（交互式）
├── doc.html                # 使用文档
├── Dockerfile.frps         # frps Docker 镜像
├── Dockerfile.frpc         # frpc Docker 镜像
├── go-frp-panel/           # Web 管理面板（Go + Vue3）
│   ├── cmd/server/         # 面板服务入口
│   ├── internal/
│   │   ├── frps/           # frps 核心逻辑
│   │   │   ├── admin_api.go
│   │   │   ├── api.go
│   │   │   ├── bandwidth.go
│   │   │   ├── diagnostic_api.go
│   │   │   ├── firewall.go
│   │   │   └── optimize_api.go
│   │   └── frpc/           # frpc 客户端逻辑
│   ├── pkg/
│   │   ├── diagnostic/     # 诊断工具
│   │   ├── optimizer/      # 智能优化引擎
│   │   └── utils/          # 工具函数
│   └── web/frps/src/       # Vue3 前端
│       ├── components/
│       │   ├── OptimizePanel.vue    # 优化面板
│       │   ├── MonitorDashboard.vue # 监控仪表盘
│       │   ├── BandwidthManager.vue # 带宽管理
│       │   ├── ServerConfigNew.vue  # 服务配置
│       │   └── HelpDoc.vue          # 帮助文档
│       └── locales/        # 国际化 (zh-CN / en-US)
└── .github/workflows/      # CI/CD 自动构建
```

---

## 🔧 智能优化引擎

面板内置智能优化引擎，根据服务器配置自动计算最优网络参数：

| 传输场景 | 适用内存 | 连接池 | TCP 缓冲 |
|----------|----------|--------|----------|
| 1 GB 轻量 | ≤ 512MB | 100 | 4MB |
| 5 GB 适中 | 1-2GB | 500 | 16MB |
| 10 GB 推荐 | 2-4GB | 1000 | 32MB |
| 50 GB 深度 | 4-8GB | 5000 | 128MB |
| 100 GB 极限 | ≥ 8GB | 10000 | 256MB |

优化项包括：TCP 缓冲区、连接池大小、SYN 队列、KeepAlive 参数、重试策略等。

---

## 🌐 管理面板 API

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/optimize/profile` | GET | 获取系统信息与优化推荐 |
| `/api/optimize/sysctl` | GET | 生成 sysctl 配置 |
| `/api/optimize/apply` | POST | 一键应用优化 |
| `/api/optimize/rollback` | POST | 回滚优化 |
| `/api/bandwidth/status` | GET | 带宽状态 |
| `/api/bandwidth/limit` | POST | 设置限速 |
| `/api/firewall/rules` | GET | 防火墙规则 |
| `/api/diagnostic/run` | POST | 运行诊断 |
| `/api/restart` | POST | 重启服务 |

---

## 📖 文档

详细使用文档：[doc.html](doc.html)

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

---

## 📜 许可证

MIT License - 详见 [LICENSE](LICENSE)

---

## ⭐ Star History

<a href="https://www.star-history.com/?type=date&repos=attychen%2Fimmich-frps">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/chart?repos=attychen/immich-frps&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/chart?repos=attychen/immich-frps&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/chart?repos=attychen/immich-frps&type=date&legend=top-left" />
 </picture>
</a>

<div align="center">

# immich-frps · frps-panel

**frp 内网穿透可视化管理面板 — 监控 · 优化 · 带宽管理 · 防火墙**

[![GitHub release](https://img.shields.io/github/v/release/attychen/immich-frps?include_prereleases)](https://github.com/attychen/immich-frps/releases)
[![GitHub downloads](https://img.shields.io/github/downloads/attychen/immich-frps/total)](https://github.com/attychen/immich-frps/releases)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Build](https://img.shields.io/github/actions/workflow/status/attychen/immich-frps/release.yml?branch=frps-panel)](https://github.com/attychen/immich-frps/actions)

</div>

---

## 📋 项目简介

`frps-panel` 是基于 [fatedier/frp](https://github.com/fatedier/frp) 和 [xxl6097/go-frp-panel](https://github.com/xxl6097/go-frp-panel) 二次开发的 **frp 可视化管理面板**，专为大文件传输（如 Immich 照片同步）深度优化。

提供 **frps 服务端** 和 **frpc 客户端** 的 Web 管理界面，支持多架构（Linux / Windows / macOS / FreeBSD / Android / 龙芯 / RISC-V），免配置文件，一键安装。

---

## ✨ 功能特性

### 🖥️ 核心功能
| 功能 | 说明 |
|------|------|
| 📊 **监控仪表盘** | 实时流量、连接数、CPU / 内存监控 |
| ⚡ **智能优化引擎** | 根据服务器配置自动计算最优 TCP 参数，5 级传输场景（1GB ~ 100GB） |
| 📡 **带宽管理器** | 全局 / 客户端限速、流量统计、每日配额 |
| 🛡️ **防火墙** | IP 黑白名单，可视化规则管理 |
| 🔧 **在线配置** | 无需配置文件，Web 界面完成所有配置 |
| 📋 **诊断工具** | 端口检测、网络诊断、性能分析 |
| 🌐 **国际化** | 中文 / English 切换 |

### 🚀 平台能力
| 功能 | 说明 |
|------|------|
| ✅ 跨平台 | Linux / Windows / macOS / FreeBSD / Android / 龙芯 / RISC-V / MIPS |
| ✅ 服务形式运行 | 支持 install / uninstall / start / stop / restart |
| ✅ 在线升级 | URL 升级 + 文件上传升级 |
| ✅ 多客户端 | frpc 可同时运行多个客户端配置 |
| ✅ 密钥内嵌 | 生成的 frpc 客户端二进制内嵌授权密钥 |
| ✅ 配置导入导出 | 用户配置一键备份恢复 |

---

## 🚀 各平台部署

> 详细文档请访问 **[使用手册](https://attychen.github.io/immich-frps/doc.html)**（支持中/English 切换）

### Linux

| 架构 | 适用设备 |
|------|----------|
| `amd64` | x86_64 服务器 / 虚拟机 |
| `arm64` | 树莓派 4/5、ARM 云服务器 |
| `arm` / `armhf` | 树莓派 3、老旧 ARM 设备 |
| `mips64` / `mips64le` | 龙芯 3A/3B |
| `mips` / `mipsle` | 路由器 (MT7621 等) |
| `riscv64` | 昉·星光、SiFive |
| `loong64` | 龙芯 3A6000 |

```bash
# 下载（替换 {arch} 为对应架构，如 amd64 / arm64）
wget https://github.com/attychen/immich-frps/releases/latest/download/acfrps_v0.69.1_attychen_linux_{arch}
chmod +x acfrps_v0.69.1_attychen_linux_{arch}
./acfrps_v0.69.1_attychen_linux_{arch} install
# 按提示输入端口、用户名、密码
```

### Windows

| 架构 | 适用设备 |
|------|----------|
| `amd64` | 绝大多数 Windows PC / Server |
| `arm64` | Surface Pro X、ARM 笔记本 |

```powershell
# PowerShell 管理员运行
# 下载 acfrps_v0.69.1_attychen_windows_amd64.exe
.\acfrps_v0.69.1_attychen_windows_amd64.exe install
```

### macOS

| 架构 | 适用设备 |
|------|----------|
| `amd64` | Intel Mac |
| `arm64` | Apple Silicon (M1/M2/M3) |

```bash
# 下载对应架构
chmod +x acfrps_v0.69.1_attychen_darwin_{arch}
./acfrps_v0.69.1_attychen_darwin_{arch} install
```

### FreeBSD

```bash
fetch https://github.com/attychen/immich-frps/releases/latest/download/acfrps_v0.69.1_attychen_freebsd_amd64
chmod +x acfrps_v0.69.1_attychen_freebsd_amd64
./acfrps_v0.69.1_attychen_freebsd_amd64 install
```

### Docker

```bash
docker pull acallsh/frps-panel:latest
docker run -d --name frps-panel --network host \
  -v /etc/frp:/etc/frp \
  acallsh/frps-panel:latest
```

> 📖 完整安装指南、配置参数说明、API 文档 → **[使用手册](https://attychen.github.io/immich-frps/doc.html)**

---

## 📞 联系方式

- **WeChat**: `attychen`
- **GitHub Issues**: [https://github.com/attychen/immich-frps/issues](https://github.com/attychen/immich-frps/issues)

---

##  项目结构

```
frps-panel/
├── cmd/
│   ├── frps/          # frps 服务端入口
│   ├── frpc/          # frpc 客户端入口
│   └── server/        # 面板服务入口
├── internal/
│   ├── frps/          # frps 核心逻辑
│   │   ├── api.go              # 主结构体 & 路由
│   │   ├── bandwidth.go        # 带宽控制器
│   │   ├── diagnostic_api.go   # 诊断 API
│   │   ├── firewall.go         # 防火墙
│   │   └── optimize_api.go     # 优化 API + 带宽 API
│   └── frpc/          # frpc 客户端逻辑
├── pkg/
│   ├── diagnostic/    # 诊断工具包
│   ├── optimizer/     # 智能优化引擎
│   ├── comm/          # 通用工具
│   └── utils/         # 工具函数
├── web/frps/src/      # Vue 3 前端
│   └── components/
│       ├── MonitorDashboard.vue  # 监控仪表盘
│       ├── OptimizePanel.vue     # 优化面板
│       ├── BandwidthManager.vue  # 带宽管理
│       ├── ServerConfigNew.vue   # 服务配置
│       └── HelpDoc.vue           # 帮助文档
├── Dockerfile         # 多架构 Docker 构建
└── .github/workflows/ # CI/CD 自动构建 16 架构
```

---

## 🔧 管理面板 API

| 端点 | 方法 | 说明 |
|------|------|------|
| `/api/optimize/profile` | GET | 系统信息 + 优化推荐 |
| `/api/optimize/sysctl` | GET | 生成 sysctl 配置 |
| `/api/optimize/apply` | POST | 应用优化 |
| `/api/optimize/rollback` | POST | 回滚优化 |
| `/api/bandwidth/global` | GET/POST | 全局带宽限制 |
| `/api/bandwidth/stats` | GET | 带宽统计 |
| `/api/bandwidth/client` | GET/POST | 客户端带宽限制 |
| `/api/firewall/mode` | GET/POST | 防火墙模式 |
| `/api/firewall/rules` | GET/POST | 防火墙规则 |
| `/api/firewall/check` | POST | IP 检查 |
| `/api/diagnostic/run` | POST | 运行诊断 |
| `/api/diagnostic/report` | GET | 诊断报告 |

---

## 🔗 相关项目

| 项目 | 说明 |
|------|------|
| [fatedier/frp](https://github.com/fatedier/frp) | frp 原始项目，核心代理引擎 |
| [xxl6097/go-frp-panel](https://github.com/xxl6097/go-frp-panel) | 原始面板项目，本项目的上游 |

---

## 📜 许可证

[MIT](./LICENSE) License

本项目基于 [xxl6097/go-frp-panel](https://github.com/xxl6097/go-frp-panel) 二次开发，核心 frp 引擎来自 [fatedier/frp](https://github.com/fatedier/frp)。

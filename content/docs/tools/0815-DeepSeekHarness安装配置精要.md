---
title: "DeepSeek Harness 安装配置精要"
date: 2026-08-15T09:00:00+08:00
description: "DeepSeek Harness（DSH）安装配置精要：环境、插件与部署要点。"
created: 2026-08-15
tags:
  - DSH
  - DeepSeek
  - DSH
  - MCP
  - WSL2
  - 插件
  - 飞书
  - AI 工具
  - 部署

draft: false
---


- **收录日期**：2026-08-15
- **标签**：#DSH #DeepSeek #Harness #MCP #WSL2 #插件 #飞书 #AI工具

## 核心内容（大白话归纳）

DeepSeek Harness（简称 **DSH**）是 DeepSeek 官方的 AI 代理/编排框架，本机跑在 **WSL2**（Ubuntu）上，`dsh web` 是 Web 操作界面。这份文档记录了把 DSH 完整跑起来的过程：**5 个 MCP 服务器 + 16 个社区插件 + 飞书 Bridge 常驻 + WSL 控制 Windows**。

一句话理解：**DSH 是你自己电脑上的一套"AI 工作台"**——能接 GitHub、浏览器自动化、跨会话记忆、文件系统沙箱，还能从飞书里发指令让它干活。

## 一、装好的 5 个 MCP 服务器

全部写在 `~/.dsh/profiles/web/cordis.patch.yml`（新增条目必须用 `- insert:` 包裹），模型侧工具名格式 `mcp__<serverName>__<工具名>`。

| 服务器 | 作用 | 关键点 |
|---|---|---|
| memory | 跨会话知识图谱记忆 | 包 `@modelcontextprotocol/server-memory`，9 工具可用 |
| github | GitHub 全操作（仓库/PR/Issue/搜索） | 官方 `github-mcp-server` v1.9.0 Go 二进制（npm 旧包已废弃）；需 `GITHUB_PERSONAL_ACCESS_TOKEN` |
| playwright | 浏览器自动化（导航/点击/截图） | 官方 `@playwright/mcp@latest`；chromium 下到 `~/dsh/.pw-browsers`；缺系统库用 `LD_LIBRARY_PATH` 指到 `~/dsh/.pw-libs` |
| thinking | 结构化逐步推理 | `@modelcontextprotocol/server-sequential-thinking`，思维步骤持久化在服务器内，不占对话上下文 |
| fs | 受限文件访问 | `@modelcontextprotocol/server-filesystem`，沙箱根 `/home/yangyang/dsh` |

## 二、装的 16 个社区插件（bundles）

统一命令：`dsh plugin --profile web add <包名>`（自动 reconcile 进 `dsh.profile.bundles`）。当前 17 项（含官方 dsh-base / dsh-web-app）。

| 插件 | 用途 | 坑点 |
|---|---|---|
| dshmarket | 可视化插件市场（主题切换） | ⚠️ npm 上 `dshmarket@0.0.1` 是空占位包，必须 `dshmarket@1.2.2` 才被激活 |
| dsh-side-panel | 右侧开发者面板（文件/历史/Git） | 插槽 `shell.overlay`，与其他插件不冲突 |
| @modusensus/dsh-mneme | 跨会话记忆（SQLite + Markdown 镜像） | 包名带 scope；本地语义搜索需 `pnpm-workspace.yaml` 开 `allowBuilds` |
| dsh-toolkit ×10 | 零依赖工具十件套（time/json/csv/regex…） | 不在 npm，用 `github:omdsh-dev/dsh-tool-*` git 安装 |
| dsh-vision-router | 纯文本模型视觉（免费 Qwen2.5-VL-72B） | 免 key，2 req/min/IP |
| dsh-open-in-vscode | 侧栏一键开 VS Code | WSL 场景自动走远程模式打开 Linux 目录 |

## 三、飞书 Bridge + systemd 常驻（重点）

```sh
pnpm dlx github:imetn/dsh-lark-bridge setup --project /home/yangyang/dsh
```

- 自动建独立 `lark` profile；飞书里发消息 → 路由到 DSH 执行，WebSocket 长连，**无需公网 webhook**。
- 用户级 systemd 常驻（无需 sudo），服务文件 `~/.config/systemd/user/dsh-lark.service`：
  - ⚠️ **坑**：systemd 环境没有 nvm 的 node PATH，必须在 service 里写 `Environment=PATH=...`，否则报 `/usr/bin/env: node: No such file or directory`（status 127）。
  - 启用：`systemctl --user enable --now dsh-lark`，WSL 重启后自动拉起。

## 四、WSL 控制 Windows（实测可用）

| 能力 | 命令 |
|---|---|
| 跑 PowerShell | `powershell.exe -NoProfile -Command "..."` |
| 看运行窗口 | `Get-Process \| Where-Object {$_.MainWindowTitle}` |
| 截屏 | PowerShell `CopyFromScreen` → `C:\temp\*.png`，WSL 经 `/mnt/c/temp/` 读取 |
| 开资源管理器 | `explorer.exe <path>` |
| 文件互通 | `/mnt/c/...` 与 `\\wsl.localhost\` 双向 |

> PowerShell 输出加 `[Console]::OutputEncoding=[Text.Encoding]::UTF8` 防乱码；WSL 访问 Windows 服务用宿主 IP 或 `.wslconfig` 开镜像网络。

## 五、关键踩坑清单

| 坑 | 现象/原因 | 解法 |
|---|---|---|
| 沙箱写权限 | bash 默认只能写 workspace，改 `~/.dsh` 等需提权 | `npm_config_cache` 指到 workspace 内绕过 |
| supply-chain 策略 | 新包被 `minimumReleaseAge` 拦截 | 显式指定版本，或加 `minimumReleaseAgeExclude` 白名单 |
| allowBuilds | pnpm v11 默认禁 build scripts | `pnpm-workspace.yaml` 放行 onnxruntime-node / protobufjs / sharp |
| HMR 热加载 | MCP 改 yml 自动重连；bundle UI 插件需重启 `dsh web` | 分层理解：配置热替换、UI 插件冷启动 |
| cordis.patch.yml | 直接写 `- id:` 报 `entry not found` | 必须用 `- insert:` 包裹新增条目 |
| 废弃包 | `server-github` / `server-playwright` 已下架 | 换官方 `@playwright/mcp` / `github-mcp-server` |

## 这条知识对我有什么用

你最近在 WSL2 上学 Linux、玩 AI 工具（Codex/DeepSeek、OpenClaw）、搞网络安全。DSH 是 DeepSeek 官方的"AI 代理操作系统"：把 GitHub / 浏览器 / 记忆 / 文件 / 飞书全接上后，可用飞书或 Web 界面指挥它干活，等于给自己配了常驻 AI 助手。这份文档是你**已经踩完坑的安装手册**，下次重装或加插件直接照抄命令。

---


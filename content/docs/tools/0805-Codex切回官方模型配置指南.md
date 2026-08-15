---
title: "Codex 切回官方模型配置指南"
date: 2026-08-05T09:00:00+08:00
description: "Codex 接入 DeepSeek 后如何干净切回官方模型：一键脚本回退操作速查。"
created: 2026-08-05
tags:
  - Codex
  - DeepSeek
  - AI工具
  - 配置管理
  - 工具与通用

draft: false
---

> **来源**：上官昊阳提供的 DeepSeek 官方一键配置脚本「回退」说明
> **定位**：用 DeepSeek 官方一键脚本把 Codex（OpenAI 官方 AI 编程代理）接成 DeepSeek 模型后，如何干净切回 Codex 官方模型。属于「AI 编程工具配置」类速查。

---

## 一、背景

DeepSeek 提供了一键脚本，把 Codex 的底层模型替换成 DeepSeek。脚本首次运行时会**自动备份原配置**到 `~/.codex/backup-deepseek/`。想回到官方模型，本质就是把这份备份还原回去（或重跑脚本让它自动还原）。

---

## 二、三种回退方法

| 方法 | 适用场景 | 难度 | 核心动作 |
|---|---|---|---|
| **① 重跑脚本选恢复项** | 大多数情况，最省心 | 低 | 再跑一次安装命令，菜单选第 3 项「恢复到安装前的默认配置」 |
| **② 手动恢复备份** | 脚本跑挂了 / 想自己掌控 | 中 | 把 `backup-deepseek/config.toml` 覆盖回 `~/.codex/` |
| **③ 第三方切换工具** | 本来就装了切换器 | 低 | `codex-safe-switch official` 或图形工具点「恢复官方」 |

### 方法一：再次运行一键脚本（最推荐）

脚本初次运行已自动备份原配置到 `~/.codex/backup-deepseek/`。重跑同一条命令，在菜单选 **第 3 项**「恢复到安装前的默认配置」即可。

- **macOS / Linux**：
  ```bash
  bash <(curl -fsSL https://cdn.deepseek.com/api-docs/codex-deepseek-setup.sh)
  ```
- **Windows (PowerShell)**：
  ```powershell
  irm https://cdn.deepseek.com/api-docs/codex-deepseek-setup-en.ps1 | iex
  ```

### 方法二：手动恢复配置文件（进阶）

1. **定位配置文件夹**（Codex 配置统一在用户目录的 `.codex`）：
   - macOS / Linux：`~/.codex/`
   - Windows：`C:\Users\<你的用户名>\.codex\`
2. **恢复备份**：
   - 找到 `~/.codex/backup-deepseek/` 文件夹；
   - 把里面的 `config.toml` **复制并覆盖**到 `~/.codex/` 目录下；
   - （可选）若改过 `models.json`，删掉或重命名它，Codex 会自动用内置官方模型列表。

### 方法三：第三方管理工具

- `codex-safe-switch`：命令行一键切换，`codex-safe-switch official` 直接切回官方 ChatGPT 配置；
- `CCSwitch` / `CodexSwitch`：图形化工具，界面里点「恢复官方登录」之类选项即可。

---

## 三、注意事项

- **重启才生效**：完成后**完全退出并重启** Codex 客户端（ChatGPT 桌面端、VS Code 插件等），否则新配置不加载。
- **默认模型**：恢复后 Codex 默认通常是 `o4-mini`；会话内用 `/model` 命令，或改 `config.toml` 的 `model` 字段，可换其他官方模型。



---

## 五、使用记录

（留空）

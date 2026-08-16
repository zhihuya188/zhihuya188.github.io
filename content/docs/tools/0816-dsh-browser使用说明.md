---
title: "dsh-browser 使用说明"
description: "用 DSH 操作你已登录的 Chrome 浏览器：DSH 插件 + Chrome 扩展的安装、使用与故障排查。"
date: 2026-08-16T10:00:00+08:00
draft: false
tags:
  - DSH
  - dsh-browser
  - Chrome
  - 浏览器
  - AI工具
categories:
  - 工具
---

> **用 DSH 操作你已经登录的 Chrome** —— 不是无头浏览器，是你正在用的那个浏览器。
> 登录态、Cookie、Session 全部保留。

---

## 1. 它是什么

`dsh-browser`（[Lum1104/dsh-browser](https://github.com/Lum1104/dsh-browser)）由两部分组成：

| 部分 | 作用 |
|---|---|
| **DSH 插件** `@deepseek-ai/dsh-bridge-browser` | 在 dsh 里注册 `browser_*` 工具，通过 WebSocket 桥接 Chrome |
| **Chrome 扩展**（MV3 侧边栏） | 挂在你的 Chrome 里，接收模型指令操作当前标签页 |

**核心差异**（对比之前的 Playwright MCP）：
- ✔️ 操作用户**已登录**的浏览器（保留登录态）
- ✔️ **文字优先**快照（页面→带编号的交互元素清单），模型按编号操作，省 token
- ❌ 不做无头抓取、不截图（截图不进模型管道）

---

## 2. 安装步骤（已完成）

> 以下为**本次已完成**的操作记录，重装时可参考。

### 2.1 安装 DSH 侧插件 + 构建扩展

```bash
curl -sL https://raw.githubusercontent.com/Lum1104/dsh-browser/main/scripts/install.sh -o install.sh
bash install.sh
```

脚本自动完成：
1. 从 GitHub 下载源码到 `~/.dsh/dsh-browser/`
2. 构建 bridge 插件，注册到 web profile（`link:` 安装）
3. 构建 Chrome 扩展到 `~/.dsh/browser-extension/`

> ⚠️ **macOS 专属段在 WSL 会失败**：脚本末尾用 `open`/`pbcopy` 打开 Chrome，WSL 没这些命令，报 `pbcopy: command not found`（exit 127）。**不影响前 3 步**，扩展已构建好。

### 2.2 浏览器侧加载扩展

扩展路径（构建产物，已同步到 Windows C 盘方便加载）：
```
C:\dsh-browser-extension
```

Chrome 加载步骤：
1. 地址栏输 `chrome://extensions`
2. 右上角打开 **开发者模式**
3. 点 **「加载已解压的扩展程序」** → 选 `C:\dsh-browser-extension`
4. 加载后点工具栏 **DeepSeek 鲸鱼图标** 打开侧边栏

> 侧边栏打开后，扩展会用 `~/.dsh/ext-bridge-token` 里的 token 自动连接 bridge（回环免 token，自动发现本机 dsh）。

---

## 3. 当前安装状态

| 组件 | 状态 | 路径 |
|---|---|---|
| dsh web | ✅ 运行中（PID 5057，需重启生效新装插件） | — |
| bridge 插件 | ✅ `@deepseek-ai/dsh-bridge-browser@0.0.1` | `~/.dsh/dsh-browser/packages/browser/bridge-browser` |
| bridge token | ✅ | `~/.dsh/ext-bridge-token`（0600） |
| Chrome 扩展 | ✅ v0.1.0 | `C:\dsh-browser-extension` / `~/.dsh/browser-extension` |
| 源码 | ✅ | `~/.dsh/dsh-browser/` |

**browser 工具集**（注册在 `ctx.tools`，聊天模型可用）：
`browser_snapshot` · `browser_click` · `browser_type` · `browser_press` · `browser_scroll` · `browser_navigate` / `browser_back` / `browser_forward` / `browser_reload` · `browser_get_text` · `browser_wait`

---

## 4. 使用方法

在 **Web GUI 新建聊天会话**，对模型用自然语言提需求：

| 你说 | 模型用 |
|---|---|
| "读取我当前浏览器页面的内容" | `browser_snapshot`（结构化文本 + 编号控件） |
| "帮我点击页面上第 3 个链接" | `browser_click`（按编号） |
| "在搜索框输入 xxx 并回车" | `browser_type` + `browser_press` |
| "往下滚动/回到顶部" | `browser_scroll` |
| "打开 https://example.com" | `browser_navigate` |
| "把页面上懒加载部分读出来" | `browser_get_text`（指定区域） |

**特点**：快照是文字（不耗截图 token），控件带编号，模型按编号定位操作，支持 React/Vue 界面。

---

## 5. 故障排查

| 现象 | 原因 / 处理 |
|---|---|
| `no browser extension is connected to the bridge` | Chrome 扩展没连上 bridge。①确认扩展已加载 ②点鲸鱼图标开侧边栏 ③确认 `~/.dsh/ext-bridge-token` 存在 |
| `pbcopy: command not found`（安装脚本） | 正常，WSL 无 macOS 命令，忽略 |
| 扩展列表没有 dsh-browser | 重新加载 `C:\dsh-browser-extension`（开发者模式） |
| 安装后模型无 `browser_*` 工具 | **重启 dsh web**（bundle 层插件需重启才加载） |

---

## 6. 相关命令 / 备忘

```bash
# 重新构建扩展（改过源码后）
cd ~/.dsh/dsh-browser && pnpm --filter dsh-browser-extension run build
# 同步到 Windows
rsync -a --delete ~/.dsh/browser-extension/ /mnt/c/dsh-browser-extension/

# token 位置（扩展认证用）
~/.dsh/ext-bridge-token

# 扩展产物
~/.dsh/browser-extension/   (开发者可访问)
C:\dsh-browser-extension    (供 Chrome 加载)
```

---

*生成时间：2026-08-16 · 环境：WSL2 · 版本：bridge 0.0.1 / extension 0.1.0*

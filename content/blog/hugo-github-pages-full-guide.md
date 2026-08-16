---
title: "Hugo + GitHub Pages 部署个人网页：从零到一的完整步骤"
date: 2026-08-16T12:00:00+08:00
draft: false
description: "一篇完整的 Hugo 个人博客搭建教程：本地安装、初始化、选主题、配置、推送到 GitHub Pages 自动部署，全程图解对照。"
tags:
  - Hugo
  - GitHub
  - GitHub Pages
  - 教程
categories:
  - 教程
---

想搭一个个人网站/博客？**Hugo + GitHub Pages** 是最省心的组合之一：本地写 Markdown，推送到 GitHub 就自动发布，**零服务器、零数据库、全免费**。本文带你从头走一遍完整流程。

{{< callout type="tip" >}}
**完整成果**：本文操作完成后，你会得到一个「写文章 → `git push` → 上线」的个人博客，域名形如 `https://你的用户名.github.io/`。
{{< /callout >}}

## 一、整体流程预览

{{< steps >}}
### 安装 Hugo
本地环境装好 Hugo（推荐 extended 版）。

### 新建站点 + 加主题
`hugo new site` 初始化，引入你喜欢的主题。

### 写文章、本地预览
用 Markdown 写博客，`hugo server` 实时预览。

### 创建 GitHub 仓库并推送
建 `用户名.github.io` 仓库，把代码推上去。

### 开启 GitHub Pages
选择部署源为 GitHub Actions，自动构建发布。

### 发布更新
以后每篇新文章，`git push` 即自动上线。
{{% /steps %}}

---

## 二、本地安装 Hugo

### 2.1 下载 Hugo

访问 [Hugo 官方发布页](https://github.com/gohugoio/hugo/releases)，下载对应系统的 **extended** 版本（主题通常需要 extended 支持）。

- **Windows**：下载 `.zip`，解压后把 `hugo.exe` 所在目录加入 PATH。
- **WSL/Linux**：下载 `.tar.gz`，解压到本地并使用。
- **macOS**：`brew install hugo`（含 extended）。

Linux/WSL 示意：

```bash
# 下载 hugo_extended（版本号按需替换）
wget https://github.com/gohugoio/hugo/releases/download/v0.160.1/hugo_extended_0.160.1_linux-amd64.tar.gz
tar xzf hugo_extended_0.160.1_linux-amd64.tar.gz
sudo mv hugo /usr/local/bin/
```

### 2.2 验证安装

```bash
hugo version
# 看到版本号即成功，如：hugo v0.160.1 ... extended
```

---

## 三、初始化站点 + 添加主题

### 3.1 新建站点

```bash
hugo new site my-site
cd my-site
```

生成的基本结构（`content/` 放 Markdown，`config.*` 是配置，`themes/` 放主题）。

### 3.2 添加主题

以本站的 **Hextra** 主题为例（用 git submodule 引入，方便后续更新）：

```bash
git init                                   # 初始化 git
git submodule add https://github.com/imfing/hextra.git themes/hextra
```

在配置文件中启用主题（以 `config.toml` 为例）：

```toml
baseURL = 'https://你的用户名.github.io/'
title = '我的站点'
theme = 'hextra'
```

> 不同主题配置不同，具体看主题的文档。

### 3.3 写第一篇文章

```bash
hugo new content/blog/my-first-post.md
```

编辑生成的 Markdown，正文用 Markdown 语法写，front matter 里 `draft: true` 表示草稿（发布前改为 `false`）。

---

## 四、本地预览

```bash
hugo server -D
```

`-D` 表示同时显示草稿。打开终端提示的地址（通常是 `http://localhost:1313`）即可实时预览，改文件会热更新。

{{< callout type="info" >}}
**检查清单**：确认文章能正常渲染、导航和主题样式正常，再进行下一步部署。
{{< /callout >}}

---

## 五、创建 GitHub 仓库并推送

### 5.1 建仓库

在 GitHub 新建一个**公开仓库**，名字必须是 **`你的用户名.github.io`**（GitHub Pages 固定要求）。

### 5.2 推送代码

```bash
git add -A
git commit -m "init site"
git remote add origin https://github.com/你的用户名/你的用户名.github.io.git
git push -u origin main
```

---

## 六、开启 GitHub Pages（自动部署）

### 6.1 配置 Pages

1. 进仓库 **Settings → Pages**
2. 在 **Build and deployment** → **Source** 选择 **GitHub Actions**

### 6.2 添加部署工作流

在仓库根目录创建 `.github/workflows/hugo.yml`，写入自动构建部署脚本。参考本站实际使用的完整工作流：

```yaml
name: Deploy Hugo site to Pages
on:
  push:
    branches: ["main"]
permissions:
  contents: read
  pages: write
  id-token: write
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive   # 拉取主题 submodule
      - name: Setup Pages
        uses: actions/configure-pages@v5
      - name: Install Hugo
        run: |
          wget https://github.com/gohugoio/hugo/releases/download/v0.160.1/hugo_extended_0.160.1_linux-amd64.deb
          sudo dpkg -i hugo_extended_0.160.1_linux-amd64.deb
      - name: Build
        run: hugo --gc --minify --baseURL "https://你的用户名.github.io/"
      - name: Upload
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public
  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
    steps:
      - name: Deploy
        uses: actions/deploy-pages@v4
```

推送后，GitHub Actions 会自动构建，几分钟内站点上线：`https://你的用户名.github.io/`。

{{< callout type="warning" >}}
**常见坑**：主题用 `git submodule` 时，工作流里 `actions/checkout` 必须加 `submodules: recursive`，否则构建时缺少主题而报错。
{{< /callout >}}

---

## 七、日常发布流程

至此大功告成。以后每次更新：

```bash
# 1. 新增/修改文章（Markdown）
hugo new content/blog/新文章.md

# 2. 本地预览确认
hugo server -D

# 3. 推送即发布
git add -A && git commit -m "add post" && git push origin main
```

推送到 `main` 分支后，GitHub Actions 自动构建部署，约 1~2 分钟后线上可见。

---

## 八、进阶建议

- **换主题**：换 `theme` 配置和 submodule 即可，主题风格差异巨大，多试几个。
- **绑定域名**：在 `config.toml` 改 `baseURL`，并在仓库 Settings 里配置自定义域名。
- **配置搜索/评论**：现代主题（如 Hextra）自带搜索、多语言、暗色模式等，去主题文档开启。
- **版本管理**：把主题作为 submodule 管理，升级用 `git submodule update --remote themes/xxx`。

{{< callout type="tip" >}}
**快速上手**：想省去手动配置，可以直接用 Hextra 官方模板 [hugo-theme-stack-starter](https://github.com/CaiJimmy/hugo-theme-stack-starter)，或参考[本站](https://zhihuya188.github.io)的实际结构。
{{< /callout >}}

---

## 小结

Hugo 的**极高构建速度** + GitHub Pages 的**免费托管** + GitHub Actions 的**免运维自动部署**，让个人博客的搭建和维护都极其简单。本文这个流程走通后，你剩下的工作就是——**专心多写几篇好文章**。

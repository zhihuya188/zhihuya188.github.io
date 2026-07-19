---
title: "把 Hugo 站点部署到 GitHub Pages"
date: 2026-07-19T11:00:00+08:00
draft: false
description: "用 GitHub Actions 实现推送即部署，再也不用手动上传。"
tags: ["部署", "GitHub Pages", "CI"]
categories: ["运维"]
---

这个示例站点已经配好了自动部署，原理很简单：

1. 把代码推送到 `main` 分支
2. GitHub Actions 自动拉取主题子模块（PaperMod）
3. 用 Hugo 构建出 `public/`
4. 发布到 GitHub Pages

## 工作流核心

文件在 `.github/workflows/hugo.yml`，关键两步：

```yaml
- name: Build with Hugo
  run: hugo --gc --minify --baseURL "${{ steps.pages.outputs.base_url }}/"

- name: Deploy to GitHub Pages
  uses: actions/deploy-pages@v4
```

`configure-pages` 会自动算出你的 Pages 地址并覆盖 `baseURL`，所以你**不需要手动改 `config.toml`**。

## 上线只需一步

在仓库 **Settings → Pages → Source** 里选择 **GitHub Actions**，剩下的交给 CI。

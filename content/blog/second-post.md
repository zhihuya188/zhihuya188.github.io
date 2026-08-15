---
title: "用 GitHub Pages 自动部署 Hugo"
aliases:
  - '/posts/second-post/'
date: 2026-07-19T11:30:00+08:00
draft: false
description: "介绍如何通过 GitHub Actions 把 Hugo 站点自动部署到 GitHub Pages。"
tags: ["部署", "GitHub Pages", "CI"]
categories: ["示例"]
---

把 Hugo 站点部署到 GitHub Pages，最省心的方式是用 **GitHub Actions** 自动构建。

## 原理

每次你 `git push` 到 `main` 分支，GitHub 上的工作流就会：

1. 拉取代码（含主题子模块）
2. 安装 Hugo
3. 运行 `hugo --gc --minify` 生成静态文件
4. 把 `public/` 发布到 GitHub Pages

## 关键配置

仓库需要开启 **Settings → Pages → Source = GitHub Actions**。

工作流文件位于 `.github/workflows/hugo.yml`，本示例站点已经为你准备好了。

## 本地预览

```bash
hugo server -D
```

打开提示的 `http://localhost:1313` 即可实时预览。

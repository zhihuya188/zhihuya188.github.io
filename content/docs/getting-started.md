---
title: 快速上手
weight: 1
---

## 技术栈

本站基于 **Hugo + Hextra 主题** 构建，托管在 GitHub Pages，推送 `main` 分支即自动部署。

| 组件 | 说明 |
|------|------|
| Hugo | v0.160.1 (extended)，本地二进制在 `.tools/hugo` |
| 主题 | Hextra（submodule，`themes/hextra`） |
| 部署 | GitHub Actions（`.github/workflows/hugo.yml`），无需 Node |
| 托管 | GitHub Pages（自动，无需手动操作） |

## 本地预览

```bash
cd /home/yangyang/hugo_zhihuya188
.tools/hugo server -D
```

打开 `http://localhost:1313` 即可实时预览。`-D` 表示同时显示草稿。

## 写一篇文章

```bash
# 1. 新建文章（在 content/blog/ 下生成 Markdown）
.tools/hugo new blog/我的文章.md

# 2. 编辑：写好标题、日期、标签，正文用 Markdown
# 3. 把 front matter 里的 draft 改为 false（或删除该行）
# 4. 预览确认无误后发布
git add -A && git commit -m "新增文章" && git push origin main
```

推送到 `main` 后，GitHub Actions 自动构建部署，约 1~2 分钟后在 `https://zhihuya188.github.io` 可见。

## 文章 front matter 模板

```yaml
---
title: "文章标题"
date: 2026-08-15T10:00:00+08:00
draft: false
description: "一句话描述，用于列表页与 SEO。"
tags:
  - 标签一
  - 标签二
categories:
  - 分类名
---
```

> 注意：`date` 不要设置为未来时间，否则 Hugo 会当作"未发布内容"跳过。

## 发布流程（三件事）

1. **写**：Markdown 写正文，可配合短代码（见[短代码速查](/docs/shortcodes/)）
2. **验**：`.tools/hugo server -D` 本地预览
3. **发**：`git push origin main` 自动部署

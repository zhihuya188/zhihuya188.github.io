---
title: 内容组织
weight: 2
---

## 目录结构

Hugo 按 `content/` 目录结构生成站点 URL：

```text
content
├── _index.md          // 首页（hextra-home 布局）
├── blog/              // 文章区 → /blog/
│   ├── _index.md      // 文章列表页
│   └── xxx.md         // 单篇文章 → /blog/xxx/
├── docs/              // 文档区 → /docs/（带侧边栏）
│   ├── _index.md      // 文档首页
│   └── *.md           // 文档页面
├── archives/          // 归档 → /archives/
├── tags/              // 标签 → /tags/
└── about.md           // 关于 → /about/
```

每个 `_index.md` 是对应 section 的索引页；其余 Markdown 是普通页面。

## 三种内容布局

| 布局 | 目录 | 特点 |
|------|------|------|
| `docs` | `content/docs/` | 结构化文档，带左侧目录导航 |
| `blog` | `content/blog/` | 博客文章，卡片式列表 + 单篇视图 |
| `default` | 其他目录 | 单页文章，无侧边栏 |

## 文章 URL 规则

- 文章 URL 由**文件名**决定（不配置 permalinks，保持默认）：`content/blog/deploy-to-github-pages.md` → `/blog/deploy-to-github-pages/`
- 迁移文章目录时，用 front matter 的 `aliases` 保留旧链接：

```yaml
---
title: "文章"
aliases:
  - '/posts/旧路径/'
---
```

Hugo 会自动为旧路径生成跳转页面，外链不失效。

## 图片管理（三种方式）

1. **文章同目录**：图片与 md 放一起，正文用 `![](图片.png)`
2. **Page Bundle**：`文章名/index.md` + 图片放同一目录
3. **static 目录**：`static/images/` 全局图片，用 `/images/xxx.png` 引用

## 面包屑

- 文档页和文章页自动显示面包屑（如 `首页 > 文章 > 标题`）
- 可通过 front matter `breadcrumbs: false` 关闭
- section 级统一配置用 `_index.md` 的 `cascade`：

```yaml
---
title: 文章
cascade:
    params:
        breadcrumbs: true
---
```

## 排序

侧边栏/列表默认按文件名字母序；用 `weight` 自定义顺序（数值小的在前）。

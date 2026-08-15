---
title: "我的第一篇文章"
aliases:
  - '/posts/first-post/'
date: 2026-07-19T10:00:00+08:00
draft: false
description: "这是用 Hugo 写的第一篇示例文章，演示基本写作与代码块。"
tags:
  - Hugo
  - 教程
categories:
  - 示例
---

欢迎！这是 **Hugo** 站点里的第一篇文章。

## 为什么用 Hugo

- 极速的静态站点生成器（Go 编写）
- 用 Markdown 写作，专注内容
- 主题生态丰富（比如本站的 PaperMod）

## 一个代码块示例

```python
def greet(name: str) -> str:
    return f"Hello, {name}! from Hugo"

print(greet("World"))
```

> 小贴士：文章的 `draft` 字段设为 `false` 才会被发布到线上。

## 列表与引用

1. 写一篇 Markdown
2. 运行 `hugo` 生成静态文件
3. 推送到 GitHub，自动部署

就这么简单。

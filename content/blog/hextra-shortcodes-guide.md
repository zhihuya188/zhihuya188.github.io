---
title: "Hextra 短代码快速上手：让文章更生动"
date: 2026-08-14T18:00:00+08:00
draft: false
description: "演示 Hextra 主题提供的常用短代码：Callout、Tabs、Details、Cards、Steps、Icon、FileTree 等，一篇看懂用法。"
tags:
  - Hugo
  - Hextra
  - 教程
categories:
  - 教程
---

Hextra 主题内置了一批精美的短代码，在文章中用上它们，可以让内容更有层次感。本文演示常用几个，直接复制即可使用。

## 提示框（Callout）

{{< callout type="info" >}}
**提示**：Callout 支持 `info`、`tip`、`warning`、`error`、`important` 五种类型。
{{< /callout >}}

{{< callout type="warning" >}}
**注意**：写文章时记得把 `draft` 改成 `false`，否则不会发布。
{{< /callout >}}

{{< callout type="tip" >}}
**技巧**：`Ctrl+K` 可以快速调出站内搜索。
{{< /callout >}}

## 标签页（Tabs）

用 Tabs 可以在同一位置切换不同内容，适合展示多方案的对比：

{{< tabs >}}
  {{< tab name="Hugo Modules" >}}通过 `go.mod` 引入主题，自动拉取更新，官方推荐。{{< /tab >}}
  {{< tab name="Git Submodule" >}}用 `git submodule add` 固定主题版本，仓库管理更直观。{{< /tab >}}
  {{< tab name="直接复制" >}}把主题文件放进 `themes/` 目录，最简单但更新麻烦。{{< /tab >}}
{{< /tabs >}}

## 折叠面板（Details）

{{< details title="点击展开：查看示例代码" closed="true" >}}

```bash
git submodule add https://github.com/imfing/hextra.git themes/hextra
echo 'theme = "hextra"' >> hugo.toml
hugo server -D
```

{{< /details >}}

## 卡片（Cards）

{{< cards >}}
  {{< card link="/blog/" title="文章列表" icon="book-open" >}}
  {{< card link="/archives/" title="归档" icon="archive" >}}
  {{< card link="/about/" title="关于我" icon="badge-check" >}}
{{< /cards >}}

## 步骤（Steps）

{{% steps %}}
### 写文章
在 `content/blog/` 下新建 Markdown 文件，写好 front matter 和正文。

### 本地预览
运行 `hugo server -D`，打开 `localhost:1313` 实时查看效果。

### 推送发布
`git push` 到 main 分支，GitHub Actions 自动构建部署，约 1~2 分钟生效。
{{% /steps %}}

## 图标（Icon）

内联图标可以直接写在文字中：{{< icon name="github" >}} GitHub、{{< icon name="warning" >}} 警告、{{< icon name="badge-check" >}} 徽章、{{< icon name="arrow-right" >}} 箭头。

## 文件树（FileTree）

用文件树展示目录结构，特别适合技术文章：

{{< filetree/container >}}
  {{< filetree/folder name="content" state="open" >}}
    {{< filetree/file name="_index.md" >}}
    {{< filetree/folder name="blog" state="open" >}}
      {{< filetree/file name="post-1.md" >}}
      {{< filetree/file name="post-2.md" >}}
    {{< /filetree/folder >}}
    {{< filetree/file name="about.md" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

## 徽章（Badge）

{{< badge "New" >}} 徽章适合标记新内容、实验特性等，也可以配合卡片使用。支持自定义颜色：{{< badge content="重要" color="red" >}}、带图标：{{< badge content="提示" icon="sparkles" >}}。

## 小结

短代码的完整列表见 [Hextra 官方文档](https://imfing.github.io/hextra/docs/guide/shortcodes/)，以上这些已经覆盖日常写作 90% 的需求。把它们组合起来，你的文章会立刻专业很多。

---
title: 短代码速查
weight: 4
---

Hextra 内置一批精美短代码，写文章时直接引用即可。

## 提示框（Callout）

```markdown
{{</* callout type="info" */>}}
**提示**：支持 info / tip / warning / error / important 五种类型。
{{</* /callout */>}}
```

{{< callout type="info" >}}
**提示**：支持 info / tip / warning / error / important 五种类型。
{{< /callout >}}

## 标签页（Tabs）

```markdown
{{</* tabs */>}}
  {{</* tab name="方案A" */>}}内容 A{{</* /tab */>}}
  {{</* tab name="方案B" */>}}内容 B{{</* /tab */>}}
{{</* /tabs */>}}
```

{{< tabs >}}
  {{< tab name="方案A" >}}内容 A{{< /tab >}}
  {{< tab name="方案B" >}}内容 B{{< /tab >}}
{{< /tabs >}}

## 折叠面板（Details）

```markdown
{{</* details title="点击展开" closed="true" */>}}
被隐藏的内容，支持 **Markdown**。
{{</* /details */>}}
```

{{< details title="点击展开" closed="true" >}}
被隐藏的内容，支持 **Markdown**。
{{< /details >}}

## 卡片（Cards）

```markdown
{{</* cards */>}}
  {{</* card link="/blog/" title="文章" icon="book-open" */>}}
  {{</* card link="/tags/" title="标签" icon="tag" */>}}
{{</* /cards */>}}
```

{{< cards >}}
  {{< card link="/blog/" title="文章" icon="book-open" >}}
  {{< card link="/tags/" title="标签" icon="tag" >}}
{{< /cards >}}

## 步骤（Steps）

```markdown
{{%/* steps */%}}
### 第一步
内容
### 第二步
内容
{{%/* /steps */%}}
```

{{% steps %}}
### 第一步
写 Markdown
### 第二步
推送到 main 自动部署
{{% /steps %}}

## 图标（Icon）

```markdown
{{</* icon name="github" */>}} {{</* icon name="warning" */>}}
```

图标名见主题的 `data/icons.yaml`（github、warning、book-open、archive、tag、sparkles 等）。

## 文件树（FileTree）

```markdown
{{</* filetree/container */>}}
  {{</* filetree/folder name="content" state="open" */>}}
    {{</* filetree/file name="blog" */>}}
    {{</* filetree/file name="docs" */>}}
  {{</* /filetree/folder */>}}
{{</* /filetree/container */>}}
```

{{< filetree/container >}}
  {{< filetree/folder name="content" state="open" >}}
    {{< filetree/file name="blog" >}}
    {{< filetree/file name="docs" >}}
  {{< /filetree/folder >}}
{{< /filetree/container >}}

## 徽章（Badge）

```markdown
{{</* badge "New" */>}}（自闭合，无结束标签）
{{</* badge content="重要" color="red" */>}}
```

{{< badge "New" >}} {{< badge content="重要" color="red" >}}

## 首页组件（hextra-home）

首页 `content/_index.md` 用 `layout: hextra-home`，配合 hero 系列短代码：

```markdown
---
title: '我的站点'
layout: hextra-home
---

{{</* hextra/hero-headline */>}}
  欢迎光临
{{</* /hextra/hero-headline */>}}

{{</* hextra/hero-subtitle */>}}
  一句话介绍
{{</* /hextra/hero-subtitle */>}}

{{</* hextra/hero-button text="阅读文章" link="blog" */>}}
```

完整列表见 [Hextra 官方短代码文档](https://imfing.github.io/hextra/docs/guide/shortcodes/)。

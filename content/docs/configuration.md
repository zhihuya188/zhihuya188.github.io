---
title: 站点配置
weight: 3
---

站点主配置在仓库根目录的 `config.toml`。以下是本站已启用的配置说明。

## 基础配置

```toml
baseURL = 'https://zhihuya188.github.io/'
title = '我的 Hugo 站点'
theme = 'hextra'

# 中文站点
defaultContentLanguage = 'zh-cn'
hasCJKLanguage = true

# 用 Git 提交时间作为文章"最后修改"来源
enableGitInfo = true
```

## Markdown 渲染

```toml
[markup]
    # 允许裸 HTML
    [markup.goldmark]
        [markup.goldmark.renderer]
            unsafe = true
    # Hextra 语法高亮
    [markup.highlight]
        noClasses = false
```

## 导航菜单

```toml
[menu]
    [[menu.main]]
        name = '文章'
        pageRef = '/blog'
        weight = 1
    # ... 归档 / 标签 / 关于 ...
    [[menu.main]]
        name = '搜索'
        weight = 5
        [menu.main.params]
            type = 'search'
    [[menu.main]]
        name = 'GitHub'
        url = 'https://github.com/zhihuya188'
        weight = 6
        [menu.main.params]
            icon = 'github'
```

菜单项类型：
- `pageRef`：站内页面
- `url`：外部链接
- `type: search`：搜索按钮
- `icon`：图标（如 github、rss）

> ⚠️ TOML 语法注意：菜单项的 `params` 必须用 `[menu.main.params]` 嵌套表写法，不能写 `params: {…}`。

## 主题参数

```toml
[params]
    # 站点描述（首页 SEO meta）
    description = '洋洋的个人博客：记录技术笔记、AI 资讯与生活随笔。'

    # 文章页显示最后修改日期与作者
    displayUpdatedDate = true
    displayUpdatedAuthor = true
    dateFormat = '2006-01-02'

    # 外部链接加箭头装饰
    externalLinkDecoration = true

    # 主题模式：跟随系统 + 手动切换
    [params.theme]
        default = 'system'
        displayToggle = true

    # 文章/目录区显示标签
    [params.blog]
        [params.blog.list]
            displayTags = true
    [params.toc]
        displayTags = true
```

> ⚠️ TOML 归属陷阱：`displayUpdatedDate` 等**顶层 params 键**必须写在第一个子表（如 `[params.navbar]`）**之前**，否则会被归入上一个子表而不生效。

## 输出格式

```toml
[outputs]
    home = ['html', 'llms']     # 额外生成 llms.txt，供 AI 工具引用
    page = ['html']
    section = ['html', 'rss']
```

## 自定义主题色

在 `assets/css/custom.css` 中用 HSL 变量覆盖主色：

```css
:root {
  --primary-hue: 262deg;        /* 靛紫色 */
  --primary-saturation: 84%;
  --primary-lightness: 55%;
}
```

## 页脚版权

在 `i18n/zh-cn.yaml` 中覆盖：

```yaml
copyright: "© 2026 洋洋 · 用 Hugo & Hextra 构建"
```

更多配置项见 [Hextra 官方配置文档](https://imfing.github.io/hextra/docs/guide/configuration/)。

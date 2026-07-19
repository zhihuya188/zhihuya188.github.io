---
title: "hugo部署笔记"
date: 2019-11-03T19:01:58+08:00
description: "hugo部署笔记"
draft: false
tags: ["hugo", "blog"]
categories: ["blog"]
---

> Hugo 官方的定义是：
>
> Hugo is a fast and modern static site generator written in Go, and designed to make website creation fun again.(Hugo 是使用 Go 编写的快速而现代的静态站点生成器，旨在使网站创建变得有趣。)

<!--more-->

## 安装 Hugo

### Windows 安装 Hugo

在 https://github.com/gohugoio/hugo/releases/tag/v0.59.1 下载二进制包安装

安装完成后(配置环境变量)，在命令行输入以下命令验证安装：
```
hugo version
```

### 建立 Hugo 项目

一个 Hugo 项目就是一个站点，创建命令如下：

```
hugo new site [project-name]
```

例如我的站点名称是 blog,创建命令如下：

```
hugo new site blog
```

创建完成后，在 blog 文件夹下会生成以下文件结构：

```
.
├── archetypes # 存放生成博客的模版
├── assets # 存放被 Hugo Pipes 处理的文件
├── config # 存放 hugo 配置文件 支持 JSON YAML TOML 三种格式配置文件
├── content # 存放 markdown 文件
├── data # 存放 Hugo 处理的数据
├── layouts # 存放布局文件
├── static # 存放静态文件 图片 CSS JS文件
└── themes # 存放主题

```

### 添加主题

进入根目录，克隆主题文件就是安装主题。

为了快速搭建博客，可以使用主题。使用主题后，只需要向 content 文件夹添加 Markdown 文件即可。

Hugo 有主题市场 https://themes.gohugo.io/ ，我选择了`even`：

进入根目录，克隆主题文件就是安装主题。
```
cd blog
git clone https://github.com/olOwOlo/hugo-theme-even themes/even
```

### 添加新博客
添加新博客命令比较简单，命令如下：
```
hugo new post/my-first-blog.md
```

`hugo-theme-even` 的模版文件 `blog/themes/even/archetypes/default.md` 比较复杂，新建 `blog/archetypes/default.md` 文件覆盖掉即可。

我的模板配置如下：

```
---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
date: {{ .Date }}
description: ""
draft: true
tags: []
categories: []
---

<!--more-->

```

> draft 参数控制网站上该页面是否显示。设置为 false 或者去掉该参数才显示。<!--more--> 之前的内容会自动作为页面摘要。

### 打包
为了部署到线上，需要将 Markdown 文件打包成 HTML 文件。打包命令如下，even 是主题名：

```
hugo -t even

```

### 部署到 Github Pages
打包之后就是纯 HTML 文件，理论上所有支持部署静态页面的网站都是支持的。

我的部署命令如下，更多部署方式查看 https://gohugo.io/hosting-and-deployment/
```
#!/bin/bash
# 部署到 github pages 脚本
# 错误时终止脚本
set -e

# 删除打包文件夹
rm -rf public

# 打包。even 是主题
hugo -t even # if using a theme, replace with `hugo -t <YOURTHEME>`

# 进入打包文件夹
cd public

# Add changes to git.

git init
git add -A

# Commit changes.
msg="building site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# 推送到githu  
# nusr.github.io 只能使用 master分支
git push -f git@zhihu.github.com:zhihuya188/zhihuya188.github.io.git master

# 回到原文件夹
cd ..
```

## 参考

[Hugo + Github Pages 搭建个人博客](https://www.cnblogs.com/stevexu/p/10779375.html)
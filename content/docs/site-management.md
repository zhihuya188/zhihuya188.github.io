---
title: 站点管理
weight: 5
---

本站的日常管理指南：本地工具、分支结构、发布与故障排查。

## 本地工具

| 工具 | 路径 | 说明 |
|------|------|------|
| Hugo 二进制 | `.tools/hugo` | v0.160.1 extended，与线上一致（已 gitignore） |
| Git | 系统自带 | 版本管理 |

`.tools/hugo` 丢失时重新下载：

```bash
mkdir -p .tools && cd .tools
wget -q https://github.com/gohugoio/hugo/releases/download/v0.160.1/hugo_extended_0.160.1_linux-amd64.tar.gz -O hugo.tar.gz
tar xzf hugo.tar.gz && rm hugo.tar.gz
```

## 分支结构

| 分支 | 内容 | 说明 |
|------|------|------|
| `main` | Hugo 源码 | **日常管理唯一分支**，推送即部署 |
| `master` | 2019 旧构建产物 | 已弃用，保留作备份 |

## 日常命令速查

```bash
.tools/hugo server -D              # 本地预览（含草稿）
.tools/hugo new blog/xxx.md        # 新建文章
.tools/hugo --gc --minify          # 手动构建到 public/
git pull origin main               # 同步
git add -A && git commit -m "msg"  # 提交
git push origin main               # 发布（触发自动部署）
```

## 部署机制

- 工作流：`.github/workflows/hugo.yml`
- 触发：推送 `main` 分支，或 Actions 页面手动运行
- 构建命令：`hugo --gc --minify`（与本地一致）
- 要求仓库 **Settings → Pages → Source = GitHub Actions**

## 更新主题

```bash
git submodule update --remote themes/hextra
git add themes/hextra
git commit -m "update hextra"
git push origin main
```

## 常见操作

- **改站点名/描述**：`config.toml` 的 `title` 与 `params.description`
- **改导航**：`config.toml` 的 `[menu]`
- **改主题色**：`assets/css/custom.css`
- **改版权**：`i18n/zh-cn.yaml`
- **删除文章**：删除 `content/blog/` 下对应 md 后提交推送

## 故障排查

| 现象 | 处理 |
|------|------|
| 推送后站点没更新 | 打开仓库 Actions 页看构建日志 |
| 构建报错 | 本地跑 `.tools/hugo` 复现，多为 Markdown/front matter 问题 |
| 文章不显示 | 检查 `draft` 是否为 false；`date` 是否为未来时间 |
| 页面 404 | 确认文件 push 到了 `main` 分支 |
| 本地与线上不一致 | 确认 `.tools/hugo` 版本与 workflow 的 `HUGO_VERSION` 一致 |

## 提交代码前的检查清单

1. 文章 `draft: false`
2. `date` 不是未来时间
3. front matter 格式规范（tags/categories 用 block 写法）
4. 本地 `.tools/hugo` 构建无报错
5. 图片路径正确（优先相对路径）

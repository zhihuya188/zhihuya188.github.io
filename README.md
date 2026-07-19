# 我的 Hugo 站点

基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题构建，已配置 **GitHub Pages 自动部署**。

## 本地预览

```bash
# 使用与线上版本一致的 Hugo（推荐）
.tools/hugo server -D

# 或使用你系统里已安装的 hugo
hugo server -D
```

打开终端提示的 `http://localhost:1313` 即可实时预览。

## 目录结构

| 路径 | 说明 |
|------|------|
| `config.toml` | 站点配置（标题、菜单、社交链接等） |
| `content/posts/` | 文章目录（Markdown） |
| `content/about.md` | 关于页 |
| `themes/PaperMod/` | 主题（git submodule） |
| `.github/workflows/hugo.yml` | GitHub Pages 自动部署工作流 |

## 部署到 GitHub Pages

1. 在 GitHub 新建一个**公开**仓库（例如 `my-hugo-site`）。
2. 把本地代码推上去：
   ```bash
   git remote add origin https://github.com/<你的用户名>/<仓库名>.git
   git push -u origin main
   ```
3. 进入仓库 **Settings → Pages → Source**，选择 **GitHub Actions**。
4. 等 Actions 跑完，站点就会发布在：
   `https://<你的用户名>.github.io/<仓库名>/`

> 工作流里的 `configure-pages` 会自动把 `baseURL` 改成你的 Pages 地址，无需手动改 `config.toml`。

## 写一篇新文章

```bash
hugo new posts/my-post.md
```

编辑生成的 Markdown，写完后把 `draft: false` 即可发布。

## 常见自定义

- **站点名 / 描述**：改 `config.toml` 顶部的 `title` 与 `params.description`
- **页脚社交图标**：改 `[params.socialIcons]` 段
- **顶部菜单**：改 `[menu]` 段
- **换主题**：把 `themes/PaperMod` 换成别的主题，并改 `theme =` 字段

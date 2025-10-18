---
date: '2025-10-18T13:13:23+08:00'
draft: false
title: '基于 hugo-theme-stack-starter 快速搭建 Hugo 博客'
description: '使用 hugo-theme-stack-starter 模板快速搭建个人博客并部署到 GitHub Pages 的完整指南'
tags: 
  - 'Hugo'
  - 'GitHub Pages'
  - '博客'
categories: 
  - '技术笔记'
slug: "hugo-blog-setup-guide"
---

> **声明**：本文由 AI 助手（GitHub Copilot）根据个人博客搭建经验编写，主要用于记录和分享基于 hugo-theme-stack-starter 快速搭建博客的技术流程。

本文记录了使用 hugo-theme-stack-starter 模板快速搭建个人博客并部署到 GitHub Pages 的完整流程，包括模板使用、配置定制、内容创建和自动化部署等内容。

## 1. 基于 hugo-theme-stack-starter 创建项目

使用 [hugo-theme-stack-starter](https://github.com/CaiJimmy/hugo-theme-stack-starter) 模板创建项目，这是最快速的方式：

### 方法一：使用 GitHub Template + Codespace（推荐）

1. 访问 [hugo-theme-stack-starter](https://github.com/CaiJimmy/hugo-theme-stack-starter)
2. 点击 "Use this template" -> "Create a new repository"
3. 将仓库命名为 `<用户名>.github.io`
4. 创建完成后，点击 "Create codespace" 创建 GitHub Codespace
5. Codespace 已预装 Hugo extended，直接运行 `hugo server` 即可预览

### 方法二：本地开发

如果不想使用 Codespace，可以克隆到本地开发。**需要确保本地已安装 Git、Go 和 Hugo extended**。

```bash
git clone https://github.com/<用户名>/<用户名>.github.io.git
cd <用户名>.github.io
```

**注意**：hugo-theme-stack-starter 使用 Hugo Modules 功能加载主题，不再使用传统的子模块方式。

## 2. 配置网站

hugo-theme-stack-starter 已经包含了完整的配置结构，主要配置文件位于 `config/_default/` 目录：

### 主要配置文件

- `config.toml` - 基础站点配置
- `params.toml` - 主题参数配置  
- `menu.toml` - 菜单配置
- `languages.toml` - 多语言配置

编辑 `config/_default/config.toml`：

```toml
baseurl = "https://<用户名>.github.io"
languageCode = "zh-cn"
title = "个人博客"
defaultContentLanguage = "zh-cn"
hasCJKLanguage = true
```

### 自定义配置

根据需要修改 `config/_default/params.toml` 中的参数，如：

- 网站描述、关键词
- 社交媒体链接
- 评论系统配置
- 搜索功能等

## 3. 创建第一篇文章

stack-starter 已经包含了示例文章，可以参考 `content/post/` 目录下的示例。

```bash
# 使用 Hugo 命令创建新文章
hugo new post/my-first-post/index.md
```

Stack 主题支持 Page Bundle 结构，推荐使用：

```
content/post/my-first-post/
├── index.md        # 文章内容
├── cover.jpg       # 封面图片
└── images/         # 文章图片
    └── example.png
```

文章前置元数据示例：

```yaml
+++
title = "文章标题"
description = "文章简短描述"
date = 2025-10-18T10:00:00+08:00
image = "cover.jpg"
math = false
license = false
hidden = false
comments = true
draft = false
tags = ["标签1", "标签2"]
categories = ["分类"]
+++
```

## 4. 本地预览

```bash
# 不包含草稿
hugo server

# 包含草稿
hugo server -D
```

访问 http://localhost:1313 查看网站。

## 5. 部署到 GitHub Pages

### 5.1 GitHub 仓库配置

如果使用 GitHub Template 创建，仓库已经存在。如果是手动创建，需要：

1. 创建名为 `<用户名>.github.io` 的仓库，例如 `xmimu.github.io`
2. 将本地项目推送到 GitHub

### 5.2 配置 GitHub Pages

**重要**：需要将构建分支从 `master` 改为 `gh-pages`：

1. 在 GitHub 仓库中，点击 Settings -> Pages
2. Source 选择 "Deploy from a branch"
3. Branch 选择 "gh-pages" / "/ (root)"

### 5.3 GitHub Actions 自动部署

hugo-theme-stack-starter 已经包含了完整的 GitHub Actions 配置，包括：

- 自动构建和部署到 `gh-pages` 分支
- 每日自动更新主题的定时任务

### 5.4 推送代码

```bash
# 提交并推送代码
git add .
git commit -m "初始化博客"
git push origin master
```

### 5.5 自动部署流程

推送到 `master` 分支后：
1. GitHub Actions 自动触发
2. 构建 Hugo 站点
3. 部署到 `gh-pages` 分支
4. GitHub Pages 自动发布
5. 通过 `https://<用户名>.github.io` 访问

## 6. 常见问题解决

### 6.1 主题更新

hugo-theme-stack-starter 使用 Hugo Modules，更新主题方式：

```bash
# 手动更新主题
hugo mod get -u github.com/CaiJimmy/hugo-theme-stack/v3
hugo mod tidy
```

**注意**：当前配置使用 `v3` 版本。如果发布 `v4` 或更高版本，需要手动修改 `config/module.toml` 文件。

### 6.2 Hugo 无法显示内容

- 检查文章的 `draft` 状态是否为 `false`
- 使用 `hugo server -D` 命令可以预览草稿内容
- 确认配置文件中的路径设置正确

### 6.3 GitHub Pages 配置问题

- 确保 Pages 设置中 Branch 选择的是 `gh-pages`
- 检查 Actions 日志查看具体错误
- 确认 Hugo 版本兼容性

### 6.4 中文字符问题

确保 `config/_default/config.toml` 中设置了：

```toml
languageCode = "zh-cn"
defaultContentLanguage = "zh-cn"
hasCJKLanguage = true
```

## 7. 最佳实践

### 7.1 文章组织

Stack 主题推荐使用 Page Bundle 结构：

```
content/post/
└── my-article/
    ├── index.md      # 文章内容
    ├── cover.jpg     # 封面图片
    └── images/       # 其他图片
        └── diagram.png
```

### 7.2 配置管理

hugo-theme-stack-starter 提供了模块化配置：

- `config/_default/` - 默认配置
- `config/production/` - 生产环境配置
- `config/development/` - 开发环境配置

### 7.3 内容管理

- 使用 `categories` 和 `tags` 组织文章
- 利用 `image` 字段设置文章封面
- 合理使用 `description` 提供文章摘要

### 7.4 主题定制

- 自定义样式：修改 `assets/scss/custom.scss`
- 自定义布局：在 `layouts/` 目录下覆盖主题模板
- 添加短代码：在 `layouts/shortcodes/` 目录下创建

### 7.5 其他托管平台部署

如果要部署到其他静态托管服务（如 Vercel），需要确保平台安装了 Go：

**Vercel 配置**：
- Build Command: `amazon-linux-extras install golang1.11 && hugo --gc --minify`
- 环境变量: `HUGO_VERSION` 设置为最新的 Hugo extended 版本

**Node.js 20 环境**：
- Install Command: `dnf install -y golang`

## 总结

使用 hugo-theme-stack-starter 大大简化了 Hugo 博客的搭建过程：

### 优势

1. **开箱即用**：包含完整的配置和示例内容
2. **Hugo Modules**：使用现代化的模块系统加载主题
3. **GitHub Codespace 支持**：预配置开发环境，一键启动
4. **自动化部署**：内置 GitHub Actions 工作流和定时更新
5. **丰富功能**：搜索、评论、数学公式、代码高亮等
6. **响应式设计**：适配各种设备
7. **SEO 友好**：内置 SEO 优化配置

### 后续扩展

- 自定义主题样式和布局
- 添加更多内容类型（如项目展示、关于页面等）
- 集成第三方服务（统计、评论系统等）
- 优化网站性能和 SEO

---

*注：这篇文章是在 GitHub Copilot 的协助下编写的，旨在提供清晰的 Hugo 博客搭建指南。如果您在实际操作中遇到任何问题，欢迎参考 [Hugo 官方文档](https://gohugo.io/documentation/)获取最新信息。*
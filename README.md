# blog

基于 [Hugo](https://gohugo.io/) 与 [hugo-theme-stack](https://github.com/CaiJimmy/hugo-theme-stack) 主题的个人博客，中英双语，通过 GitHub Actions 自动部署到 GitHub Pages。

- 线上地址：<https://blog.dylucas.cn/>
- Hugo 版本：v0.165.0 extended（最低要求 0.157）
- 主题版本：hugo-theme-stack v4.0.3（git submodule）

## 目录结构

```
├── config.yaml              # 站点配置（语言、侧边栏、小部件等）
├── content/
│   ├── post/                # 文章（Page Bundle 形式，每篇一个目录）
│   ├── categories/          # 分类封面与说明
│   └── page/                # 关于、归档、搜索、友链等固定页面
├── static/favicon.ico
├── .github/workflows/hugo.yml
└── themes/hugo-theme-stack  # 主题子模块
```

## 本地开发

安装 Hugo extended >= 0.157（推荐 `brew install hugo`），然后：

```bash
# 首次克隆时拉取主题子模块
git clone --recurse-submodules https://github.com/dylucas/blog.git

# 本地预览（含草稿）
hugo server -D

# 构建产物到 public/
hugo --gc --minify
```

## 写作

在 `content/post/<文章名>/index.md` 中新建文章：

```bash
hugo new post/文章名/index.md
```

常用 front matter：

```yaml
---
title: 文章标题
date: 2026-08-27
image: cover.jpg        # 封面图（相对路径或外链）
tags: ["tag1"]
categories: ["Java"]
draft: false
---
```

## 部署

推送到 `main` 分支后，GitHub Actions（[.github/workflows/hugo.yml](.github/workflows/hugo.yml)）自动构建并发布到 GitHub Pages。工作流中固定的 `HUGO_VERSION` 需与本地保持一致。

## 升级主题

```bash
cd themes/hugo-theme-stack
git fetch --tags
git checkout <目标版本>   # 例如 v4.0.3
cd ../..
git add themes/hugo-theme-stack && git commit -m "chore: upgrade stack theme"
```

升级后注意同步本地 Hugo 版本与 CI 中的 `HUGO_VERSION`，并跑一次 `hugo --gc --minify` 验证配置兼容性（如 v4 将 `sidebar.avatar` 由嵌套结构改为字符串）。

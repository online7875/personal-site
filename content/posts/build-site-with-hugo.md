---
title: "用 Hugo + PaperMod 搭一个自己的主页"
date: 2026-11-25
summary: "为什么选 Hugo、主题怎么配、Cloudflare Pages 怎么接、Giscus 评论怎么开——一次讲清楚。"
description: "从零搭一个产品/技术向个人主页的完整选型与配置说明。"
tags: ["Hugo", "PaperMod", "Cloudflare Pages"]
ShowToc: true
comments: true
---

这是本站的第一篇文章，顺便把搭建过程记下来，以后换机器不用重新查文档。

## 为什么是这套组合

- **Hugo**：单二进制、毫秒级构建、中文文档多。不需要 Node 环境，不用维护依赖树。
- **PaperMod**：干净、暗色模式舒服、支持个人主页模式和站内搜索，改起来只需要一个 `custom.css`。
- **Cloudflare Pages**：免费额度够个人站用，全球 CDN，推 Git 就自动构建。
- **Giscus**：评论存到 GitHub Discussions，没有数据库、没有第三方账号体系。

## 本地跑起来

```bash
# 安装 Hugo（macOS）
brew install hugo

# 新建站点
hugo new site personal-site
cd personal-site

# 拉主题（不用 submodule，直接放 themes/ 里）
git clone --depth 1 https://github.com/adityatelange/hugo-PaperMod themes/PaperMod

# 本地预览
hugo server -D
```

浏览器打开 `http://localhost:1313` 就能看到。

## 目录结构

```text
personal-site/
├── hugo.toml              # 全站配置：菜单、参数、Giscus
├── assets/
│   ├── avatar.jpg         # 首页头像
│   └── css/extended/      # 自定义样式，会被自动合并
├── content/
│   ├── about.md           # 关于我
│   ├── projects/          # 项目作品集（一个 markdown 一个项目）
│   ├── posts/             # 博客
│   └── search.md          # 站内搜索页
├── layouts/
│   ├── home.html          # 首页：个人卡片 + 精选项目 + 最近更新
│   └── _partials/comments.html  # Giscus 评论
├── static/images/         # 图片等静态资源
└── themes/PaperMod/
```

## 一条命令发到线上

```bash
hugo --gc --minify
```

生成的 `public/` 就是整站，扔到任何静态托管都能跑。

Cloudflare Pages 上的构建设置：

| 配置项 | 值 |
| --- | --- |
| Framework preset | Hugo |
| Build command | `hugo --gc --minify` |
| Build output directory | `public` |
| 环境变量 | `HUGO_VERSION=0.170.1` |

## 评论与搜索

- **Giscus**：在 [giscus.app](https://giscus.app) 授权 GitHub 仓库、勾选 Discussions，拿到 `repoId` 和 `categoryId`，填进 `hugo.toml` 的 `[params.giscus]` 即可。
- **搜索**：PaperMod 自带 Fuse.js 搜索，只要 `home` 输出 JSON、再加一个 `layout: search` 的页面。

## 最后

内容比框架重要。这套东西搭一次能用很多年，剩下的时间应该花在写东西上。

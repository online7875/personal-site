# 产品手记 · 个人主页

Hugo + PaperMod 搭的个人主页 / 简历 / 技术博客，托管在 Cloudflare Pages，评论用 Giscus。

## 本地预览

```bash
# 需要 Hugo（v0.158 以上，本项目用的是 0.170.1）
hugo version

# 启动本地服务（含草稿）
hugo server -D
# 打开 http://localhost:1313
```

## 构建

```bash
hugo --gc --minify
# 产物在 public/
```

## 目录结构

```text
personal-site/
├── hugo.toml                  # 全站配置：标题、菜单、个人卡片、Giscus
├── assets/
│   ├── avatar.jpg             # 首页头像
│   └── css/extended/custom.css# 自定义样式（会被自动合并进主题样式）
├── content/
│   ├── about.md               # 关于我
│   ├── projects/              # 项目作品集（一个 .md 一个项目）
│   ├── posts/                 # 博客
│   └── search.md              # 站内搜索页
├── layouts/
│   ├── home.html              # 首页：个人卡片 + 精选项目 + 最近更新 + CTA
│   ├── projects/list.html     # 项目列表：双列卡片
│   └── _partials/comments.html# Giscus 评论
├── static/
│   ├── images/projects/       # 项目封面
│   └── favicon*.png           # 站点图标
└── themes/PaperMod/           # 主题（已内置，非 submodule）
```

## 上线前要改的 5 个地方

1. **站点地址**：`hugo.toml` 的 `baseURL`，改成 `https://<你的项目名>.pages.dev/`。
   （如果 Cloudflare 构建命令带 `--baseURL "$CF_PAGES_URL/"`，这行可以不动。）
2. **名字与副标题**：`hugo.toml` → `[params.profileMode]` 的 `title` / `subtitle`；
   `[params]` 的 `author`；`title`（左上角站名）。
3. **联系方式**：`[params] contactEmail`、`[[params.socialIcons]]` 里的 GitHub / LinkedIn 链接。
4. **关于页**：`content/about.md` 里的邮箱、链接，以及经历描述。
5. **Giscus**：`[params.giscus]` 的 `repo` / `repoId` / `category` / `categoryId`（见部署指南）。

## 写内容

```bash
# 新文章
hugo new content posts/my-new-post.md

# 新项目
hugo new content projects/my-project.md
```

- 项目排序用 `weight`（小的在前）。
- 项目封面放 `static/images/projects/`，在 front matter 里写 `cover.image: /images/projects/xxx.jpg`。
- 单页想关评论：front matter 加 `comments: false`。

## 部署

见 [部署指南.md](部署指南.md)。

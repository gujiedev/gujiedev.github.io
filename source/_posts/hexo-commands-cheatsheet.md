---
title: Hexo 常用命令速查
date: 2026-10-25 21:30:00
tags:
  - Hexo
  - 工具
categories:
  - 建站记录
---

这篇是写给自己的备忘录，顺便测试一下代码高亮。

## 常用命令

```bash
# 本地预览，默认 http://localhost:4000
npm run dev

# 写新文章（会在 source/_posts/ 生成 xxx.md）
npx hexo new "文章标题"

# 写新页面（如 关于我）
npx hexo new page "about"

# 草稿箱（source/_drafts/，默认不渲染）
npx hexo new draft "草稿标题"
npx hexo publish draft 草稿标题

# 生成静态文件到 public/ 目录
npm run build

# 一键清理缓存 + 构建 + 部署（需配置 _config.yml 的 deploy）
npm run deploy
```

## 文章 front-matter 模板

```yaml
---
title: 文章标题
date: 2026-10-25 21:30:00
tags: [Java, 后端]
categories: [技术]
cover: /images/cover.jpg   # 部分主题支持封面图
---
```

## 部署流程

本地 `git push` → GitHub Actions 自动执行 `hexo generate` → 产物推送到 `gh-pages` 分支 → GitHub Pages 发布。全程无需人工干预。

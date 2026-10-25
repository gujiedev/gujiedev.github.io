# 我的博客（Hexo + GitHub Pages）

基于 [Hexo 7](https://hexo.io/zh-cn/) 的个人博客骨架，配合 GitHub Actions 实现「写 Markdown → git push → 自动发布」的完整工作流。

## 技术栈

| 组件 | 选型 | 说明 |
|---|---|---|
| 博客框架 | Hexo 7 | 静态站点生成器，模板语法接近 Vue，中文社区成熟 |
| 托管 | GitHub Pages | 免费，全球可访问 |
| 部署 | GitHub Actions | push 自动构建，产物推送到 `gh-pages` 分支 |
| 主题 | landscape（默认） | 够用且清爽，可随时更换 |
| 本地运行 | Node.js 18+ | 本机已装 Node 22 |

## 目录结构

```
hexo-blog/
├── _config.yml            # 博客核心配置（标题、URL、主题等）
├── package.json           # 依赖与 npm scripts
├── scaffolds/             # 文章/页面模板（front-matter 骨架）
├── source/
│   ├── _posts/            # 你的 Markdown 文章都在这里
│   └── ...                # 其他自定义页面/资源
├── .github/workflows/
│   └── deploy.yml         # GitHub Actions 自动构建发布工作流
└── public/                # 构建产物（自动生成，不要手动提交）
```

## 本地开发

```bash
# 1. 安装依赖（只需一次）
npm install

# 2. 启动本地预览，访问 http://localhost:4000
npm run dev

# 3. 写新文章
npx hexo new "文章标题"      # 生成 source/_posts/文章标题.md

# 4. 构建到 public/ 目录
npm run build
```

常用命令一览：

| 命令 | 作用 |
|---|---|
| `npm run dev` | 本地预览（支持热更新，改完刷新浏览器即可） |
| `npm run build` | 清理 + 生成静态文件到 `public/` |
| `npx hexo new "标题"` | 新建文章 |
| `npx hexo new page "about"` | 新建独立页面 |
| `npx hexo new draft "标题"` | 新建草稿（`source/_drafts/`，默认不发布） |
| `npx hexo publish draft 标题` | 草稿转为正式文章 |

## 部署到 GitHub Pages（首次，约 5 分钟）

### 第 1 步：改配置

打开 `_config.yml`，修改以下字段：

```yaml
url: https://你的GitHub用户名.github.io   # 博客地址
author: 你的昵称
```

> 如果仓库名不是 `用户名.github.io`（比如叫 `my-blog`），`url` 要写成 `https://用户名.github.io/my-blog`。

### 第 2 步：创建仓库并推送

```bash
# 在 GitHub 上新建一个仓库，名字建议：
#   方案A：用户名.github.io   → 最终地址 https://用户名.github.io
#   方案B：任意名字如 my-blog  → 最终地址 https://用户名.github.io/my-blog

# 本目录初始化并推送
cd hexo-blog
git init
git add .
git commit -m "init: hexo blog"
git branch -M main
git remote add origin git@github.com:你的用户名/仓库名.git
git push -u origin main
```

### 第 3 步：开启 GitHub Pages

推送后，GitHub Actions 会自动开始构建（仓库页面的 **Actions** 标签页可见进度，约 1-2 分钟）。

然后去仓库 **Settings → Pages**：

- **Source** 选择 `Deploy from a branch`
- **Branch** 选择 `gh-pages` / `(root)`，保存

等 1-2 分钟，访问 `https://你的用户名.github.io`（方案B 则带 `/仓库名` 后缀）即可看到博客。

> 之后每次 `git push` 到 main，Actions 都会自动重新构建发布，**不用再管任何部署命令**。

## Gitee Pages 镜像（可选，国内加速）

Gitee 也有 Pages 服务，但没有自动化构建，需要每次手动点"更新"。如果想做国内镜像：

1. 在 Gitee 建同名仓库，把 `public/` 目录内容推送上去
2. Gitee 仓库 **服务 → Gitee Pages** 中开启，选择部署分支
3. 每次本地 `npm run build` 后，手动把 `public/` 内容推到 Gitee 并点击"更新"

个人建议：先只用 GitHub Pages，访问稳定、无需维护；Gitee Pages 近两年服务波动较大，不值得为它增加复杂度。想加速国内访问，更现代的做法是接 Cloudflare CDN 或同步部署到 Vercel。

## 自定义域名（可选）

1. 域名 DNS 添加一条 `CNAME` 记录，指向 `你的用户名.github.io`
2. 在仓库 `source/` 目录下新建文件 `CNAME`，内容为你的域名（如 `blog.example.com`），提交推送
3. 仓库 Settings → Pages → Custom domain 填入域名，勾选 Enforce HTTPS

## 下一步可以做的事

- **换主题**：`npm install hexo-theme-butterfly`，然后把 `_config.yml` 里 `theme: landscape` 改成 `theme: butterfly`（主题配置见对应主题的文档）
- **加评论**：主题支持 Giscus / Valine 等，在主题配置里填
- **访问统计**：接百度统计或不蒜子（`busuanzi`）
- **SEO**：装 `hexo-generator-sitemap` 和 `hexo-generator-feed`，提交 sitemap 到搜索引擎

## 常见问题

**Q: push 后 Actions 构建失败？**
看仓库 Actions 页面的报错日志。最常见原因是 `_config.yml` 的 YAML 缩进/格式错误，可用在线 YAML 校验工具检查。

**Q: 本地预览正常，部署后 404？**
检查 `_config.yml` 的 `url` 和 `root` 是否和实际访问地址匹配，特别是方案B（子路径）的 `root` 要设为 `/仓库名/`。

**Q: 文章没发布？**
确认文章日期不是未来时间（`future: true` 时未来文章不渲染），且 front-matter 格式正确。

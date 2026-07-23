# YF Blog / 依凡的记事本 

个人博客，基于 [Hugo](https://gohugo.io/) + [PaperMod](https://github.com/adityatelange/hugo-PaperMod/) 构建。

## 有效目录结构

```
├── content/          # 文章内容（Markdown）
│   └── post/         # 博客文章
├── layouts/          # 自定义模板覆盖
│   ├── _markup/      # Markdown 渲染钩子（图片等）
│   └── _partials/    # 局部模板覆盖（footer 等）
├── archetypes/       # 文章模板
├── themes/           # 主题（PaperMod 子模块）
├── config/
│   └── production/
│       └── hugo.toml  # 生产构建配置（publishDir = "dist"）
├── hugo.toml          # 站点配置
├── dist/              # hugo --minify 生产构建输出（由 config/production/hugo.toml 指定）
└── public/            # hugo server -D 开发服务器输出
```

## 自定义项

- **Footer 备案链接** — 覆盖 `layouts/_partials/footer.html`，使 `params.footer.text` 中的 HTML 链接正确渲染。
- **图片渲染** — `layouts/_markup/render-image.html` 自定义了 `<img>` 标签输出，添加 `referrerpolicy="no-referrer"` 并透传 Markdown 属性。
- **网站图标** — `static/` 下包含各平台 favicon 资源（`favicon.ico`, `apple-touch-icon.png`, `android-chrome-192x192.png` 等），其中 `safari-pinned-tab.svg` 为 Safari 浏览器 Pinned Tab 图标（SVG）。使用 [favicon.io](https://favicon.io/favicon-generator/) 生成。

## 文章写作辅助

`.github/skills/md-to-hugo/` 包含一个 Copilot skill（`SKILL.md`），用于将普通 Markdown 笔记快速转换为 Hugo 博文格式：

- 自动添加 TOML front matter（标题、日期、标签等）
- 转英文文件名
- 做内容兼容性检查

在 `content/post/` 下新建 Markdown 文件后，可通过 Copilot 调用此 skill 完成格式转换。

## 部署

运行 `hugo --minify` 后，将 `dist/` 目录部署到任何静态托管服务（Vercel、Netlify、Cloudflare Pages、自己的服务器等）。
开发时使用 `hugo server -D`，产物在 `public/`（仅本地调试用）。

当前部署在：<https://blog.yfstudio.online/>

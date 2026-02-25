# 使用 MkDocs + GitHub Pages 制作插件帮助网页完整流程

本流程适用于将任何插件的 Markdown 帮助文档快速制作成美观的网页，并免费托管在 GitHub Pages。

---

## 1. 环境准备
- 安装 Python（推荐 3.7 及以上）
- 安装 MkDocs：
  ```bash
  pip install mkdocs
  ```
- （可选）安装 Material 主题：
  ```bash
  pip install mkdocs-material
  ```

---

## 2. 新建 MkDocs 项目
- 新建一个空文件夹（如 `my-plugin-docs`），进入该文件夹。
- 初始化 MkDocs 项目：
  ```bash
  mkdocs new .
  ```
- 结构如下：
  ```
  mkdocs.yml
  docs/
    index.md
  ```

---

## 3. 编辑文档内容
- 所有 Markdown 文档（如 index.md、usage.md、faq.md 等）都放在 `docs/` 文件夹。
- 编辑 `mkdocs.yml` 配置导航栏，例如：
  ```yaml
  site_name: 插件帮助文档
  nav:
    - 首页: index.md
    - 安装: installation.md
    - 使用: usage.md
    - 常见问题: faq.md
    - 联系: contact.md
  theme:
    name: mkdocs
    language: zh
  ```
- 可自定义 logo、favicon、主题等。

---

## 4. 本地预览
- 在项目根目录运行：
  ```bash
  mkdocs serve
  ```
- 浏览器访问 http://127.0.0.1:8000/ 预览效果。

---

## 5. 生成静态网页
- 在项目根目录运行：
  ```bash
  mkdocs build
  ```
- 会生成 `site/` 文件夹，里面全是 HTML、CSS、JS 等静态网页文件。

---

## 6. 上传到 GitHub
- 新建一个公开仓库（如 `My-Plugin-Help`）。
- 上传以下内容：
  - `mkdocs.yml`（可选，便于后续维护）
  - `docs/`（Markdown 源文件，便于维护）
  - `site/` 文件夹里的所有内容，**复制到仓库的 `/docs` 文件夹**（不是上传整个 site 文件夹，而是把 site 里的内容放到 docs 里）。
- 最终仓库结构如下：
  ```
  /docs
    index.html
    ...（所有静态网页和资源）
  mkdocs.yml
  /docs_src（可选，存放 Markdown 源文件）
  ```

---

## 7. 配置 GitHub Pages
- 打开仓库，进入 Settings > Pages。
- Source 选择 `main` 分支的 `/docs` 文件夹，保存。
- 几分钟后，页面会显示你的帮助网页地址。

---

## 8. 访问与维护
- 访问 GitHub Pages 提供的网址即可浏览帮助网页。
- 后续如需修改内容，只需：
  1. 修改 Markdown 文档
  2. 重新 `mkdocs build`
  3. 覆盖上传 `site/` 里的内容到仓库 `/docs` 文件夹
  4. 推送到 GitHub

---

## 9. 常见问题
- **页面无样式/空白**：检查 `/docs` 文件夹下是否有 `index.html`、`css/`、`js/` 等静态资源。
- **导航栏不显示**：确认 `mkdocs.yml` 配置正确，且所有页面都已 build。
- **访问慢/404**：等待几分钟，GitHub Pages 有缓存延迟。

---

## 10. 进阶用法
- 可用 `mkdocs gh-deploy` 自动部署到 `gh-pages` 分支（适合不想用 `/docs` 文件夹的情况）。
- 可用 Material 主题美化文档。
- 支持自定义域名（需购买域名）。

---

如有疑问，建议查阅 [MkDocs 官方文档](https://www.mkdocs.org/) 或 [GitHub Pages 官方文档](https://docs.github.com/en/pages)。

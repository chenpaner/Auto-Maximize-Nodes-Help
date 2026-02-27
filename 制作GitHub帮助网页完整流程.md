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





这是一个非常经典的问题！既然你的工作流逻辑（Artifact 模式）是正确的，那么“修改了模板但网站没变”通常只有以下 **3 个原因**。

请按照顺序排查，90% 的情况是 **原因 1**。

---

### 原因 1：GitHub Actions 没有读取到你最新的 `mkdocs.yml` (最常见)

即使你提交了代码，如果 GitHub Actions 运行的代码不是最新的那一次提交，它就会用旧的配置构建。

**检查步骤：**
1. 进入你的仓库 -> 点击 **Actions** 标签页。
2. 点击最近一次失败的或成功的 Workflow 运行记录。
3. 点击左侧的 **`build`** 任务，展开 **`Checkout`** 和 **`Build Site`** 步骤。
4. **关键点**：看日志里 `Checkout` 步骤输出的 `HEAD is now at <commit_hash> ...`。
   - 对比这个 `<commit_hash>` 和你本地 `git log` 的最新哈希值是否一致？
   - 如果不一致，说明 Action 拉取的是旧代码。
   - **解决方法**：有时候网络波动会导致 checkout 失败或拉取旧分支。尝试在本地执行一次 `git push --force` (小心使用) 或者简单地再做一个微小的修改（比如在 README 加个空格）并提交推送，强制触发一次新的 Action，确保它拉取到最新代码。

---

### 原因 2：浏览器缓存了静态资源 (CSS/JS)

MkDocs 生成的 HTML 文件可能更新了，但浏览器为了速度，依然加载了旧的 CSS 样式表或 JS 脚本文件。这是前端开发中最常见的“假象”。

**验证方法（强制刷新）：**
- **Windows/Linux**: 在网页上按 `Ctrl` + `F5` (或 `Ctrl` + `Shift` + `R`)。
- **Mac**: 按 `Cmd` + `Shift` + `R`。
- **无痕模式**: 打开浏览器的“无痕/隐私模式”，输入网址访问。如果无痕模式下是新模板，普通模式是旧模板，那就是缓存问题。

**解决方法：**
如果是缓存问题，通常等待几分钟或强制刷新即可解决。如果你希望彻底避免，可以在 `mkdocs.yml` 中开启某些插件的版本控制（但通常不需要）。

---

### 原因 3：配置文件修改未生效或被覆盖

虽然你在本地改了 `mkdocs.yml`，但可能存在以下细节问题：

1.  **缩进错误**：YAML 对缩进非常敏感。如果 `theme` 下面的属性缩进不对，MkDocs 会忽略该属性并使用默认值，且**不会报错**（通常只会警告）。
    *   **检查**：在本地终端运行 `mkdocs build --verbose`，看是否有 `WARNING` 提示未知的配置项。
2.  **文件名错误**：确认你修改的文件名确实是 `mkdocs.yml` (全小写)，而不是 `Mkdocs.yml` 或其他。
3.  **Action 中的参数覆盖**：
    回顾你的 Action 代码：
    ```yaml
    - name: Build Site
      run: mkdocs build --site-dir _site
    ```
    这个命令本身没问题，它会读取根目录下的 `mkdocs.yml`。但请确认你没有在 Action 里通过其他方式（比如 `echo` 写入文件）意外覆盖了配置文件。

---

### 🔥 终极调试方案：查看构建日志

如果以上都没解决，我们需要看 GitHub Actions 到底构建了什么。

1. 进入 **Actions** 页面，点击最新的运行记录。
2. 找到 **`Build Site`** 这一步，点击展开。
3. 仔细查看输出日志。
   - 应该能看到类似 `Building documentation...` 的信息。
   - **关键技巧**：在 `mkdocs.yml` 中临时添加一个明显的错误（比如把 `site_name` 改成一个很怪的名字），然后提交。
   - 再次触发 Action。
   - **如果网站名字变了**：说明 Action 确实读取了新配置，那问题绝对是 **浏览器缓存 (原因 2)**。
   - **如果网站名字没变**：说明 Action **根本没读到你的新代码 (原因 1)**，或者你推送到错误的分支了。

### 额外提示：关于 `readthedocs` 主题的视觉差异

如果你是从 `mkdocs` 主题切换到 `readthedocs` 主题：
- **变化可能不明显**：这两个内置主题在某些默认设置下（尤其是没有侧边栏导航或内容较少时）看起来可能有点像。
- **确认切换成功的方法**：
  - `mkdocs` 主题：顶部导航栏通常在页面最上方，搜索框在右上角。
  - `readthedocs` 主题：左侧有一个固定的深色（或灰色）侧边栏，包含导航和搜索框，顶部栏较窄。
  - **请重点观察左侧是否出现了固定的导航栏**，这是区分两者的最明显特征。

**建议操作：**
先做 **强制刷新 (Ctrl+F5)**。如果不行，请在本地运行 `mkdocs serve` 预览，确认本地显示正常后，再检查 GitHub Actions 的 `Checkout` 哈希值。
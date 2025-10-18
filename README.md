# 基于`SITCON 短網址服務`改进的短链接服务

## 这是什么？

使用 GitHub Pages 创建的一个简易短链接服务，基于原项目[SITCON 短網址服務](https://github.com/sitcon-tw/URL-Shortener)进行修改，在其本身支持的自定义标题、摘要和 Open Graph 标签的基础上，增加了对每个页面倒计时的单独时间设置和显示以及背景图片/视频的自定义。由于与原本页面相差较大，所以顺带对移动端进行了适配。适合无服务器或者不想透过服务器进行短链接搭建的新手使用。

## 如何使用

1. 在 `_redirects` 文件夹中创建一个新的 Markdown 文件。文件名将成为短链接的一部分（slug），在该文件夹下新增的文件夹也应算在内。
   例如：`template.md` 将生成 `https://example.com/template`，`/template/template.md` 将生成 `https://example.com/template/template`

2. 在 Markdown 文件中使用以下格式：

   ```yaml
   ---
   layout: redirect
   title: "演示文档"
   description: "这是一篇演示文档"
   pic: "/images/your-image.jpg"
   # 或
   pic: "https://example.com/images/your-image.jpg"
   background: "/images/0.png" #视频或图片，视频建议优先使用外链
   #或
   background: "https://example.com/images/0.png"
   redirect_to: "https://example.com/"
   redirect_time: 3 #跳转时间，不填默认为0.
   ---

   ```

   注意：
   - 如果图片存放在当前repo的 `images` 文件夹中，请使用绝对路径（例如：`/images/your-image.jpg`）。
   - 如果要使用外部图片，请使用完整的 URL。
   - 背景视频建议使用外部链接。
   - `title`、`description`和`pic`用于Open Graph 标签，但部分情况下`title`和`description`能用于显示标题与内容。（例如404页面和首页）
   - 如不需要请注释掉对应行，目前`pic`和`background`以及`redirect_time`不影响运行。(大概)【其中如`redirect_time`未填则默认为0】
  
   一键复制模板：

   ```yaml
   ---
   layout: redirect
   title: "标题"
   description: "摘要"
   pic: "https://example.com/images/withname.png"
   background: "/images/0.png" #视频或图片
   redirect_to: "https://example.com/"
   redirect_time: 3
   ---

   ```

3. 如果你想使用自定义图片，请将图片上传到 `images` 文件夹。

4. 提交变更并推送到 GitHub 存储库。

5. GitHub Actions 会自动创建并部署你新的短链接。

## 技术原理

本repo使用以下技术和工具：

1. **GitHub Pages**：用于托管静态网站。
2. **Jekyll**：静态网站生成器，用于将 Markdown 文件转换为 HTML。
3. **自定义 HTML 模板**：位于 `_layouts/redirect.html`，用于生成跳转页面。
4. **GitHub Actions**：自动创建和部署流程。

### 文件结构

```
/
├── _redirects/     # 存放 Markdown 文件的文件夹
├── _layouts/       # 存放 HTML 模板
├── images/         # 存放图片文件
├── _config.yml     # Jekyll 设定文件
└── .github/workflows/  # GitHub Actions 设置
```

### 跳转机制

跳转使用 JavaScript 延迟执行。这个延迟设定~~在 HTML 模板~~在 Markdown 文件中，可以根据需求调整。如未填写`redirect_time`参数，则默认为0。

### 自动部署

每当有新的变更推送到主分支，GitHub Actions 会自动触发部署流程，将 Markdown 文件转换为 HTML，并部署到 GitHub Pages。

## 维护注意事项

1. 确保 `_config.yml` 中的设定无误，特別是 `collections` 和 `url` 部分。

2. 如需修改跳转逻辑~~或 Google Tag Manager 設定~~，请编辑 `_layouts/redirect.html` 文件。

3. GitHub Actions 设定文件位于 `.github/workflows/build.yml`，如需调整自动化流程，请修改此文件。

4. 若要更新 Google Tag Manager 代码，请在 HTML 模板中找到对应的代码块进行修改。

5. 图片文件应放在 `images/` 文件夹中。请确保使用适当大小和格式的图片，以增强网站效能。

6. 在引用图片时，请使用绝对路径（例如：`/images/your-image.jpg`）或完整的 URL。

7. 图片路径支持两种格式：
   - 绝对路径：用于引用存放在 `images/` 文件夹中的图片（例如：`/images/your-image.jpg`）。
   - 完整 URL：用于引用外部图片（例如：`https://example.com/your-image.jpg`）。
   请确保在 Markdown 文件中指定正确的图片路径。

8. 定期检查外部图片链接是否有效，确保所有短链接页面都能正确显示预期的图片。

## 效能考量

1. **页面载入时间**：由于使用 JavaScript 进行跳转，页面内可能会有短暂的显示时间，可以在对应的 Markdown 文件中调整这个值。

2. ~~**SEO 影响**：虽然使用 JavaScript 跳转可能会稍微影响搜索引擎最优化，但已经加入了适当的 meta 标签来避免这个问题。~~

## 相关资源

- [URL-Shortener](https://github.com/sitcon-tw/URL-Shortener) **特别感谢**
- [GitHub Pages](https://docs.github.com/en/pages)
- [Jekyll](https://jekyllrb.com/docs/)
- [Markdown 指南](https://www.markdownguide.org/)
- [Open Graph 協議](https://ogp.me/)

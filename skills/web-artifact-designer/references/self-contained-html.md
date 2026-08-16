# 自包含 HTML Artifact 构建指南

DSH 的默认交付物是**单个自包含 HTML 文件**：双击离线可开、CSS/JS 内联、主体内容不依赖外部资源。这份指南讲清楚怎么把单文件写到「精致」的水准，以及什么时候才该升级成多文件 React 项目。

## 一、单文件的骨架

一个干净的单文件骨架长这样：

```html
<!doctype html>
<html lang="zh-CN">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>产品发布 · 星辰计划</title>
  <style>
    /* 设计系统：变量 + 基础 + 组件，全部内联 */
    :root {
      --bg: #faf7f2;
      --text: #1c1917;
      --muted: #78716c;
      --primary: #b45309;
      --border: #e7e5e4;
    }
    * { box-sizing: border-box; margin: 0; }
    body { background: var(--bg); color: var(--text); font-family: system-ui, -apple-system, "Segoe UI", sans-serif; line-height: 1.65; }
    /* ... */
  </style>
</head>
<body>
  <!-- 语义化结构：header / main / section / footer -->
</body>
</html>
```

要点：

- **`<meta name="viewport">` 必须写**，否则移动端会缩成一团。
- **用语义化标签**（`header/main/section/article/footer`）和正确的标题层级（一个 `<h1>`，其余 `<h2>/<h3>`），这既是可访问性，也是层级设计。
- **CSS 变量集中在 `:root`**，改主题只改一处。
- **`box-sizing: border-box` 和 reset**（`* { margin: 0 }`）先做，避免默认间距干扰。

## 二、字体策略（自包含的代价与取舍）

| 方案 | 优点 | 代价 | 适用 |
|---|---|---|---|
| 系统字体栈 `system-ui, -apple-system, "Segoe UI", "PingFang SC", "Microsoft YaHei", sans-serif` | 零依赖、秒开、跨平台都有 | 各平台观感略不同 | **默认首选**，尤其正文 |
| Google Fonts `<link>` 或 `@import` | 标题有性格 | 需要联网；中文字体巨大 | 英文标题可接受；中文慎用 |
| 本地/内联字体 `@font-face` + base64 | 真正离线自包含 | 文件变大（几十 KB~几 MB） | 少数情况下值得 |

**推荐组合**：正文用系统字体栈；英文标题用 Google Fonts 里的显示字体（如 `Fraunces`、`Space Grotesk`、`Sora`、`Bricolage Grotesque`）；中文标题用 `Noto Sans SC`/`Source Han Sans SC` 的系统安装版本或接受系统字体 + 加重加字距。**不要为中文标题挂全量 Web 字体**——几 MB 的下载毁掉打开体验。

## 三、图标与图形

- **图标**：内联 SVG，统一风格（线性/填充二选一）。Heroicons / Lucide / Feather 的 `path` 可直接粘贴内联。控制 `stroke-width` 一致。
- **插画/图形**：能用 CSS（`border-radius`、`gradient`、`clip-path`、`transform`）画就用 CSS；复杂矢量用内联 `<svg>`。
- **图表**：柱状/折线/环形用内联 SVG 手绘，数值直接标注，颜色取自 CSS 变量。不要引入几十 KB 的图表库，除非要做可交互复杂图。
- **照片/位图**：单文件无法内联大图又保持轻量。优先用 CSS 渐变/SVG 营造视觉；真需要位图时用外链 URL 并注明「需联网」，或压缩到可接受体积后 base64 内联。

## 四、响应式要点

- 用 `clamp()` 做流式字号，让标题在手机和桌面都好看：`font-size: clamp(2rem, 4vw + 1rem, 4rem)`。
- 栅格 `grid-template-columns: repeat(auto-fit, minmax(240px, 1fr))` 让卡片自动换列，少写断点。
- 关键断点 640/1024 处检查：导航是否挤压、卡片是否塌、有无横向滚动。
- 图片/图表 `max-width: 100%; height: auto`。

## 五、交互（需要才加）

- 简单的折叠、tab 切换、复制按钮、浅色/深色切换：用一小段原生 JS，几行搞定，别引框架。
- 需要响应式状态管理、路由、复杂组件树时，才考虑升级（见下）。
- 动效优先 `CSS transition/animation`，JS 只负责切换 class/状态。

## 六、何时上 React（多文件 artifact）

满足以下**多条**才升级成 React 项目，否则坚持单文件：

- 有多个视图/路由（如登录→列表→详情）。
- 大量可复用、有内部状态的组件。
- 需要与 API 实时交互并管理复杂状态。
- 用户明确要求「做成一个可维护的前端项目」。

升级时遵循：

1. 用 Vite + React + Tailwind 搭建（DSH 有 `bash`，可直接 `npm create vite@latest`）。
2. 仍然先定设计系统：把 `:root` 变量映射到 Tailwind 主题 token，别让 Tailwind 默认配色接管。
3. 组件化但别过度拆分；每个组件做一件事。
4. 视觉质量清单**同样适用**——上了 React 不意味着可以放弃字体/色彩/留白。

## 七、命名与交付

- 文件名语义化：`product-launch-landing.html`、`q3-sales-infographic.html`、`brand-poster.svg`。
- 交付时说明：文件路径 + 双击即可打开 + 一句「想调整风格/配色/文案告诉我方向」。
- macOS 可 `open <file>.html` 直接唤起浏览器给用户看。

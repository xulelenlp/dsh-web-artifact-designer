# web-artifact-designer · DSH 前端设计生成 Skill

> 把「帮我做个海报 / 网页 / 信息图 / 图表」变成一份**可直接双击打开、视觉上真正像样的自包含 HTML / SVG 设计稿**。
> A DeepSeek Harness (DSH) skill that turns visual/frontend design requests into polished, self-contained HTML/SVG artifacts — instead of generic AI-looking pages.

[English](#english) · 中文

## 这个 skill 解决什么问题

AI 写前端最大的痛点是：**「能跑，但丑」**——紫色渐变、全居中、默认字体、emoji 当图标、没有层级。这个 skill 把 Anthropic 官方 `canvas-design` / `web-artifacts-builder` 的设计方法论适配到 DSH，给模型一套**可重复的流程 + 硬性质量门槛**，让设计产出从「AI 味」变成「敢署名」的作品。

它解决的是真实高频问题：海报、信息图、落地页、数据图表、banner、UI 组件稿、单页应用原型——这些是运营、市场、产品、开发者每天都在要的东西。

## 特性

- ✅ **纯提示词 skill，零依赖、即装即用**（无需 Playwright / 构建链）
- ✅ **设计而非装饰**的方法论：字体排印、配色系统、留白、细节完成度
- ✅ **反模式清单**：直击「AI 味」的 9 个通病，先教模型避坑
- ✅ **自包含 HTML 交付规范**：双击离线可开、可分享、可迭代
- ✅ **硬性质量清单**：交付前逐条自检，不达标不许交付
- ✅ **DSH 原生工作流**：结合 `write`/`edit`/`read`/`open`/`web_search` 的实操指引
- ✅ **附赠示例**：一个能体现质量标杆的「复古编辑风」咖啡品牌落地页
- ✅ **bundle 一键安装**：封装为 DSH Cordis 插件，`dsh plugin add` 即可装

## 目录结构

```
├── index.js                         # Cordis 插件入口：把 skills/**/SKILL.md 注册为 skill provider
├── cordis.patch.yml                 # bundle 清单：把本包挂载进 profile
├── package.json                     # dsh.bundle 元数据（dsh plugin add 依赖它）
├── skills/web-artifact-designer/
│   ├── SKILL.md                     # 核心技能（方法论 + 工作流 + 质量清单）
│   ├── references/
│   │   ├── design-principles.md     # 设计原则：反模式 / 字体 / 色彩 / 布局 / 细节
│   │   └── self-contained-html.md   # 自包含 HTML artifact 构建指南
│   └── assets/
│       ├── template.html            # 开箱即用的 starter 模板
│       └── example-landing.html     # 质量标杆示例（咖啡品牌落地页）
└── demo/q3-sales-infographic.html   # 实际产出 demo（信息图）
```

## 安装

### 方式一：bundle 一键安装（推荐，进插件市场/awesome 清单走这条）

```bash
dsh plugin --profile web add github:xulelenlp/dsh-web-artifact-designer#main
```

装完重启会话即生效。skill 以 provider 形式随 profile 挂载，无需手动复制文件。

### 方式二：复制到用户级 skill 目录（全局生效，纯 markdown）

```bash
mkdir -p ~/.dsh/skills
cp -r skills/web-artifact-designer ~/.dsh/skills/
```

### 方式三：项目级（仅当前项目生效）

```bash
mkdir -p .dsh/skills
cp -r skills/web-artifact-designer .dsh/skills/
```

### 验证是否生效

新会话里问一句：「你有哪些 skill？」或直接「帮我做一个咖啡品牌的落地页」，观察模型是否产出自包含 HTML 设计稿、并主动对照质量清单自查。

## 使用示例

**输入**：「帮我做一张 Q3 销售数据的信息图，风格简洁专业。」

**模型行为（加载本 skill 后）**：
1. 澄清 brief（受众/核心信息/尺寸/风格，必要时 1-2 问）
2. 定概念 + 字体 + 配色 + 布局骨架
3. `write` 输出单个自包含 HTML（数据图表用内联 SVG 手绘）
4. 逐条对照质量清单自查并修正
5. 交付：告知路径、提示 `open xxx.html`、邀请迭代

**输出**：`q3-sales-infographic.html`，双击即可在浏览器打开。

## 与其他 DSH 社区项目的区别

| 项目 | 定位 |
|---|---|
| `DSH-Office` / PPT / 可视化插件 | 面向**办公文档**（PPTX/DOCX/XLSX/PDF）与**图表库** |
| 本 skill | 面向**网页/视觉设计稿**（海报、落地页、信息图、组件稿），产出**自包含 HTML/SVG** |

两者互补不重叠：要 PPT 用 Office 插件，要「一张好看的网页/海报」用本 skill。

## 设计原则速览

1. **先定概念，再写代码**——「给谁看、传达什么、什么情绪」比 CSS 技巧值钱。
2. **字体排印占 90%**——标题有性格 + 正文求舒适 + 清晰层级。
3. **刻意约束**——一个焦点、一套配色、一套间距节奏，约束出风格。
4. **避开「AI 味」**——默认渐变、全居中、emoji 图标、随机间距统统不要。

完整内容见 `references/design-principles.md`。

## 贡献

欢迎 PR / Issue：

- 补充更多设计风格配方（如「酸性设计」「新粗野主义」等）
- 增加更多质量标杆示例
- 修正中英文表述
- 报告实际使用中模型「不听话」的场景

## License

MIT © 2026

---

## English

A zero-dependency, prompt-only skill for DeepSeek Harness that teaches the model to produce **polished, self-contained HTML/SVG design artifacts** (posters, infographics, landing pages, charts, component mockups). Adapted from Anthropic's official `canvas-design` and `web-artifacts-builder` skills, with a DSH-native workflow (`write`/`edit`/`read`/`open`/`web_search`), a hard quality checklist, and an anti-pattern list targeting the generic "AI look".

**Install**: copy `skills/web-artifact-designer` into `~/.dsh/skills/` (global) or `.dsh/skills/` (project), then start a new session.

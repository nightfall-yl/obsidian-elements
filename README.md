<p align="center">
  <h1 align="center">🏠 Homeboard</h1>
  <p align="center">将 Obsidian 首页变成可配置的导航仪表盘</p>
  <p align="center">
    <img src="https://img.shields.io/badge/Obsidian-1.3.0+-7C3AED?logo=obsidian" alt="Min App Version">
    <img src="https://img.shields.io/badge/Version-2026.4-22C55E" alt="Version">
    <img src="https://img.shields.io/badge/License-MIT-blue" alt="License">
  </p>
</p>

---

## ✨ 功能亮点

- 🎨 **可视化 Builder** — 拖拽式仪表盘搭建，所见即所得
- 📐 **多列布局** — 支持 1~4 列自由排版，列宽可拖拽调整并自动记忆
- 🔗 **Links 导航卡片** — 快捷入口、项目索引、外部链接，一卡搞定
- 🗓️ **热力图** — GitHub 风格的贡献热力图，追踪你的创作节奏
- ✏️ **原地编辑** — 光标放在代码块上即可通过命令回填编辑，无需重新创建
- 🌐 **多语言** — 支持中文 / English

## 📦 安装

### 手动安装

1. 下载 [releases](https://github.com/nightfall-yl/Obsidian-Homeboard/releases) 中的最新版本
2. 将以下文件放入你的 Obsidian 插件目录：

```
.vault/.obsidian/plugins/obsidian-homeboard/
├── main.js
├── manifest.json
└── styles.css
```

3. 在 Obsidian 设置 → 社区插件中启用 **Homeboard**

### 从源码构建

```bash
git clone https://github.com/nightfall-yl/Obsidian-Homeboard.git
cd Obsidian-Homeboard
npm install
npm run build
```

构建产物会输出到根目录及 `release/` 目录。

## 🚀 快速上手

### 创建首页仪表盘

在任意 Markdown 文件中插入 `homeboard` 代码块：

````markdown
```homeboard
id: homepage-main
title: 我的首页
columns: 2
gap: 16
cards:
  - type: links
    title: 常用入口
    span: 1
    linksLayout: inline
    links:
      - label: 收件箱
        url: 00.收件箱-Index
      - label: 日记
        url: 00.Daily Index
      - label: 快速捕获
        url: QuickCap

  - type: links
    title: PARA 管理
    span: 1
    linksLayout: inline
    links:
      - label: 项目
        url: 00.项目 Index
      - label: 领域
        url: 00.领域 Index
      - label: 资源
        url: 00.资源 Index
```
````

### 使用可视化 Builder

通过命令面板运行 `Open Homeboard builder` 打开拖拽式搭建界面。

> 💡 **提示**：给每个代码块设置固定 `id`（如 `id: homepage-main`），可确保列宽拖拽结果持久化。

### 编辑已有代码块

将光标移入现有的 `homeboard` 代码块，运行命令面板中的 `Edit Homeboard block at cursor`，即可读取当前配置并在保存后回写。

## 📖 配置参考

### 基本参数

| 参数 | 说明 | 默认值 |
|------|------|--------|
| `id` | 代码块唯一标识（推荐设置，用于记忆列宽） | 自动生成 |
| `title` | 仪表盘标题 | — |
| `columns` | 列数（1~4） | 2 |
| `gap` | 卡片间距（px） | 16 |
| `cards` | 卡片列表 | — |

### 卡片类型

#### `links` — 导航卡片

适合放置常用页面、项目入口、外部网站。

```yaml
- type: links
  title: Quick Links
  linksLayout: inline        # inline | list
  links:
    - label: Inbox
      url: obsidian://open?vault=YourVault&file=Inbox
    - label: GitHub
      url: https://github.com/
      external: true          # 标记为外部链接
```

| 字段 | 说明 |
|------|------|
| `title` | 卡片标题 |
| `span` | 跨列数 |
| `linksLayout` | 链接排列方式：`inline`（内联）或 `list`（列表） |
| `links` | 链接列表，每项含 `label`、`url`，可选 `external` |

## 🎮 操作方式

| 方式 | 说明 |
|------|------|
| **命令面板** | `Insert Homeboard block` / `Open Homeboard builder` / `Edit Homeboard block at cursor` |
| **右键菜单** | 编辑区右键 → 新增 Homeboard 组件 → 新建热力图 / 新建分栏 |
| **编辑按钮** | 阅读模式下代码块旁的浮动编辑按钮 |

## 🔧 技术栈

- TypeScript + React
- Obsidian API
- esbuild 构建
- Luxon 日期处理

## 📁 项目结构

```
src/
├── main.ts              # 插件入口
├── builderModal.ts      # Builder 模态框
├── homepageProcessor.ts # 代码块解析与渲染
├── homepageConfig.ts    # 配置类型
├── homepageTypes.ts     # 类型定义
├── homepageYaml.ts      # YAML 解析
├── i18/                 # 国际化（中/英）
├── processor/           # 数据处理
├── query/               # Dataview 查询
├── render/              # 图表渲染（热力图、日历图等）
├── view/                # React 组件
└── util/                # 工具函数
```

## 📄 License

[MIT](LICENSE)

## 🙏 致谢

基于 [Obsidian](https://obsidian.md/) 插件 API 构建。

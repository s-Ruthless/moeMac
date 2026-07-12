# 更新日志

## v2.1.0 (2026-07-12)

### 性能优化

- **Lighthouse 性能大幅提升** — 非首屏 CSS 异步加载（`media="print" onload`），KaTeX / APlayer / Mermaid JS 按需懒加载，LCP 图片（头像、壁纸）`preload` 预加载，Masonry 核心 JS 预加载
- **Masonry 布局安全网** — 添加 CSS `load` 事件监听和 `window.load` 重排，确保异步 CSS 加载后瀑布流正确布局

### Bug 修复

- **归档折叠抖动** — 将 `max-height` 动画改为 CSS Grid `grid-template-rows` 动画，消除移动端折叠时的屏幕抖动
- **移动端 Dock Tooltip 遮挡** — 移动端移除 Dock hover 提示，JS 跳过事件绑定
- **移动端封面图非正方形** — 固定文章列表卡片封面图高度为 90px
- **Postcard 卡片布局** — 缩略图固定 100×100 正方形，消除多卡片模式下图片下方空白
- **豆瓣卡片 hover 模糊** — 添加 `will-change: transform` 和 `backface-visibility: hidden`，修复字体模糊和图片卡顿
- **豆瓣电影评分抓取** — 优先获取个人评分，回退社区评分，修正 `allstarXX` 转换公式，支持 `data-rating` 新格式
- **画廊 JS 加载延迟** — 优化 `initGallery` 初始化时序，使用 `requestAnimationFrame` 确保 DOM 渲染后正确布局
- **文章列表页卡片错位** — 关键布局 CSS `pages.css` 改回同步加载，避免 Masonry 计算错误

### 变更

- **豆瓣页面移除音乐模块** — 从模板和抓取脚本中移除音乐相关逻辑
- **KaTeX / APlayer 懒加载** — 仅检测到数学公式 / 音乐播放器时才加载对应 JS

---

## v2.0.0 (2026-07-12)

### 重磅更新

本次为重大版本升级，涵盖 SEO 优化、内容创作工具链、性能优化、CSP 安全修复等多个方面。

#### 新增功能

- **SEO 短链接（abbrlink）** — 内置 `scripts/abbrlink.js`，根据文章标题自动生成 CRC32 哈希短链接，URL 稳定不变，无需安装任何插件
- **RSS 订阅** — 内置 `scripts/rss.js`，自动输出标准 RSS 2.0 格式的 `atom.xml`，支持全文/摘要、文章数量限制、主页社交图标区自动显示 RSS 按钮
- **站点地图** — 内置 `scripts/sitemap.js`，自动生成 `sitemap.xml`，包含文章、页面、分类、标签、归档页，可提交到 Google Search Console / 百度站长平台
- **KaTeX 数学公式** — 内置 `scripts/katex-render.js`，支持行内 `$...$` 和块级 `$$...$$` 公式，CSS/JS/字体全部本地化
- **Mermaid 图表** — 内置 `scripts/mermaid-render.js`，支持流程图、时序图、甘特图等，使用 `mermaid` 代码块语法
- **标签外挂系统** — 内置 `scripts/tag-plugins.js` + `scripts/container-blocks.js`，提供提示容器、标签页、折叠面板、轮播图、画廊、时间线、步骤条、文章引用卡片等丰富的标签外挂
- **内置 TOC 目录** — `scripts/helpers.js`，不依赖 hexo-toc 插件
- **瀑布流布局** — 集成 Masonry（本地化），用于文章墙卡片和相册

#### 性能优化

- **动画系统重构** — 移除 GSAP 依赖，改用 CSS @keyframes + IntersectionObserver + Web Animations API，零外部依赖
- **JS 加载优先级优化** — `defer` 脚本按优先级排序，核心脚本添加 `preload`，CSS 加载完成检查防止动画重放
- **资源预连接** — 自动从用户配置中提取外部域名，生成 `preconnect` + `dns-prefetch` 标签
- **CSS/JS 版本号** — 统一版本号参数强制浏览器缓存刷新

#### 安全修复

- **CSP 合规** — 修复 `main.js` 中 AJAX 导航使用 `new Function()` 执行 `onerror` 字符串的问题，改为正则解析
- **CSP 合规** — 修补 `APlayer.min.js` 中 webpack 引导代码的 `Function("return this")()` 和 `eval("this")`，直接替换为 `window` 引用

#### 静态资源全本地化

所有第三方 CSS / JS / 字体文件均已下载到 `source/assets/` 目录，不依赖任何外部 CDN：

| 资源 | 说明 |
|---|---|
| APlayer + Meting | 音乐播放器 |
| KaTeX（含 20 个 woff2 字体） | 数学公式渲染 |
| Mermaid | 图表渲染 |
| Masonry | 瀑布流布局 |
| Font Awesome | 图标字体 |
| Gitalk / Waline / Twikoo / CWD | 评论系统 |
| 卜算子 | 访客统计 |

> 唯一的外部引用是 Giscus 评论系统的 `giscus.app/client.js`（官方要求不可本地化）。

#### UI / UX 改进

- **分类图标统一** — 所有分类统一使用 `fas fa-folder` 图标
- **卡片布局优化** — 文章墙改为 3 列网格布局，置顶徽章+分类标签+日期并排显示不再换行
- **代码块行号对齐** — 修复代码块行号与内容上下不对齐的问题
- **语法高亮增强** — 启用 `auto_detect`，补充浅色和深色模式下的 token 类型 CSS 规则
- **AJAX 导航画廊修复** — 修复 AJAX 导航后画廊图片布局异常，优化 `initGallery` 初始化逻辑
- **暗黑模式** — 早期注入 `<script>` 在所有 CSS/JS 之前读取 localStorage，避免 FOUC 闪烁
- **移动端适配** — 独立布局、触摸友好、自动检测视口切换

#### 文档

- **README.md** — 精简为项目介绍和快速上手指南，详细配置指向在线文档
- **使用说明** — 新增插件分类说明（hexo init 自带 / 手动安装 / 无需插件）、abbrlink、RSS、Sitemap 配置文档

#### 破坏性变更

- `permalink` 需改为 `posts/:abbrlink.html`（如使用 abbrlink 功能）
- 移除 GSAP 依赖（`gsap.min.js`、`gsap-animations.js`、`ScrollTrigger.min.js`），由原生动画替代
- 移除 `pinyin-pro` 依赖

---

## v1.1.0

- 修复荣耀浏览器窗口不显示问题
- 移动端相册瀑布流改为 3 列
- 修复评论 null 报错
- 动态预连接优化
- 移动端兼容性改进

## v1.0.0

- 初始发布
- macOS 桌面风格、Dock 导航、浮动窗口、毛玻璃质感
- 暗黑模式、AJAX 导航、音乐播放器
- Giscus / Waline / Twikoo / Gitalk 评论系统
- 豆瓣书影音、相册、归档统计

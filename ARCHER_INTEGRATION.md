# Gmeek + Archer 侧边栏集成方案

## 📋 方案说明

这个方案将 Hexo Archer 主题的**左侧文章归档**和**右侧 TOC 目录**集成到 Gmeek 博客框架中，保持 Gmeek 的轻量特性，同时获得 Archer 的优雅视觉效果。

## ✨ 功能特性

### 左侧边栏（文章归档）
- ✅ 按年份分组显示所有文章
- ✅ 显示文章发布日期（MM/DD 格式）
- ✅ 自动高亮当前文章
- ✅ 粘性定位，滚动时保持可见
- ✅ 自定义滚动条样式

### 右侧边栏（TOC 目录）
- ✅ 自动解析文章标题生成目录
- ✅ 支持 H1-H6 多级标题
- ✅ 平滑滚动定位
- ✅ 当前章节自动高亮
- ✅ 粘性定位跟随滚动

### 响应式设计
- ✅ 桌面端：完整三栏布局
- ✅ 平板端（<1200px）：隐藏右侧 TOC，提供切换按钮
- ✅ 移动端（<900px）：隐藏两侧边栏，提供切换按钮
- ✅ 侧边栏滑入滑出动画效果

## 📁 文件说明

### 1. `base-enhanced.html` - 增强基础模板
完整的三栏布局基础模板，包含：
- 左侧文章归档侧边栏
- 中间主内容区
- 右侧 TOC 目录
- 响应式布局 CSS
- JavaScript 自动化脚本

### 2. `post-enhanced.html` - 文章页模板示例
继承 `base-enhanced.html` 的文章页模板示例。

## 🚀 使用方法

### 方法 1：直接修改 Gmeek.py（推荐）

在 `Gmeek.py` 中找到模板加载部分，修改为使用增强模板：

```python
# 找到这一行（大约在第 100 行左右）
env = Environment(loader=FileSystemLoader('templates'))

# 在生成文章页时使用 post-enhanced.html
# 找到渲染 post 的地方，替换为：
template = env.get_template('post-enhanced.html')
```

### 方法 2：创建独立主题

1. 复制 `templates/` 目录创建新主题：
```bash
cp -r templates templates-archer
cp templates/base-enhanced.html templates-archer/base.html
cp templates/post-enhanced.html templates-archer/post.html
```

2. 修改 `Gmeek.py` 加载主题目录：
```python
env = Environment(loader=FileSystemLoader('templates-archer'))
```

### 方法 3：覆盖原模板（最简单）

**⚠️ 注意：会覆盖原始模板，建议先备份**

```bash
# 备份原模板
cp templates/base.html templates/base.html.backup
cp templates/post.html templates/post.html.backup

# 使用新模板
cp templates/base-enhanced.html templates/base.html
cp templates/post-enhanced.html templates/post.html
```

## 🎨 自定义配置

### 调整布局宽度

在 `base-enhanced.html` 的 `:root` CSS 变量中修改：

```css
:root {
    --sidebar-width: 260px;      /* 左侧边栏宽度 */
    --toc-width: 220px;           /* 右侧 TOC 宽度 */
    --content-max-width: 900px;   /* 主内容最大宽度 */
}
```

### 修改主题色

```css
:root {
    --archer-primary: #0969da;    /* 主题色 */
}
```

### 禁用某个侧边栏

**禁用左侧归档：**
```css
.archer-sidebar-left {
    display: none !important;
}
```

**禁用右侧 TOC：**
```css
.archer-toc-right {
    display: none !important;
}
```

### 调整移动端断点

```css
/* 修改平板断点 */
@media (max-width: 1200px) { /* 改为你想要的宽度 */ }

/* 修改手机断点 */
@media (max-width: 900px) { /* 改为你想要的宽度 */ }
```

## 📊 数据源配置

### 文章列表 JSON

脚本会自动尝试以下路径获取文章列表：
- `/post.json`
- `/posts.json`
- `/index.json`
- `/feed.json`

确保你的 Gmeek 配置生成了其中一个 JSON 文件。

### JSON 格式要求

```json
[
  {
    "title": "文章标题",
    "url": "/post/article.html",
    "date": "2024-01-01T00:00:00Z"
  }
]
```

或者：

```json
{
  "posts": [ /* 文章数组 */ ]
}
```

## 🔧 高级功能

### TOC 平滑滚动

点击目录项会平滑滚动到对应位置，可以调整滚动行为：

```javascript
function smoothScroll(id) {
    event.preventDefault();
    document.getElementById(id).scrollIntoView({
        behavior: 'smooth',  // 改为 'auto' 禁用动画
        block: 'start'       // 'center' 或 'end' 调整位置
    });
}
```

### 自定义 TOC 高亮触发距离

```javascript
// 在 scroll 事件中修改
if (rect.top <= 100) // 改为你想要的像素值
```

## 📱 移动端体验优化

### 自动关闭侧边栏

添加以下代码到 `<script>` 标签中：

```javascript
// 点击链接后自动关闭侧边栏
document.addEventListener('click', (e) => {
    if (e.target.matches('.archive-post-title, .toc a')) {
        setTimeout(() => {
            document.getElementById('archerSidebar').classList.remove('show');
            document.getElementById('archerToc').classList.remove('show');
        }, 300);
    }
});
```

## 🎯 与原 Gmeek 的差异

| 功能 | 原 Gmeek | Archer 增强版 |
|------|---------|-------------|
| 布局 | 单栏 | 三栏（左归档/中内容/右TOC） |
| 文章导航 | 无 | 左侧归档列表 |
| 目录 | 无 | 右侧自动TOC |
| 响应式 | 基础 | 完整（含侧边栏抽屉） |
| 样式 | GitHub Primer | Primer + Archer 风格 |

## 🐛 常见问题

### Q: 左侧归档列表不显示？
A: 检查是否正确生成了 JSON 文件，打开浏览器控制台查看网络请求。

### Q: TOC 目录为空？
A: 确保文章内容包含 `<h1>` - `<h6>` 标题标签。

### Q: 移动端侧边栏无法打开？
A: 检查 JavaScript 是否正确加载，查看浏览器控制台是否有错误。

### Q: 样式错乱？
A: 确保 GitHub Primer CSS 正确加载，检查 `{{ blogBase['primerCSS'] }}` 变量。

## 📄 许可证

本方案基于：
- Gmeek（原项目许可证）
- Hexo Archer 主题（MIT License）

使用时请保留原项目版权信息。

## 🙏 致谢

- [Gmeek](https://github.com/Meekdai/Gmeek) - 超轻量级博客框架
- [Hexo Archer](https://github.com/fi3ework/hexo-theme-archer) - 优雅的 Hexo 主题

---

**创建时间**: 2026-01-02
**版本**: v1.0.0

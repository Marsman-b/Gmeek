# 快速安装指南

## 🎯 最简单的方法（推荐）

### 步骤 1：备份原模板

```bash
cd D:\zhuomian\gmeek\Gmeek\templates
copy base.html base.html.backup
copy post.html post.html.backup
```

### 步骤 2：应用 Archer 风格模板

**选项 A：仅应用到文章页（推荐新手）**

将 `post.html` 替换为使用增强模板：

```bash
copy post-enhanced.html post.html
```

修改 `post.html` 第一行：
```jinja
{% extends "base.html" %}
```
改为：
```jinja
{% extends "base-enhanced.html" %}
```

**选项 B：应用到所有页面**

```bash
copy base-enhanced.html base.html
copy post-enhanced.html post.html
```

### 步骤 3：重新生成博客

在 Gmeek 根目录运行：

```bash
python Gmeek.py
```

或者通过 GitHub Actions 触发全局重新生成。

## 🎨 效果预览

### 桌面端（三栏布局）
```
┌─────────────┬──────────────────────┬──────────────┐
│  文章归档    │     主要内容          │   TOC 目录   │
│             │                      │             │
│  2024       │  # 文章标题           │  📑 CATALOG │
│  ├ 01/15 文章│  文章正文...          │  1. 章节 1  │
│  ├ 01/10 文章│                      │  2. 章节 2  │
│  2023       │                      │    2.1 小节 │
│  ├ 12/20 文章│                      │  3. 章节 3  │
└─────────────┴──────────────────────┴──────────────┘
```

### 移动端
- 左右侧边栏自动隐藏
- 顶部显示切换按钮
- 点击按钮滑出侧边栏

## ⚙️ 快速自定义

### 修改宽度

在 `base-enhanced.html` 找到 `:root` 部分（约第 30 行）：

```css
:root {
    --sidebar-width: 260px;      /* 左侧栏宽 → 改为你想要的值 */
    --toc-width: 220px;           /* 右侧栏宽 → 改为你想要的值 */
    --content-max-width: 900px;   /* 内容宽度 → 改为你想要的值 */
}
```

### 修改主题色

```css
:root {
    --archer-primary: #0969da;    /* 改为你喜欢的颜色 */
}
```

### 只保留左侧归档（隐藏 TOC）

在 `<style>` 标签最后添加：

```css
.archer-toc-right, .toc-toggle {
    display: none !important;
}
```

### 只保留右侧 TOC（隐藏归档）

在 `<style>` 标签最后添加：

```css
.archer-sidebar-left, .sidebar-toggle {
    display: none !important;
}
```

## 🔍 验证安装

### 1. 检查文件是否创建成功

```bash
ls D:\zhuomian\gmeek\Gmeek\templates\base-enhanced.html
ls D:\zhuomian\gmeek\Gmeek\templates\post-enhanced.html
```

### 2. 在浏览器打开生成的文章

打开浏览器开发者工具（F12），查看控制台应该看到：

```
Gmeek x.x.x + Archer Style
```

### 3. 检查功能

- [ ] 左侧显示文章列表（按年份分组）
- [ ] 右侧显示文章目录
- [ ] 点击目录项平滑滚动
- [ ] 当前章节高亮显示
- [ ] 移动端显示切换按钮

## ❓ 常见问题排查

### 问题：左侧归档列表显示"暂无文章"

**原因**：未找到文章列表 JSON

**解决**：
1. 检查是否生成了 `post.json` 或 `posts.json`
2. 打开浏览器开发者工具，查看网络请求
3. 确认 JSON 文件路径是否正确

### 问题：右侧 TOC 显示"暂无目录"

**原因**：文章没有标题（h1-h6）

**解决**：
确保文章 Markdown 包含标题，例如：
```markdown
# 主标题
## 二级标题
### 三级标题
```

### 问题：样式显示异常

**原因**：CSS 冲突或 Primer CSS 未加载

**解决**：
1. 检查 `{{ blogBase['primerCSS'] }}` 是否正确输出
2. 清除浏览器缓存
3. 检查是否有自定义 CSS 覆盖了样式

### 问题：JavaScript 不工作

**原因**：脚本加载失败或语法错误

**解决**：
1. 打开浏览器控制台查看错误信息
2. 确认 `{{ IconList }}` 等变量是否正确传递
3. 检查 Jinja2 模板语法是否正确

## 📊 与原版对比

| 特性 | 原 Gmeek | Archer 增强版 | 改进 |
|------|---------|-------------|------|
| 文章导航 | ❌ | ✅ 左侧归档列表 | 更易浏览 |
| 文章目录 | ❌ | ✅ 右侧 TOC | 长文阅读友好 |
| 布局 | 单栏 | 三栏 | 信息密度更高 |
| 响应式 | 基础 | 完整 | 移动端体验优化 |
| 代码量 | 极简 | 适中 | 功能更丰富 |

## 🔄 回退到原版

如果不满意，可以轻松回退：

```bash
cd D:\zhuomian\gmeek\Gmeek\templates
copy base.html.backup base.html
copy post.html.backup post.html
```

然后重新生成博客即可。

## 📞 获取帮助

遇到问题？
1. 查看 `ARCHER_INTEGRATION.md` 详细文档
2. 检查浏览器控制台错误信息
3. 确认 Gmeek 原版功能正常

---

**安装完成后，享受 Archer 风格的优雅博客体验吧！** 🎉

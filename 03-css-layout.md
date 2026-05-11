# 第 3 节 · 用 Flex 把衣柜排成网格

> 目标：让衣柜从"一列一行"变成"卡片网格"，并在手机上自动变单列。
> **本节完成路线图功能点**：#1 衣物列表展示（最终布局）、#2 分类管理（导航栏视觉）

---

## 一、先聊"盒子"——块级 vs 行内

每个标签都是个"盒子"，但浏览器对它们的默认排版不同：

| 类型 | 行为 | 例子 |
| --- | --- | --- |
| **block** | 自己占一行，宽度自动撑满 | `<div>` `<p>` `<h1>` `<ul>` |
| **inline** | 跟旁边内容挤在同一行 | `<span>` `<a>` `<img>` |

用 `display` 属性可以改：

```css
display: block;          /* 块级 */
display: inline;         /* 行内 */
display: inline-block;   /* 既能并排，又能设宽高 */
```

上一节衣物卡片每个占一行（块级默认行为）。这节用 Flex 把它们排成网格。

## 二、Flex 是什么

Flex 让一个**容器**里的**子元素**按某种规则横向/纵向排列、自动伸缩。

**两个角色**：
- **容器**：加了 `display: flex` 的元素
- **项目**：容器**直接的子元素**

## 三、Flex 容器的 5 个核心属性

写在容器上：

```css
.container {
  display: flex;              /* 开启 Flex */
  flex-direction: row;        /* 主轴方向：row 横向 / column 纵向 */
  justify-content: center;    /* 主轴对齐：start / center / end / space-between */
  align-items: center;        /* 交叉轴对齐：start / center / end / stretch */
  gap: 16px;                  /* 项目之间的间距 */
  flex-wrap: wrap;            /* 项目超出宽度时换行 */
}
```

记忆方式：**direction 管方向、justify-content 管主轴、align-items 管交叉轴、gap 管间距、wrap 管换行。**

## 四、动手 1：把衣物排成网格

打开 `my-wardrobe.html`，给所有 `.card` 套一个外层 `<div class="grid">`：

```html
<div class="grid">
  <div class="card">...</div>
  <div class="card">...</div>
  <div class="card">...</div>
  <div class="card">...</div>
</div>
```

CSS 加：

```css
.grid {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
  justify-content: center;
}

.card {
  width: 220px;       /* 之前是 max-width: 320，改成固定宽度便于排列 */
  /* 其它样式保留 */
}
```

刷新浏览器：卡片应该自动排成多列，整体居中。

## 五、动手 2：顶部分类导航栏

`<body>` 顶部加：

```html
<nav class="nav">
  <a href="#" class="active">全部</a>
  <a href="#">上衣</a>
  <a href="#">裤子</a>
  <a href="#">鞋</a>
  <a href="#">外套</a>
  <a href="#">配饰</a>
</nav>
```

CSS：

```css
.nav {
  display: flex;
  gap: 12px;
  justify-content: center;
  padding: 16px;
  background-color: white;
  border-radius: 8px;
  margin-bottom: 24px;
}

.nav a {
  text-decoration: none;
  color: #555;
  padding: 6px 12px;
  border-radius: 6px;
  transition: background-color 0.2s;
}

.nav a:hover {
  background-color: #f0f0f0;
}

.nav a.active {
  background-color: #1a73e8;
  color: white;
}
```

> 这节只做**视觉**，下节学 JS 才能真的切换分类。`.active` 类先手动加到"全部"上。

## 六、响应式：手机上变单列

手机屏幕窄，多列排不下。用 `@media` 写条件样式：

```css
@media (max-width: 600px) {
  .card {
    width: 100%;       /* 单列：每个卡片占满 */
  }

  .nav {
    flex-wrap: wrap;   /* 导航允许换行 */
  }
}
```

意思："屏幕宽度 ≤ 600px 时，应用以下样式。"

把浏览器窗口拖窄到 600px 以下看看效果。

## 七、Grid 是什么（认识一下）

Grid 是另一种布局工具，更适合**二维网格**。简单例子：

```css
.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(220px, 1fr));
  gap: 16px;
}
```

效果：自动按容器宽度填充 220px 一列。比 Flex 写网格更简洁。

**什么时候用 Grid？** 严格的二维布局（仪表盘、相册）。日常 90% 场景 Flex 够用。本节作业仍用 Flex。

---

# 📌 课后作业

**完成路线图功能点 #1 衣物列表展示（最终布局）+ #2 分类管理（导航栏视觉）**

## 题目：把衣柜首页改成响应式网格 + 加分类导航

要求：

1. 用 Flex 把衣物卡片排成至少 2 列的网格
2. 顶部加分类导航栏（"全部 / 上衣 / 裤子 / 鞋 / 外套 / 配饰"）
3. 给"全部"加 `.active` 类做出选中态
4. 加一条 `@media` 规则：屏幕 ≤ 600px 时网格变单列、导航换行
5. 卡片和导航项都要有 `:hover` 反馈

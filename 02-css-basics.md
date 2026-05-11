# 第 2 节 · 用 CSS 给衣柜换上"皮肤"

> 目标：让上一节那个朴素的衣柜页面变好看——配色、字体、卡片样式、悬浮反馈。
> **本节完成路线图功能点**：#1 衣物列表展示（卡片样式美化）

---

## 一、CSS 是什么

HTML 只负责"有什么内容"，**不负责好不好看**。让网页好看，是 **CSS** 的活儿。

> 类比：HTML 是"毛坯结构"，CSS 是"装修"——刷漆、铺地板、挂灯。

## 二、CSS 长什么样

记住这一条公式：

```css
选择器 {
  属性: 值;
  属性: 值;
}
```

例子：

```css
h1 {
  color: red;
  font-size: 32px;
}
```

意思："所有 `<h1>` 标签，字色红色、字号 32 像素。"

- **选择器** → 作用在**哪些元素**上
- **属性** → 改它的**什么**
- **值** → 改成**什么样**
- 每条规则末尾加 `;`，整段用 `{}` 包

## 三、CSS 怎么"接到" HTML 上

三种方式，**这节只用第三种**：

### 方式 1：内联（不推荐）

```html
<h1 style="color: red;">Hello</h1>
```

每个元素都要单独写，没法维护。

### 方式 2：内部（小项目可用）

```html
<head>
  <style>
    h1 { color: red; }
  </style>
</head>
```

样式只对**这一个 HTML 文件**生效。

### 方式 3：外部样式表（推荐 ⭐️）

新建 `style.css` 写样式，HTML 用 `<link>` 引入：

```html
<head>
  <link rel="stylesheet" href="style.css" />
</head>
```

**多文件共用、HTML/CSS 分离、行业标准做法。**

## 四、3 种最常用的选择器

### 1. 标签选择器

所有同名标签一起改：

```css
p { color: gray; }
```

### 2. class 选择器（最常用 ⭐️）

HTML 加 `class` 属性，CSS 用 `.类名` 匹配：

```html
<p class="highlight">这段是重点</p>
<p>这段是普通段落</p>
```

```css
.highlight {
  color: orange;
  font-weight: bold;
}
```

> **class 是日常开发中用得最多的选择器**，多用。

### 3. id 选择器（少用）

ID 在整个页面里**必须唯一**，用 `#` 匹配：

```css
#page-title { color: blue; }
```

> 新手优先用 class，不要乱用 id。

## 五、常用 CSS 属性速查

不用全背，写的时候回来查。

### 文字

```css
color: #333;
font-size: 16px;
font-weight: bold;
font-family: "Helvetica", "PingFang SC", sans-serif;
text-align: center;
line-height: 1.6;
```

### 背景与边框

```css
background-color: #f5f5f5;
border: 1px solid #ddd;
border-radius: 8px;
```

### 盒子间距

```css
width: 200px;
height: 100px;
padding: 16px;    /* 内边距 */
margin: 24px;     /* 外边距 */
```

> 颜色写法：`red`、`#ff0000`、`#f00`、`rgb(255,0,0)` 都行，日常常用 `#xxxxxx`。

## 六、盒模型

每个 HTML 元素都是个**盒子**，从内到外有 4 层：

```
┌─────────── margin（外边距）────────────┐
│  ┌─────── border（边框）──────────┐   │
│  │  ┌── padding（内边距）─────┐   │   │
│  │  │     content（内容）    │   │   │
│  │  └──────────────────────────┘   │   │
│  └────────────────────────────────┘   │
└────────────────────────────────────────┘
```

新手最常犯的错：分不清该用 `padding` 还是 `margin`。

**口诀**：**内 `padding`，外 `margin`。**

## 七、伪类与过渡：悬浮反馈

`:hover` 表示"鼠标悬浮时"，配合 `transition` 让样式变化平滑：

```css
.card {
  background-color: white;
  transition: transform 0.2s;   /* 所有变化平滑过渡 0.2 秒 */
}

.card:hover {
  transform: translateY(-4px);  /* 鼠标移上去，卡片向上浮 4px */
}
```

这种"轻量动效"是好看网页的关键。

## 八、动手：给衣柜首页化个妆

### 第 1 步：建 style.css

在 `web-101` 文件夹新建 `style.css`，先空着。

### 第 2 步：在 HTML 引入

打开 `my-wardrobe.html`，在 `<head>` 里加：

```html
<link rel="stylesheet" href="style.css" />
```

### 第 3 步：给衣物加 class

把每件衣物包进 `<div class="card">`：

```html
<div class="card">
  <h2>白色 T 恤</h2>
  <img src="..." alt="白色 T 恤" />
  <p>简约百搭，约 200 元</p>
  <a href="...">类似款</a>
</div>
```

### 第 4 步：写 CSS

```css
body {
  font-family: "Helvetica", "PingFang SC", sans-serif;
  background-color: #f4f6f8;
  color: #333;
  line-height: 1.6;
  margin: 0;
  padding: 40px;
}

h1 {
  text-align: center;
  color: #1a73e8;
}

.card {
  background-color: white;
  max-width: 320px;
  margin: 16px auto;
  padding: 16px;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
  transition: transform 0.2s;
}

.card:hover {
  transform: translateY(-4px);
}

.card img {
  width: 100%;
  border-radius: 8px;
}
```

刷新浏览器，应该看到：浅灰背景 + 居中的白色卡片 + 鼠标悬浮时卡片"浮起"。

---

# 📌 课后作业

**完成路线图功能点 #1 衣物列表展示（卡片样式美化）**

## 题目：把衣柜首页装修成"卡片版"

要求：

1. 在 `web-101` 新建 `style.css`，并在 `my-wardrobe.html` 用 `<link>` 引入
2. 至少做到：
   - `<body>` 设置**背景色**（不要纯白）
   - 修改默认**字体颜色**和**字号**
   - 至少用一次 `class` 选择器
   - 每件衣物做成 `class="card"` 的卡片（`padding` + `border` + `border-radius`）
   - 让 `<h1>` 居中（`text-align: center;`）
   - 加 `:hover` 效果让卡片"活"起来

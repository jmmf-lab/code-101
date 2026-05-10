# 第 2 节 · 用 CSS 给页面换上"皮肤"

> 目标：在上一节那个朴素的 HTML 页面基础上，学会用 CSS 改颜色、字体、间距、边框，把页面变好看。

## 学习目标

学完这一节，你应该能做到：

1. 说出 CSS 的作用，以及它和 HTML 的关系
2. 掌握三种把 CSS "接到" HTML 上的方式，并知道**外部样式表**是首选
3. 看懂"选择器 + 属性"这个 CSS 写法的基本套路
4. 会用 8~10 个最常用的 CSS 属性
5. 用 `class` 选择器给不同元素套上不同样式
6. 对**盒模型**（margin / border / padding / content）有初步印象

---

## 一、回顾 + 这节课要解决什么

上一节我们写出了 `hello.html` 和 `about-me.html`，但你应该有这种感觉：

> "字都挤在左上角，没颜色没排版，丑得像 1995 年的网页。"

没错——HTML 只负责"有什么内容"，**不负责好不好看**。
让网页好看，是 **CSS** 的活儿。

> 类比：如果说 HTML 是房子的"毛坯结构"（哪是墙、哪是门），
> 那 CSS 就是"装修"——刷漆、贴瓷砖、铺地板、挂灯。

## 二、CSS 长什么样？

CSS 的基本写法非常死板，记住这一条公式就够：

```css
选择器 {
  属性: 值;
  属性: 值;
}
```

举个例子：

```css
h1 {
  color: red;
  font-size: 32px;
}
```

意思是："把页面里所有的 `<h1>` 标签，文字颜色设成红色，字号设成 32 像素。"

- `h1` 是**选择器**（selector）—— 决定**作用在哪些元素上**
- `color`、`font-size` 是**属性**（property）—— 决定**改它的什么**
- `red`、`32px` 是**值**（value）—— 决定**改成什么样**
- 每条规则末尾要加 `;`，整段用 `{}` 包起来

## 三、CSS 怎么"接"到 HTML 上？

CSS 写好后，必须以某种方式让浏览器知道"这段样式要套用到那个 HTML 上"，一共有三种方式：

### 方式 1：行内样式（不推荐）

直接写在标签的 `style` 属性里：

```html
<h1 style="color: red; font-size: 32px;">Hello</h1>
```

缺点：**写一次只管一个元素**，多了根本没法维护。**学习阶段尽量不用。**

### 方式 2：内部样式表（小项目可用）

写在 HTML 文件 `<head>` 里的 `<style>` 标签里：

```html
<head>
  <style>
    h1 {
      color: red;
    }
  </style>
</head>
```

缺点：样式只对这**一个 HTML 文件**生效。

### 方式 3：外部样式表（推荐 ⭐️）

CSS 单独写在一个 `.css` 文件里，HTML 用 `<link>` 引入：

```html
<head>
  <link rel="stylesheet" href="style.css" />
</head>
```

好处：

- 多个 HTML 文件可以**共用同一份样式**
- HTML 和 CSS **职责分离**，文件干净
- 行业标准做法

> 📌 这一节我们就用方式 3。

## 四、选择器入门：3 种最常用的

CSS 怎么"指定"它要作用在谁身上？靠选择器。先记 3 种：

### 1. 标签选择器

按 HTML 标签名匹配，所有同名标签一起改。

```css
p {
  color: gray;
}
```

意思：所有 `<p>` 段落都变灰色。

### 2. class 选择器（最常用 ⭐️）

先在 HTML 元素上加个 `class` 属性，CSS 里用 `.类名` 来匹配。

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

意思：只有带 `class="highlight"` 的段落才变橙色加粗。

> **class 是日常开发中用得最多的选择器**，推荐养成习惯。

### 3. id 选择器（少用）

类似 class，但用 `#`，并且**一个页面里 id 必须唯一**。

```html
<h1 id="page-title">My Page</h1>
```

```css
#page-title {
  color: blue;
}
```

> 新手阶段优先用 class，不要乱用 id。

## 五、必学的常用 CSS 属性

不用全背，先有印象，写的时候回来查就行。

### 文字相关

| 属性 | 作用 | 例子 |
| --- | --- | --- |
| `color` | 文字颜色 | `color: #333;` |
| `font-size` | 字号 | `font-size: 16px;` |
| `font-weight` | 粗细 | `font-weight: bold;` |
| `font-family` | 字体 | `font-family: "Helvetica", sans-serif;` |
| `text-align` | 对齐 | `text-align: center;` |
| `line-height` | 行高（行间距） | `line-height: 1.6;` |

### 背景与边框

| 属性 | 作用 | 例子 |
| --- | --- | --- |
| `background-color` | 背景色 | `background-color: #f5f5f5;` |
| `border` | 边框 | `border: 1px solid #ddd;` |
| `border-radius` | 圆角 | `border-radius: 8px;` |

### 盒子的间距

| 属性 | 作用 |
| --- | --- |
| `width` / `height` | 宽高 |
| `padding` | **内边距**：内容到边框的距离 |
| `margin` | **外边距**：边框到外面其他元素的距离 |

> 颜色的写法：`red`、`#ff0000`、`#f00`、`rgb(255,0,0)` 都行，
> 日常最常用 `#xxxxxx` 这种 16 进制写法。

## 六、盒模型：CSS 里最重要的一个概念

每个 HTML 元素，浏览器都把它**当成一个盒子**来排版。

每个盒子从内到外有 4 层：

```
┌─────────── margin（外边距） ───────────┐
│  ┌─────── border（边框） ──────────┐   │
│  │  ┌── padding（内边距） ─────┐   │   │
│  │  │      content（内容）     │   │   │
│  │  └─────────────────────────┘   │   │
│  └──────────────────────────────────┘   │
└──────────────────────────────────────────┘
```

- **content**：文字 / 图片本身
- **padding**：内容到边框的距离（撑开盒子内部）
- **border**：边框
- **margin**：和外面其他元素之间的"安全距离"

新手最常犯的错：**想加间距却分不清该用 padding 还是 margin**。

口诀：**内 padding，外 margin。**

## 七、动手：给上节课的 about-me 页面化个妆

### 第 1 步：建一个新的 CSS 文件

在 `web-101` 文件夹里新建一个文件，命名为 `style.css`，先空着。

### 第 2 步：在 HTML 里引入它

打开 `about-me.html`，在 `<head>` 里加上 `<link>`：

```html
<head>
  <meta charset="UTF-8" />
  <title>About Me</title>
  <link rel="stylesheet" href="style.css" />
</head>
```

### 第 3 步：改一下 HTML，加上 class

为了让 CSS 能精准定位元素，给关键元素加 class：

```html
<body>
  <div class="card">
    <h1 class="name">Tom</h1>
    <p class="intro">Hi, I am learning web development.</p>

    <h2>My Hobbies</h2>
    <ul class="hobbies">
      <li>Reading</li>
      <li>Running</li>
      <li>Coding</li>
    </ul>
  </div>
</body>
```

注意：我用一个 `<div class="card">` 把所有内容包起来——
`<div>` 是一个**没有任何含义的容器**，专门用来分组、套样式。

### 第 4 步：写 CSS

把下面的代码贴进 `style.css`，保存后**刷新浏览器**：

```css
/* 整个页面的基础样式 */
body {
  font-family: "Helvetica", "PingFang SC", sans-serif;
  background-color: #f4f6f8;
  color: #333;
  line-height: 1.6;
  margin: 0;
  padding: 40px;
}

/* 名片容器 */
.card {
  background-color: #ffffff;
  max-width: 480px;
  margin: 0 auto;       /* 水平居中：上下 0，左右 auto */
  padding: 24px 32px;
  border: 1px solid #e0e0e0;
  border-radius: 12px;
}

/* 名字 */
.name {
  color: #1a73e8;
  font-size: 28px;
  margin-bottom: 8px;
}

/* 介绍段落 */
.intro {
  color: #555;
  font-size: 16px;
}

/* 爱好列表 */
.hobbies li {
  margin-bottom: 4px;
}
```

刷新后你应该看到：

- 页面背景变成浅灰
- 中间一张白色"卡片"
- 名字是蓝色加大字号
- 内容有了行距和边距，不再挤在一起

> 💡 顺带认识一个写法：`/* 这是注释 */`
> CSS 里的注释用 `/* */` 包起来，不会影响样式，可以用来给自己写说明。

## 八、本节小结

- CSS 的基本结构：`选择器 { 属性: 值; }`
- 最佳引入方式：**外部样式表 + `<link>`**
- 三种选择器：标签 `p`、类 `.xxx`（最常用）、id `#xxx`（少用）
- 常用属性：`color` / `font-size` / `background-color` / `border` / `padding` / `margin` / `border-radius` / `text-align`
- 盒模型四层：content / padding / border / margin —— **内 padding，外 margin**

## 九、下节预告

页面好看了，但还是"死"的——按钮按了没反应，文字不会变。
下一节我们引入 **JavaScript**，让页面"活"起来：能点击、能弹窗、能改内容。

---

# 📌 课后作业

## 题目：把你的 `about-me.html` 装修一遍

**要求：**

1. 在 `web-101` 文件夹里新建 `style.css`，并在 `about-me.html` 里用 `<link>` 引入
2. 用 CSS 把页面改成你喜欢的样子，**至少**做到：
   - 给 `<body>` 设一个**背景色**（不要纯白）
   - 改一下默认**字体颜色**和**字号**
   - 至少用一次 `class` 选择器（自己起名字，比如 `highlight`、`card`、`tag`）
   - 给某个块加 `padding`、`border`、`border-radius`，做出"卡片"效果
   - 让标题居中（`text-align: center;`）

## 提交方式

把 `about-me.html` 和 `style.css` 一起发给我，附一张 Chrome 打开后的截图。

## 加分项（可选）

- 鼠标悬停在链接上时换个颜色（提示：`a:hover { color: ...; }`）
- 把图片做成圆形（提示：`border-radius: 50%;` + 固定宽高）
- 给整个 `.card` 加一点阴影（提示：`box-shadow: 0 2px 8px rgba(0,0,0,0.1);`）

## 友情提示

- CSS 的修改**保存后必须刷新浏览器**才能看到效果（Mac：`Cmd + R`，Win：`F5`）
- **每条样式后面要加分号 `;`**，新手最容易漏
- 颜色不知道怎么挑？打开 [https://coolors.co/](https://coolors.co/) 随便选一套
- 写不出来不要硬扛，把代码发给我，我们一起看

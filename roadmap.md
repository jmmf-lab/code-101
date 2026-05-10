# Web 零基础教学路线图

面向**完全零基础**的朋友，目标是用 8 节课把人带到"能独立做出一个像样的小项目，并能调用网络接口、把页面发布到网上"的水平。

每节课预计学习时长 1~1.5 小时（含作业），每周 1~2 节比较合适。

---

## 总体节奏

```
第 1~3 节：把页面"画"出来    （HTML / CSS）
第 4~5 节：让页面"动"起来    （JavaScript）
第 6 节：处理用户输入与数据
第 7 节：综合实战项目
第 8 节：连后端、上线
```

---

## 第 1 节 · 认识 Web，写出你的第一个网页 ✅

- **关键概念**：什么是网页 / 浏览器在做什么 / HTML、CSS、JS 三者分工
- **关键标签**：`h1`~`h6`、`p`、`a`、`img`、`ul`/`li`
- **工具**：Chrome、VS Code
- **动手产出**：`hello.html`、`about-me.html`
- **作业**：写一个个人介绍页 `about-me.html`

文件：[01-web-basics.md](01-web-basics.md)

---

## 第 2 节 · 用 CSS 给页面换上"皮肤" ✅

- **关键概念**：选择器、属性、值；外部样式表；盒模型
- **三种选择器**：标签、`class`、`id`
- **常用属性**：`color`、`font-size`、`background-color`、`border`、`padding`、`margin`、`border-radius`、`text-align`
- **动手产出**：给 `about-me.html` 加上 `style.css`，做出"卡片"效果
- **作业**：装修 `about-me.html`，做出"名片"风格

文件：[02-css-basics.md](02-css-basics.md)

---

## 第 3 节 · CSS 布局：让页面排得整齐

- **关键概念**：块级 vs 行内元素、Flex 布局、居中、简单的响应式
- **关键属性**：`display: flex`、`justify-content`、`align-items`、`gap`、`flex-direction`
- **动手产出**：用 Flex 做一个顶部导航栏 + 三列卡片
- **作业**：把 `about-me.html` 改造成"主页 + 三个项目卡片"的布局

> 留个伏笔：响应式（手机/电脑都能看）后面再说，本节先用固定宽度搞定。

---

## 第 4 节 · JavaScript 入门：会写一段会"算"的代码

- **关键概念**：什么是 JS、`<script>` 标签、控制台 (Console)
- **基础语法**：变量 (`let`/`const`)、数字/字符串/布尔、`if/else`、循环 (`for`)
- **函数**：怎么写、怎么调用、参数和返回值
- **动手产出**：在控制台里写小程序——猜数字、九九乘法表
- **作业**：写一个函数，输入 1~7 返回对应的星期几中文

> 这一节**不操作页面**，只在 Console 里玩 JS，先把语言本身学清楚。

---

## 第 5 节 · 让页面"动"起来：DOM 与事件

- **关键概念**：DOM（页面的 JS 表示）、事件（点击、输入）
- **核心 API**：`document.querySelector`、`element.addEventListener`、`element.textContent`、`element.classList`
- **动手产出**：
  - 点按钮换页面颜色
  - 点按钮把内容显示/隐藏
- **作业**：做一个"切换昼夜模式"按钮，点一下整个页面变深色，再点变回浅色

---

## 第 6 节 · 表单、用户输入与本地存储

- **关键概念**：`<input>`、`<form>`、`<button>`、`localStorage`
- **核心 API**：`input.value`、`event.preventDefault()`、`localStorage.setItem` / `getItem`
- **动手产出**：一个"留言板"——输入文字点提交，下面就出现一条留言
- **作业**：让留言刷新页面后**还在**（用 `localStorage` 存）

---

## 第 7 节 · 综合实战：做一个 Todo List

- **目标**：把前 6 节的知识全部串起来
- **要做的功能**：
  - 输入框 + 添加按钮
  - 列表展示所有 todo
  - 每条 todo 可以"完成"（划线）或"删除"
  - 刷新后数据还在
- **重点**：怎么把一个稍大的功能**拆成几个小函数**来实现
- **作业**：在自己的 Todo List 上加一个新功能（自选），比如"清空已完成"、"剩余条数统计"

---

## 第 8 节 · 接入网络数据 + 把网页发布到网上

- **关键概念**：API、JSON、`fetch`、异步、HTTP 状态码（粗略）
- **核心 API**：`fetch(url).then(...)` 或 `async/await`
- **动手产出**：
  - 用免费 API（比如 `https://jsonplaceholder.typicode.com/`）拉一组数据展示在页面上
  - 把项目用 [GitHub Pages](https://pages.github.com/) 或 [Vercel](https://vercel.com/) 一键发布，得到一个公网链接
- **作业**：做一个"随机笑话"或"随机猫图"页面，点按钮调用 API 换内容，并把它发布到公网

---

## 学完这 8 节，你应该可以做到：

- 看着一个简单网页能大致猜出它的 HTML/CSS 结构
- 独立写出一个**带交互、能持久化数据**的小工具（Todo / 留言板 / 计算器之类）
- 能读懂 API 文档，调一个公开 API 把数据搬到自己页面上
- 把自己的作品**发布到公网**，给朋友发一个链接就能访问

---

## 后续可选方向（路线图之外）

如果学到第 8 节还想继续，下一步通常是任选一条：

- **现代框架方向**：React / Vue 任选其一，从"组件化"开始重写 Todo List
- **后端方向**：Node.js + Express，自己写一个 API 给前端调
- **工程化方向**：Git/GitHub、包管理 (npm)、构建工具 (Vite)、TypeScript
- **设计方向**：Figma 入门 + Tailwind CSS

这些不在本路线图内，等前 8 节扎实了再考虑。

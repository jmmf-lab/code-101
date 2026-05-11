# 第 5 节 · DOM + 事件：让分类能点

> 目标：把衣物数据搬到 JS，由 JS 动态生成卡片；点击分类导航能筛选。
> **本节完成路线图功能点**：#1 衣物列表展示（动态版）、#2 分类管理、#5 按分类筛选

---

## 一、DOM 是什么

浏览器把 HTML 解析后，在内存里建一棵"树"（DOM Tree），每个标签是一个节点。JS 通过 DOM API 读取/修改这棵树，页面就会变。

## 二、查询元素

```js
// 选第一个匹配的元素
const title = document.querySelector("h1");
const nav = document.querySelector(".nav");
const grid = document.querySelector("#grid");

// 选所有匹配的元素（返回类数组对象）
const allCards = document.querySelectorAll(".card");
```

选择器语法和 CSS 一样：标签名、`.类名`、`#id`。

## 三、修改元素

```js
// 改文字
title.textContent = "Tom 的衣柜";

// 改样式（直接改某个属性）
title.style.color = "red";

// 加/去 class（推荐这种方式改样式）
card.classList.add("active");
card.classList.remove("active");
card.classList.toggle("active");

// 替换整段 HTML
grid.innerHTML = "<p>暂无衣物</p>";
```

## 四、事件监听：让元素响应点击

```js
const btn = document.querySelector(".nav a");

btn.addEventListener("click", (event) => {
  console.log("被点了！");
});
```

`event` 是事件对象，常用：
- `event.target`：被点的元素
- `event.preventDefault()`：阻止默认行为（比如 `<a>` 默认会跳转）

## 五、用模板字符串渲染列表

JS 字符串用反引号 \`\` 时可以嵌入变量，叫**模板字符串**：

```js
const item = { name: "白色 T 恤", price: 200 };

const html = `
  <div class="card">
    <h2>${item.name}</h2>
    <p>${item.price} 元</p>
  </div>
`;
```

`${...}` 里能写任何 JS 表达式。

## 六、动手 1：让衣柜由 JS 渲染

### 第 1 步：清空 HTML 里的硬编码卡片

`my-wardrobe.html` 的 `.grid` 留空：

```html
<div class="grid" id="grid"></div>
```

### 第 2 步：script.js 写衣物数组 + 渲染函数

```js
const wardrobe = [
  { name: "白色 T 恤", category: "上衣", img: "https://...", price: 200 },
  { name: "牛仔裤", category: "裤子", img: "https://...", price: 350 },
  { name: "白鞋", category: "鞋", img: "https://...", price: 600 },
  { name: "黑色外套", category: "外套", img: "https://...", price: 800 },
];

const grid = document.querySelector("#grid");

function render(items) {
  grid.innerHTML = items
    .map(
      (item) => `
      <div class="card">
        <img src="${item.img}" alt="${item.name}" />
        <h2>${item.name}</h2>
        <p>${item.category} · ${item.price} 元</p>
      </div>
    `
    )
    .join("");
}

render(wardrobe);
```

刷新页面，应该看到衣物全部由 JS 渲染出来。

> `items.map(...).join("")` 是常用套路：把数组每项变成 HTML 字符串，再拼接成一整段。

## 七、动手 2：让分类导航能点

```js
const navLinks = document.querySelectorAll(".nav a");

navLinks.forEach((link) => {
  link.addEventListener("click", (event) => {
    event.preventDefault();

    // 切换高亮
    navLinks.forEach((l) => l.classList.remove("active"));
    link.classList.add("active");

    // 筛选
    const category = link.textContent;
    if (category === "全部") {
      render(wardrobe);
    } else {
      const filtered = wardrobe.filter((item) => item.category === category);
      render(filtered);
    }
  });
});
```

刷新页面，点击不同分类，下方应该只显示对应衣物。

---

# 📌 课后作业

**完成路线图功能点 #1 衣物列表展示（动态版）+ #2 分类管理 + #5 按分类筛选**

## 题目：让衣柜由 JS 驱动

要求：

1. 把 HTML 里硬编码的衣物卡片**全部删掉**，留一个空 `<div id="grid"></div>`
2. 在 `script.js` 里建一个 `wardrobe` 数组，至少 5 件衣物，每件含 `name` / `category` / `img` / `price`
3. 写一个 `render(items)` 函数，用模板字符串 + `innerHTML` 把衣物渲染成卡片
4. 顶部分类导航能点：点哪个分类，下面就只显示该分类的衣物
5. 被点击的分类要有 `.active` 高亮样式

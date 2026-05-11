# 第 6 节 · 表单 + localStorage：能添加、能删除

> 目标：让朋友能在页面上添加新衣物、删除衣物，数据存进 `localStorage` 刷新还在。
> **本节完成路线图功能点**：#3 添加新衣物、#4 删除衣物、#7 标签系统

---

## 一、表单元素

```html
<form id="add-form">
  <input type="text" name="name" placeholder="名称" required />
  <select name="category">
    <option value="上衣">上衣</option>
    <option value="裤子">裤子</option>
    <option value="鞋">鞋</option>
    <option value="外套">外套</option>
    <option value="配饰">配饰</option>
  </select>
  <input type="text" name="img" placeholder="图片地址" />
  <input type="number" name="price" placeholder="价格" />
  <input type="text" name="tags" placeholder="标签（逗号分隔，如 简约,百搭）" />
  <button type="submit">添加</button>
</form>
```

- `<input>` 各种输入框（`type` 决定类型：`text` / `number` / `date` / `checkbox`...）
- `<select>` + `<option>` 下拉选择
- `<button type="submit">` 提交按钮（在 form 内点击会触发 form 的 submit 事件）
- `required` 表示必填

## 二、读取表单值

```js
const input = document.querySelector("input[name='name']");
input.value;       // 用户输入的内容
```

## 三、监听表单提交

```js
const form = document.querySelector("#add-form");

form.addEventListener("submit", (event) => {
  event.preventDefault();   // 阻止默认提交（默认会刷新页面）

  const formData = new FormData(form);
  const item = {
    name: formData.get("name"),
    category: formData.get("category"),
    img: formData.get("img"),
    price: Number(formData.get("price")),
    tags: formData.get("tags").split(",").map((s) => s.trim()),
  };

  console.log("新衣物：", item);
});
```

`new FormData(form)` 是个方便的 API：自动把整个 form 里的输入收集成键值对。

## 四、localStorage：浏览器本地存储

浏览器自带的一个"本地存储"，能持久化数据（关掉浏览器也在）。

```js
// 存
localStorage.setItem("name", "Tom");

// 取
localStorage.getItem("name");       // "Tom"

// 删
localStorage.removeItem("name");
```

**但 localStorage 只能存字符串！** 存对象/数组要先**序列化**：

```js
const wardrobe = [{ name: "T 恤" }];

// 存：对象 → JSON 字符串
localStorage.setItem("wardrobe", JSON.stringify(wardrobe));

// 取：JSON 字符串 → 对象
const data = JSON.parse(localStorage.getItem("wardrobe"));
```

记住这一对：`JSON.stringify` 存进去、`JSON.parse` 取出来。

## 五、数组的增删

```js
// 增
wardrobe.push(newItem);

// 删（按位置）
wardrobe.splice(2, 1);   // 从位置 2 开始删 1 个

// 删（按条件）
const index = wardrobe.findIndex((item) => item.name === "T 恤");
wardrobe.splice(index, 1);
```

## 六、动手：让衣柜能添加 + 删除

### 第 1 步：HTML 加表单

`my-wardrobe.html` 在导航下、网格上加 `<form>`（见"一、"部分代码）。

### 第 2 步：script.js 改造

```js
// 启动时从 localStorage 读取
let wardrobe = JSON.parse(localStorage.getItem("wardrobe")) || [];

const grid = document.querySelector("#grid");
const form = document.querySelector("#add-form");

function save() {
  localStorage.setItem("wardrobe", JSON.stringify(wardrobe));
}

function render(items) {
  grid.innerHTML = items
    .map(
      (item, index) => `
      <div class="card">
        <img src="${item.img}" alt="${item.name}" />
        <h2>${item.name}</h2>
        <p>${item.category} · ${item.price} 元</p>
        <p class="tags">${item.tags.join(" · ")}</p>
        <button class="delete-btn" data-index="${index}">删除</button>
      </div>
    `
    )
    .join("");
}

// 添加
form.addEventListener("submit", (event) => {
  event.preventDefault();
  const formData = new FormData(form);
  wardrobe.push({
    name: formData.get("name"),
    category: formData.get("category"),
    img: formData.get("img"),
    price: Number(formData.get("price")),
    tags: formData.get("tags").split(",").map((s) => s.trim()),
  });
  save();
  render(wardrobe);
  form.reset();
});

// 删除（事件委托：监听整个 grid 的点击）
grid.addEventListener("click", (event) => {
  if (event.target.classList.contains("delete-btn")) {
    const index = Number(event.target.dataset.index);
    wardrobe.splice(index, 1);
    save();
    render(wardrobe);
  }
});

render(wardrobe);
```

### 第 3 步：试试

填表单 → 点添加 → 卡片出现 → 刷新 → 还在；点删除按钮 → 卡片消失。

> 💡 `data-index` 是**自定义数据属性**（以 `data-` 开头），JS 用 `element.dataset.xxx` 读取。这是把"位置信息"附在 HTML 元素上的常用方式。

---

# 📌 课后作业

**完成路线图功能点 #3 添加新衣物 + #4 删除衣物 + #7 标签系统**

## 题目：让衣柜能添加、能删除

要求：

1. HTML 加一个 `<form>`，包含字段：名称、分类（`<select>`）、图片地址、价格、标签
2. 提交表单时，把数据加入 `wardrobe` 数组并存进 `localStorage`
3. 每个卡片上加"删除"按钮，点击后从数组移除并更新存储
4. 刷新页面后衣柜数据**仍然在**（用 `JSON.stringify` / `JSON.parse`）
5. 卡片上要显示标签（如 "简约 · 百搭 · 春季"）

# 第 8 节 · 实战下半场：统计 + 穿戴日记

> 目标：给衣柜加"总览统计"卡片，再加一个"穿戴日记"功能。
> **本节完成路线图功能点**：#9 穿戴日记 + 常穿/吃灰统计、#10 衣柜统计

---

## 一、数组聚合：reduce

`reduce` 把数组**累计**成单个值：

```js
const prices = [200, 350, 600];

const total = prices.reduce((sum, current) => sum + current, 0);
// 1150
```

- 第一个参数：累加函数 `(累计值, 当前元素) => 新累计值`
- 第二个参数：初始累计值

衣柜总价：

```js
const totalPrice = wardrobe.reduce((sum, item) => sum + item.price, 0);
```

## 二、按分类统计

```js
const byCategory = wardrobe.reduce((acc, item) => {
  acc[item.category] = (acc[item.category] || 0) + 1;
  return acc;
}, {});

// { "上衣": 3, "裤子": 2, "鞋": 1 }
```

## 三、Date 对象：处理日期

```js
const now = new Date();

now.toISOString();                  // "2026-05-12T10:00:00.000Z"
now.toISOString().slice(0, 10);     // "2026-05-12"（只要日期部分）

// 比较日期
const d1 = new Date("2026-05-12");
const d2 = new Date("2026-04-12");
d1 - d2;                            // 毫秒差
(d1 - d2) / (1000 * 60 * 60 * 24);  // 天数差
```

## 四、日期选择器

HTML 自带的日期输入：

```html
<input type="date" id="diary-date" />
```

用户能用浏览器原生日历选日期。

## 五、动手 1：衣柜总览统计

HTML 加：

```html
<div class="overview" id="overview"></div>
```

JS：

```js
function renderOverview() {
  const total = wardrobe.length;
  const totalPrice = wardrobe.reduce((sum, item) => sum + item.price, 0);

  const byCategory = wardrobe.reduce((acc, item) => {
    acc[item.category] = (acc[item.category] || 0) + 1;
    return acc;
  }, {});

  const categoryList = Object.entries(byCategory)
    .map(([cat, count]) => `${cat}: ${count}`)
    .join(" / ");

  document.querySelector("#overview").innerHTML = `
    <p>共 <strong>${total}</strong> 件，总价 <strong>${totalPrice}</strong> 元</p>
    <p>分类：${categoryList}</p>
  `;
}

renderOverview();
```

> 每次 `wardrobe` 改了（添加/删除/编辑）之后都要调一次 `renderOverview()`。

## 六、动手 2：穿戴日记

### 数据结构

穿戴日记是一个**数组**，每条记录是一天穿了哪几件：

```js
// 在 script.js 顶部
let diary = JSON.parse(localStorage.getItem("diary")) || [];

// 每条记录格式：
// { date: "2026-05-12", itemIndexes: [0, 2, 5] }
```

### HTML

```html
<section class="diary">
  <h2>今天穿了什么？</h2>
  <input type="date" id="diary-date" />
  <div id="diary-checkboxes"></div>
  <button id="diary-save">保存今天</button>
</section>

<section id="diary-stats"></section>
```

### JS

```js
const dateInput = document.querySelector("#diary-date");
const checkboxesDiv = document.querySelector("#diary-checkboxes");

// 默认填今天
dateInput.value = new Date().toISOString().slice(0, 10);

// 渲染每件衣物的勾选框
function renderCheckboxes() {
  checkboxesDiv.innerHTML = wardrobe
    .map(
      (item, index) => `
      <label>
        <input type="checkbox" value="${index}" />
        ${item.name}
      </label>
    `
    )
    .join("");
}

renderCheckboxes();

// 保存今天的穿戴
document.querySelector("#diary-save").addEventListener("click", () => {
  const checked = Array.from(
    checkboxesDiv.querySelectorAll("input:checked")
  ).map((cb) => Number(cb.value));

  diary.push({ date: dateInput.value, itemIndexes: checked });
  localStorage.setItem("diary", JSON.stringify(diary));
  renderStats();
});

// 统计近 30 天每件衣物穿了几次
function renderStats() {
  const now = new Date();
  const recent = diary.filter((d) => {
    const diffDays = (now - new Date(d.date)) / (1000 * 60 * 60 * 24);
    return diffDays <= 30;
  });

  const counts = {};
  recent.forEach((d) => {
    d.itemIndexes.forEach((i) => {
      counts[i] = (counts[i] || 0) + 1;
    });
  });

  // 按穿戴次数从多到少排序
  const sorted = wardrobe
    .map((item, index) => ({ item, count: counts[index] || 0 }))
    .sort((a, b) => b.count - a.count);

  document.querySelector("#diary-stats").innerHTML = `
    <h2>近 30 天穿戴榜</h2>
    <ul>
      ${sorted
        .map(({ item, count }) => `<li>${item.name}：${count} 次</li>`)
        .join("")}
    </ul>
  `;
}

renderStats();
```

---

# 📌 课后作业

**完成路线图功能点 #9 穿戴日记 + 常穿/吃灰统计 + #10 衣柜统计**

## 题目：给衣柜加总览统计 + 穿戴日记

要求：

1. 顶部加"衣柜总览"卡片：显示总件数、总价、各分类件数
2. 加"穿戴日记"区域：用 `<input type="date">` 选日期 + 衣物勾选框 + 保存按钮
3. 点击"保存今天"后，记录存进 `localStorage` 的 `diary` 数组
4. 显示"近 30 天穿戴榜"，按穿戴次数从多到少排序
5. 每次添加/删除/编辑衣物后，总览统计自动刷新

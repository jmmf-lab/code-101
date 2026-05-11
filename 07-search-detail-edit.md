# 第 7 节 · 实战上半场：搜索 + 详情 + 编辑

> 目标：在衣柜首页加搜索框、详情弹窗、编辑功能。
> **本节完成路线图功能点**：#4 编辑衣物、#6 单件详情页、#8 关键词搜索

---

## 一、字符串匹配：判断是否包含

```js
"白色 T 恤".includes("T 恤");   // true

// 不区分大小写
"White Tee".toLowerCase().includes("tee");   // true
```

## 二、数组筛选：filter

`filter(条件)` 返回所有满足条件的元素（不改原数组）：

```js
const adults = users.filter((u) => u.age >= 18);
```

## 三、动手 1：搜索框

HTML 顶部加：

```html
<input type="text" id="search" placeholder="搜索衣物..." />
```

JS 监听 `input` 事件，每次按键就实时筛选：

```js
const searchInput = document.querySelector("#search");

searchInput.addEventListener("input", () => {
  const keyword = searchInput.value.toLowerCase();
  const filtered = wardrobe.filter((item) => {
    return (
      item.name.toLowerCase().includes(keyword) ||
      item.tags.some((t) => t.toLowerCase().includes(keyword))
    );
  });
  render(filtered);
});
```

> `input` 事件每次按键都会触发，所以是"实时筛选"。如果用 `change` 事件，需要失焦才触发。

## 四、弹窗：position 定位 + 遮罩

让一个 `<div>` "浮"在页面之上：

```css
.modal-mask {
  position: fixed;            /* 相对浏览器窗口定位 */
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);   /* 半透明黑遮罩 */
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal {
  background-color: white;
  padding: 24px;
  border-radius: 12px;
  width: 400px;
}
```

- `position: fixed` 让元素脱离正常文档流，按视口定位
- `top` / `left` / `width` / `height` 占满整个屏幕
- 内部用 Flex 把 `.modal` 居中

## 五、动手 2：点卡片打开详情，可编辑

HTML 加一个隐藏的弹窗：

```html
<div class="modal-mask" id="modal" style="display: none;">
  <div class="modal">
    <h2>编辑衣物</h2>
    <form id="edit-form">
      <input type="text" name="name" placeholder="名称" />
      <input type="text" name="category" placeholder="分类" />
      <input type="number" name="price" placeholder="价格" />
      <input type="text" name="tags" placeholder="标签（逗号分隔）" />
      <button type="submit">保存</button>
      <button type="button" id="cancel-btn">取消</button>
    </form>
  </div>
</div>
```

JS：

```js
const modal = document.querySelector("#modal");
const editForm = document.querySelector("#edit-form");
let editingIndex = null;

// 点卡片打开弹窗（事件委托）
grid.addEventListener("click", (event) => {
  // 如果点的是删除按钮，不开弹窗
  if (event.target.classList.contains("delete-btn")) return;

  const card = event.target.closest(".card");
  if (!card) return;

  editingIndex = Number(card.dataset.index);
  const item = wardrobe[editingIndex];

  // 表单回填
  editForm.name.value = item.name;
  editForm.category.value = item.category;
  editForm.price.value = item.price;
  editForm.tags.value = item.tags.join(",");

  modal.style.display = "flex";
});

// 取消
document.querySelector("#cancel-btn").addEventListener("click", () => {
  modal.style.display = "none";
});

// 保存
editForm.addEventListener("submit", (event) => {
  event.preventDefault();
  wardrobe[editingIndex] = {
    ...wardrobe[editingIndex],
    name: editForm.name.value,
    category: editForm.category.value,
    price: Number(editForm.price.value),
    tags: editForm.tags.value.split(",").map((s) => s.trim()),
  };
  save();
  render(wardrobe);
  modal.style.display = "none";
});
```

> `event.target.closest(".card")` 表示"从点击位置往上找最近的 `.card` 元素"。
> `{ ...obj, name: "新名" }` 是**展开运算符**：复制 obj 所有字段，再用新值覆盖 name。

---

# 📌 课后作业

**完成路线图功能点 #4 编辑衣物 + #6 单件详情页 + #8 关键词搜索**

## 题目：搜索 + 详情 + 编辑

要求：

1. 在导航上方加搜索框，输入关键词时**实时**筛选（按名称 + 标签）
2. 点击衣物卡片时弹出**详情弹窗**（用 `position: fixed` + 遮罩）
3. 详情弹窗里用表单回填衣物信息，可以修改并保存
4. 保存后数据更新到 `localStorage`，页面重新渲染
5. 弹窗要有"取消"按钮可关闭

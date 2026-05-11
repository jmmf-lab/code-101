# 第 4 节 · JavaScript 入门：基础语法

> 目标：学会 JS 的核心语法（变量、控制流、函数、数组、对象），末尾用 JS 改一下网页。
> **本节是铺垫节**：不解锁新功能，下节才系统操作页面。

---

## 一、JS 是干嘛的

CSS 让页面好看，**JS 让页面活起来**：点击响应、修改内容、做计算、调接口。

JS 代码在浏览器里执行，可以读取/修改 HTML 内容。

## 二、把 JS 接入 HTML

两种方式：

### 方式 1：写在 `<script>` 标签里

```html
<body>
  <h1>Hello</h1>
  <script>
    console.log("JS 运行了");
  </script>
</body>
```

### 方式 2：外部文件（推荐）

```html
<script src="script.js"></script>
```

新建 `script.js` 文件，里面写 JS 代码。`<script>` 通常放在 `</body>` 之前，保证页面加载完再跑。

## 三、Console：开发者的"控制台"

浏览器有个内置工具叫 **Console**，用来：
- 看 JS 输出
- 输入 JS 代码即时运行
- 看错误信息

打开方式：Chrome 里按 `F12` 或 `Cmd+Option+I`，切到 **Console** 标签。

试试：

```js
console.log("hello");
1 + 2
```

## 四、变量与基本类型

**变量**：给一个值起个名字，方便后面使用。

```js
let name = "Tom";
const age = 28;
```

- `let` 声明的变量**可以改**
- `const` 声明的变量**不可以改**（首选 `const`，确实要变才用 `let`）

**3 种基本类型**：

```js
const name = "Tom";    // 字符串 string
const age = 28;        // 数字 number
const isCool = true;   // 布尔 boolean
```

## 五、控制流：if / for

**条件判断**：

```js
const price = 200;

if (price > 500) {
  console.log("贵");
} else if (price > 100) {
  console.log("适中");
} else {
  console.log("便宜");
}
```

**循环**：

```js
for (let i = 0; i < 5; i++) {
  console.log(i);   // 0, 1, 2, 3, 4
}
```

## 六、函数

**函数**：把一段可重复用的逻辑封装起来。

```js
function greet(name) {
  return "Hello, " + name;
}

greet("Tom");   // "Hello, Tom"
```

也可以用**箭头函数**（更简洁）：

```js
const greet = (name) => "Hello, " + name;
```

## 七、数组：放一堆东西

```js
const hobbies = ["reading", "running", "coding"];

hobbies.length;        // 3
hobbies[0];            // "reading"
hobbies.push("yoga");  // 末尾加一个
```

## 八、对象：用一组键值描述一件事

衣柜里"一件衣物"用对象最自然：

```js
const item = {
  name: "白色 T 恤",
  category: "上衣",
  price: 200,
  color: "white",
};

item.name;     // "白色 T 恤"
item.price;    // 200
```

**数组里装对象** = 多个衣物的列表：

```js
const wardrobe = [
  { name: "白色 T 恤", category: "上衣", price: 200 },
  { name: "牛仔裤", category: "裤子", price: 350 },
  { name: "白鞋", category: "鞋", price: 600 },
];

wardrobe.length;         // 3
wardrobe[0].name;        // "白色 T 恤"
```

## 九、动手 1：Console 里玩衣物数据

打开 Chrome Console，粘贴上面 `wardrobe` 数组，然后试试：

```js
// 算总价
let total = 0;
for (let i = 0; i < wardrobe.length; i++) {
  total += wardrobe[i].price;
}
console.log(total);   // 1150

// 找出所有上衣
function getTops(items) {
  const result = [];
  for (const item of items) {
    if (item.category === "上衣") {
      result.push(item);
    }
  }
  return result;
}
getTops(wardrobe);
```

## 十、动手 2：小试身手——用 JS 改页面

最后 10-15 分钟体验下 DOM。在 `my-wardrobe.html` 的 `</body>` 之前加：

```html
<div id="count"></div>

<script>
  const wardrobe = [
    { name: "白色 T 恤", price: 200 },
    { name: "牛仔裤", price: 350 },
    { name: "白鞋", price: 600 },
  ];

  document.title = "Tom 的衣柜 · " + wardrobe.length + " 件";
  document.getElementById("count").textContent =
    "衣柜里共有 " + wardrobe.length + " 件衣物";

  alert("欢迎来到 Tom 的衣柜");
</script>
```

刷新页面，应该看到：
- 弹窗"欢迎来到 Tom 的衣柜"
- 浏览器标签页标题变了
- 页面底部多了一行"衣柜里共有 3 件衣物"

下节系统学 DOM。

---

# 📌 课后作业

**本节是铺垫节，不对应新功能点。**

## 题目：用 JS 给衣柜算几个统计值

打开 Chrome Console，粘贴你自己衣柜的数据（一个 `wardrobe` 数组，每件至少含 `name`、`category`、`price`），然后写代码完成：

要求：

1. 算出衣柜总件数
2. 算出衣柜总价
3. 用 `for` 循环打印出所有衣物名字
4. 写一个函数 `filterByCategory(items, category)`，返回某个分类下的所有衣物
5. **小试身手**：在 `my-wardrobe.html` 里加 `<script>`，把衣柜件数显示到页面某个 `<div>` 里

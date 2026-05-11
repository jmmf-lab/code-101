# 第 9 节 · 用 fetch 跟后端联调：衣柜上"云"

> 目标：把衣柜从 `localStorage` 改成调老师准备好的接口，体验"和后端联调"。
> **本节完成路线图功能点**：#11 衣柜数据上"云"（API 联调）

---

## 一、前后端分工

| 端 | 负责 |
| --- | --- |
| **前端** | 浏览器里显示页面、跟用户交互、调接口 |
| **后端** | 服务器上跑、存数据库、处理业务逻辑、提供接口 |

前端通过 **API**（接口）跟后端通信。这节你**不写后端**，老师已经准备好了一份"衣柜后端"，你拿到接口文档照着调就行——这就是真实工作里前端"联调"的常态。

## 二、API 与接口文档

接口文档大概长这样：

```
GET    /api/wardrobe          获取所有衣物
POST   /api/wardrobe          添加一件衣物（body: { name, category, price, tags })
PUT    /api/wardrobe/:id      更新一件衣物
DELETE /api/wardrobe/:id      删除一件衣物
```

- `GET` / `POST` / `PUT` / `DELETE` 是 **HTTP 方法**，对应"读 / 增 / 改 / 删"
- `/api/wardrobe` 是接口路径
- `:id` 是参数占位符（实际调用时换成具体 ID）

## 三、JSON：前后端的"通用语"

前后端用 **JSON** 传数据：

```json
{ "name": "白色 T 恤", "category": "上衣", "price": 200 }
```

JSON 长得像 JS 对象，但是个**字符串**。前端用 `JSON.stringify` 和 `JSON.parse` 转换。

## 四、fetch + async/await

`fetch` 是浏览器内置的发起 HTTP 请求的方法：

```js
async function getWardrobe() {
  const response = await fetch("https://xxx.mockapi.io/api/wardrobe");
  const data = await response.json();
  return data;
}

getWardrobe().then((items) => console.log(items));
```

- `async` 函数：可以用 `await` 等异步操作完成
- `fetch(url)` 返回一个**响应对象**，要再调 `response.json()` 才拿到真正数据
- 两个 `await` 都不能少

带 body 的请求（POST/PUT）：

```js
await fetch(url, {
  method: "POST",
  headers: { "Content-Type": "application/json" },
  body: JSON.stringify({ name: "新衣物" }),
});
```

DELETE 请求：

```js
await fetch(`${url}/${id}`, { method: "DELETE" });
```

> 4 种 HTTP 方法本质都是 `fetch`，只是 `method` 参数不同——掌握 GET 后其它就是套模板。

## 五、Network 面板

打开 Chrome DevTools → **Network** 标签。

- 每发一个请求都会在这里出现一行
- 点击能看：请求 URL、方法、状态码、请求 body、响应 body
- 常见状态码：
  - `200` / `201` 成功
  - `400` 请求格式错
  - `404` URL 不存在
  - `500` 后端出错

## 六、常见错误排查

| 错误 | 含义 | 怎么排查 |
| --- | --- | --- |
| `404` | URL 路径错了 | 对照接口文档检查路径 |
| `500` | 后端报错 | 看响应 body 的错误信息，截图发后端（或老师） |
| `CORS` 跨域 | 后端没允许你的域名 | 找老师设置 CORS |
| 没响应 | 网络断了 / URL 域名拼错 | Network 看具体情况 |

排查思路：**先看 Network 面板里那一行的状态码 + 响应 body**，能省 80% 的猜测时间。

## 七、动手：把衣柜搬到云端

### 第 1 步：从老师拿到接口地址，配置到代码

`script.js` 顶部：

```js
const API_BASE = "老师给的地址";   // 比如 https://xxx.mockapi.io/api
let wardrobe = [];
```

### 第 2 步：把 CRUD 全部改成调 API

```js
// 读取（GET）
async function load() {
  const response = await fetch(`${API_BASE}/wardrobe`);
  wardrobe = await response.json();
  render(wardrobe);
  renderOverview();
}

// 添加（POST）
async function addItem(item) {
  const response = await fetch(`${API_BASE}/wardrobe`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(item),
  });
  const newItem = await response.json();
  wardrobe.push(newItem);
  render(wardrobe);
  renderOverview();
}

// 删除（DELETE）
async function deleteItem(id) {
  await fetch(`${API_BASE}/wardrobe/${id}`, { method: "DELETE" });
  wardrobe = wardrobe.filter((item) => item.id !== id);
  render(wardrobe);
  renderOverview();
}

// 编辑（PUT）
async function updateItem(id, changes) {
  await fetch(`${API_BASE}/wardrobe/${id}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(changes),
  });
  load();   // 重新拉一次保证一致
}

load();
```

### 第 3 步：表单提交、删除按钮、编辑保存等改成调上面的函数

之前是 `wardrobe.push(...)` + `save()`，现在改成 `addItem(...)`，等等。
**删除 `localStorage` 相关代码**——云端已经接管存储。

### 第 4 步：打开 Network 面板观察

每次 CRUD 都对着 Network 看：URL、method、status code、response body。

### 第 5 步：故意造错误练排查

- URL 拼错一个字 → 看 `404` 长什么样
- 断开网络 → 看错误对象
- 漏写 `Content-Type` → POST 时后端可能拒绝

## 八、Postman / Apifox

不开浏览器也能调接口的工具。能：
- 直接发请求测试接口
- 看接口文档
- 跟后端对字段

这节简单认识下：[Apifox](https://apifox.com/) / [Postman](https://www.postman.com/)。后续真做项目时再深入。

---

# 📌 课后作业

**完成路线图功能点 #11 衣柜数据上"云"（API 联调）**

## 题目：把衣柜从 localStorage 改成调 API

要求：

1. 拿到老师的接口文档，把 `API_BASE` 配置好
2. 把列表读取、添加、删除、编辑全部走 `fetch` 调接口
3. 删除 `localStorage` 相关代码（云端已经接管了存储）
4. 至少打开一次 Network 面板，截图给我看每个 CRUD 对应的请求
5. 故意造一次错误（比如 URL 拼错），告诉我浏览器报错信息是什么、状态码是几

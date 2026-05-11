# 第 1 节 · 认识 Web，写出你的第一个网页

> 目标：搞懂"网页是什么"，亲手写出一个能在浏览器打开的页面。
> **本节完成路线图功能点**：#1 衣物列表展示（静态版）

---

## 一、网页到底是什么

打开淘宝、知乎、B 站，**右键 → "查看网页源代码"**，你看到的那一堆带尖括号的文字，就是**网页源码**。

**一句话**：网页 = 一份带格式的文本 + 浏览器把它"翻译"成你看到的样子。

## 二、Web 三大基石

| 语言 | 作用 | 类比 |
| --- | --- | --- |
| HTML | 决定**内容和结构** | 房子的**骨架** |
| CSS | 决定**样式**（颜色、字体、布局） | 房子的**装修** |
| JavaScript | 决定**交互**（点击、动画） | 房子的**电器** |

本节只学 HTML。

## 三、准备工具

只需要两个：

1. **Chrome 浏览器**：https://www.google.cn/chrome/
2. **VS Code 编辑器**：https://code.visualstudio.com/

VS Code 装好后熟悉两个操作：
- `File → New File` 新建文件
- `File → Save` 保存（文件名带 `.html` 后缀）

## 四、HTML 长什么样

HTML 由一对一对的**标签**组成：开标签 `<h1>` + 闭标签 `</h1>`，中间夹内容。

新建文件夹 `web-101`，里面新建 `hello.html`，粘贴下面代码并保存：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>My First Page</title>
  </head>
  <body>
    <h1>Hello, Web!</h1>
    <p>This is my first web page.</p>
  </body>
</html>
```

双击文件用 Chrome 打开，看到 "Hello, Web!" 就成了。

**这些标签干嘛用的：**

- `<!DOCTYPE html>` 告诉浏览器"这是 HTML5"
- `<html>` 整个网页的外壳
- `<head>` 配置信息（标题、字符编码，用户看不到）
- `<body>` 用户能看到的内容
- `<h1>` 一级标题
- `<p>` 段落

> 💡 标签必须**成对**：写了 `<h1>` 一定要有 `</h1>`。

## 五、5 个最常用的标签

| 标签 | 作用 |
| --- | --- |
| `<h1>` - `<h6>` | 标题，从大到小 6 级 |
| `<p>` | 段落 |
| `<a href="...">` | 链接 |
| `<img src="..." alt="..." />` | 图片 |
| `<ul>` / `<li>` | 无序列表 |

`href`、`src`、`alt` 这种 `name=value` 写法叫**属性**——给标签提供额外信息。

## 六、动手：再加点内容

把 `hello.html` 的 `<body>` 替换成：

```html
<body>
  <h1>About Me</h1>
  <p>Hi, my name is Tom.</p>

  <h2>My Hobbies</h2>
  <ul>
    <li>Reading</li>
    <li>Running</li>
    <li>Coding</li>
  </ul>

  <a href="https://www.wikipedia.org">Wikipedia</a>

  <img src="https://placekitten.com/300/200" alt="A cute kitten" />
</body>
```

保存后**刷新浏览器**看效果。

---

# 📌 课后作业

**完成路线图功能点 #1 衣物列表展示（静态版）**

## 题目：搭出你的衣柜首页

后面几节课会让这个页面逐步变好看、能交互、能上线。**这节先把骨架搭出来。**

新建文件 `my-wardrobe.html`，至少包含：

1. `<h1>` 你的衣柜名（如 "Tom 的衣柜"）
2. `<p>` 一段简介（总共几件、整体风格）
3. **至少 4 件衣物**，每件含：
   - `<h2>` 衣物名
   - `<img>` 图片
   - `<p>` 描述（颜色 / 品牌 / 价格）
   - `<a>` 一个外链（类似款或品牌官网）
4. `<h2>` "我的风格标签" + `<ul>` 至少 3 个风格关键词

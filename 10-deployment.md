# 第 10 节 · 部署上线：把衣柜发布到公网

> 目标：把衣柜从"自己电脑能打开"变成"互联网上谁都能访问"。
> **本节完成路线图功能点**：#12 公网部署上线

---

## 一、什么是部署

到现在为止，衣柜只在你**自己电脑**的 Chrome 里能打开。
**部署**就是把代码放到一台公网服务器上，让任何人通过一个 URL 就能访问。

最简单的部署方式：把代码推到 GitHub，再让 Vercel 自动构建并托管。**全程免费、几次点击。**

## 二、Git 是什么

Git 是**版本控制工具**：记录代码每一次修改，可以回退、对比、协作。
**GitHub** 是托管 Git 仓库的网站。

**核心三步**：
- `git add <file>` 把改动加入暂存区
- `git commit -m "..."` 把暂存区的内容打成一个"快照"
- `git push` 把本地快照推到 GitHub 上

> 这节不深入讲 Git，只学"够用的"几个命令。

## 三、装 Git + 配置

**装 Git**：
- **Mac**：终端输入 `git --version`，没装会自动提示安装
- **Windows**：下载 [Git for Windows](https://git-scm.com/download/win)

**初次配置**（终端里执行一次即可）：

```bash
git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

## 四、注册 GitHub

打开 [github.com](https://github.com/) 注册账号。

## 五、动手 1：把项目推到 GitHub

### 第 1 步：在 GitHub 上建仓库

点右上角 `+` → `New repository` → 名字 `my-wardrobe` → 选 Public → 创建。

### 第 2 步：本地初始化 Git

打开终端，`cd` 到 `web-101` 文件夹，依次执行：

```bash
git init
git add .
git commit -m "init wardrobe"
```

### 第 3 步：关联远程仓库并推送

GitHub 仓库刚建好的页面会显示几条命令，复制类似下面这几条：

```bash
git remote add origin https://github.com/你的用户名/my-wardrobe.git
git branch -M main
git push -u origin main
```

刷新 GitHub 仓库页面，应该看到你的文件。

## 六、注册 Vercel

打开 [vercel.com](https://vercel.com/)，用 GitHub 账号直接登录。

## 七、动手 2：部署到 Vercel

1. Vercel 主页 → `Add New...` → `Project`
2. 选你的 GitHub 仓库 `my-wardrobe` → `Import`
3. 所有配置保持默认，点 `Deploy`
4. 等 10-30 秒，部署完成后给你一个公网 URL（如 `my-wardrobe-xxx.vercel.app`）

打开这个 URL，你的衣柜就在公网上了。在**手机**上打开同一个 URL，应该也能看到。

## 八、后续更新：改完就 push

以后改了代码，只要：

```bash
git add .
git commit -m "改了什么"
git push
```

Vercel 会**自动**检测到 push、重新构建并部署。

---

# 📌 课后作业

**完成路线图功能点 #12 公网部署上线**

## 题目：把衣柜发布到公网

要求：

1. 注册 GitHub 账号，把 `web-101` 项目推到 GitHub 一个公开仓库
2. 注册 Vercel 账号，用 GitHub 登录
3. 在 Vercel 导入项目，完成首次部署，拿到公网 URL
4. 在**手机**上打开这个 URL，确认衣柜能正常显示
5. 改一处代码（比如标题文字），重新 `git push`，确认 Vercel 自动重新部署

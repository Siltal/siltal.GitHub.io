---
title: Hexo + Github Pages
published: 2022-01-20 
image: /
tags: [Hexo, Github]
category: Hexo
draft: false
---

## Hexo + Github Pages

### 注册 github
>
> 如果你已经有 github 账户你可以跳过这部分

1. 打开 [https://github.com/](https://github.com/)
2. 点击右上角 **Sign up** 按钮

### 设置 NodeJS 环境

#### 下载 NodeJS

你可以在 [NodeJs](https://nodejs.org/en/) 下载 zip 包
随后将其解压到`D:\SDK\node`，并添加环境变量

#### 配置 NodeJS

考虑到硬盘空间，有必要将 **npm package** 移动到其它硬盘.

在终端中运行

```bash
npm config set prefix "D:/SDK/node/npm_global"
npm config set cache "D:/SDK/node/npm_cache"
```

运行以下命令可以检查当前配置

```bash
npm config ls 
```

### 设置 Git 环境

#### 下载 Git

Windows 在 [git 官网](https://git-scm.com/download/win) 下载.

#### 配置 Git

```bash
git config --global user.email "your@email.com"
git config --global user.name "Your Name"
```

### 创建本地 Hexo 文件夹
>
> 假定你的本地Hexo文件夹选择 `D:/Project/VScode/hexo`

#### Hexo 初始化文件夹

```bash
cd D:/Project/VScode/hexo
npm install -g hexo-cli
npx hexo init .
npm install
```

如果一切正常你应该看到如下目录结构

```txt
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

#### 为 Hexo 安装主题

在此处选择一个主题 [hexo-themes](https://hexo.io/themes/)

> 假设你选择了 [A-Obsidian](https://obsidian.tridiamond.tech/)

你可以到 A-Obsidian theme 的 [github repositoriy](https://github.com/bennyxguo/hexo-theme-obsidian) 页面底部
按照指示完成安装，这一般也适用于其它主题

```bash
cd D:/HexoBlog
git clone https://github.com/TriDiamond/hexo-theme-obsidian.git obsidian
```

激活主题后
打开 Hexo config file `_config.yml`, 设置主题到 `obsidian`

```yml
theme: obsidian
```

安装 theme-obsidian 的依赖文件

```bash
cd themes/obsidian
npm install
```

你可以运行以下命令启动本地测试服务器

```bash
npx hexo s
```

### 配置远程服务器

#### Create a blog repository in Github

创建一个新的公开仓库名为 **`<username>.github.io`**

一个样例是 `siltal.github.io`

#### 生成 rsa 密钥对

```bash
ssh-keygen -t rsa -C "your@email.com"
```

复制 `C:\Users\UserName\.ssh\id_rsa.pub` 内的公钥

到仓库的设置中

找到 `Deploy keys`,点击 `Add deploy key`

将公钥粘贴在此处

#### 配置 Hexo 部署

安装 Hexo-deployer-git ，它可以将生成的静态网页根文件夹部署到github pages

```bash
npm install hexo-deployer-git -g
```

在 `_config.yml` 中

```yml
## Deployment
### Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: https://github.com/<username>/<username>.github.io
  branch: main
```

生成静态网页

```bash
npx hexo g
```

最后一步，部署到远程站点

```bash
npx hexo d
```

## Hexo 命令入门

### 创建新帖子

``` bash
hexo new "My New Post"
```

更多信息: [Writing](https://hexo.io/docs/writing.html)

### 运行本地服务器

``` bash
hexo server
```

更多信息: [Server](https://hexo.io/docs/server.html)

### 生成静态文件

``` bash
hexo generate
```

更多信息: [Generating](https://hexo.io/docs/generating.html)

### 部署到远程站点

``` bash
hexo deploy
```

更多信息: [Deployment](https://hexo.io/docs/one-command-deployment.html)

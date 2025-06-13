---
title: Hexo + Github Pages
published: 2022-01-20 
tags: [Hexo, Github]
category: Hexo
draft: false
---
# Hexo + Github Pages

## Sign in Github
> If you already have a Github account, you can skip this part

1. open https://github.com/
2. Click the **Sign up** button in the upper right corner

## Setup NodeJS Environment

### Download NodeJS

You can download on [NodeJs](https://nodejs.org/en/).

### Config NodeJS

Consider disk space, it is necessary to move the **npm package** to another disk.

Run in terminal (after NodeJS installed)

```bash
$ npm config set prefix "D:/SDK/nodejs/npm_global"
$ npm config set cache "D:/SDK/nodejs/npm_cache"
```

You can check the current configuration of npm

```bash
$ npm config ls 
```

## Setup Git Environment

### Download Git 
    
For Windows, download on [this](https://git-scm.com/download/win).

For Linux, it builtin.

### Config Git

```bash
$ git config --global user.email "your@email.com"
$ git config --global user.name "Your Name"
```

## Create local Hexo directory
> Assuming that the local Hexo directory is located in **D:/HexoBlog**

### Hexo init directory

```bash
$ cd D:/HexoBlog
$ npm install -g hexo-cli
$ npx hexo init .
$ npm install
```

If everything is fine, you will see the following structure

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

### Install a theme for Hexo

select one theme on [hexo-themes](https://hexo.io/themes/)

> Assuming that you choose [A-Obsidian](https://obsidian.tridiamond.tech/)

You can go to the [github repositoriy](https://github.com/bennyxguo/hexo-theme-obsidian) of the A-Obsidian theme at the end of the page

in this README.md, you need run following command to install theme
,also applicable to most other theme

```bash
$ cd D:/HexoBlog
$ git clone https://github.com/TriDiamond/hexo-theme-obsidian.git obsidian
```

then activate Theme
Open Hexo config file _config.yml, set theme to obsidian

```yml
theme: obsidian
```

install dependency files of theme-obsidian

```bash
$ cd themes/obsidian
$ npm install
```

now you can run following command to Local Testing.
```bash
$ npx hexo s
```


## Config remote server

### Create a blog repository in Github

create a new public repository named **`<username>.github.io`**

a sample is `Siltal.github.io`

### Generator a rsa key 

```bash 
$ ssh-keygen -t rsa -C "your@email.com"
```

now copy the text of `C:\Users\UserName\.ssh\id_rsa.pub`

go to the Settings of blog repository.

find Deploy keys,click `Add deploy key`

and paste text in here.



### Config Hexo Deployment

Install Hexo-deployer-git

```bash
$ npm install hexo-deployer-git -g
```

In _config.yml

```yml
# Deployment
## Docs: https://hexo.io/docs/one-command-deployment
deploy:
  type: git
  repository: https://github.com/username/username.github.io
  branch: main
```

Generator static files.
```bash
$ npx hexo g
```

Final step,Deploy to remote sites
```bash
$ npx hexo d
```

$$
Done.
$$
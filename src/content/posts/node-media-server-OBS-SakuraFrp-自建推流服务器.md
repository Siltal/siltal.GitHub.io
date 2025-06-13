---
title: node-media-server + OBS + SakuraFrp 自建推流服务器
published: 2022-02-06 
tags: [Streaming, OBS, Port Forwarding, nodejs]
category: Hexo
draft: false
---



## 配置 nodejs 环境

### 下载 nodejs

You can download on [NodeJs](https://nodejs.org/en/).

### 配置 nodejs

移动到另外一个盘以节省磁盘空间。

安装完成后，执行下面的命令来设置它的全局包位置：

```bash
$ npm config set prefix "D:/SDK/nodejs/npm_global"
$ npm config set cache "D:/SDK/nodejs/npm_cache"
```

输入下面的命令检查当前的全局包位置：

```bash
$ npm config ls 
```

## 运行 node-media-server 

假设你的 node-media-server 要安装到 E:\Tools\node-media-server 目录下，那么你可以这样下载：

```bash
cd E:\Tools\node-media-server
npm install node-media-server

```

然后写入下面的配置文件到app.js：
```js
const NodeMediaServer = require('node-media-server');

const config = {
  rtmp: {
    port: 1935,
    chunk_size: 1024,
    gop_cache: false,
    ping: 30,
    ping_timeout: 60
  },
  http: {
    port: 8000,
    allow_origin: '*'
  },
  websocket: {
    port: 8001,
    allow_origin: '*'
  }
};

var nms = new NodeMediaServer(config)
nms.run();

```

### 启动 node-media-server

```bash
cd E:\Tools\node-media-server
node app.js
```

## 使用 OBS 推流

打开OBS的设置，选择推流选项卡，在服务器中输入  `rtmp://localhost:1935/live` ，串流密钥中输入一个直播名称(如index)，然后点击“确定”。
则你的拉流(播放端)地址是 `rtmp://localhost:1935/live/index` ，
推流(推送端)地址也是 `rtmp://localhost:1935/live/index` 。


## 配置 SakuraFrp 端口映射


[Sakura Frp 官网](https://www.natfrp.com/user/)

### 先注册一个账号
途中需要实名认证需要1元

### 新建一个隧道

点击穿透，选择隧道列表

穿透节点先选一个普通的，国内的,

隧道类型选择`TCP`，

本地地址可以是默认的，

端口输入1935 (这是rtmp的端口)，

其它的可以随意。

复制这个隧道的配置文件里的用户token

###  下载 SakuraFrp

在[这里](https://www.natfrp.com/tunnel/download)下载

解压到一个文件夹中，在此文件夹打开终端

执行：

```bash
.\frpc.exe -f XXXXXXXXXXXXXXXXXXXXXXX:XXXXXXXXXX
```

参数是你的token,
成功后，会在终端中看到提示。

## 一些注意事项

Sakura Frp 默认的带宽是10M，这就需要在OBS中调整码率，否则会出现拉流卡顿、模糊、花屏等问题。

可以在 http://127.0.0.1:8000/admin/ 查看当前的链接数量（包括推流和拉流）、带宽等信息。














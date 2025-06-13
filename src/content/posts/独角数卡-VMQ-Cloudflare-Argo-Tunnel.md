---
title: 独角数卡 + VMQ + Cloudflare Argo Tunnel
published: 2023-07-27 
tags: [Faka, Proxy, Docker]
category: Web
draft: false

---

## 独角数卡
https://github.com/assimon/dujiaoka

php 还是太污染环境了，所以使用~~优雅的~~ Docker

- Docker Engine 的安装
https://docs.docker.com/engine/install/
<details>
  <summary>Docker CE 安装</summary>

- 卸载旧版 docker 
    ```
    for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
    sudo rm -rf /var/lib/docker # 删除镜像, 容器, 数据卷, 或自定义配置文件 (非必须)
    sudo rm -rf /var/lib/containerd # 删除镜像, 容器, 数据卷, 或自定义配置文件 (非必须)
    ```
1. 更新apt包索引并安装包以允许apt通过 HTTPS 使用存储库：
    ```
    sudo apt-get update
    sudo apt-get install ca-certificates curl gnupg apt-transport-https lsb-release
    ```
2. 添加Docker官方GPG密钥：
    ```
    sudo install -m 0755 -d /etc/apt/keyrings

    # curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
    # ↑官方 or 国内镜像↓ 二选一
    curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/debian/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

    sudo chmod a+r /etc/apt/keyrings/docker.gpg
    ```
3. 使用以下命令设置存储库：
    ```
    echo \
    "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/debian \
    "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    ```
4. 更新apt包索引：
    ```
    sudo apt-get update       
    ```
5. 安装 Docker 工具集
    ```
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
6. 验证
    ```
    sudo docker run hello-world
    ```
</details>

### 容器配置 & 启动
直接看咕咕的吧，https://blog.laoda.de/archives/docker-compose-install-dujiaoka/ 不过提一点

docker-compose.yml中如果需要redis加密码可以在最后一行redis子级加一句
```
  
  ...
  redis:
    image: redis:alpine
    restart: always
    volumes:
      - ./redis:/data
    command: redis-server --requirepass 6gSgCeoAUNiHBEP3
```
同时env.conf中
```
...
# redis配置
REDIS_HOST=redis
REDIS_PASSWORD=6gSgCeoAUNiHBEP3
REDIS_PORT=6379
...
```
如果后续网站上图片显示不出来可以删掉 env.conf 中 APP_URL 的值
### 邮件配置
以 QQ Stmp 为例

|||
|-|-|
邮件驱动|smtp
Smtp服务器地址|smtp.qq.com
端口|587
账号|siltal@qq.com
密码|xxxxxxxxxxx
协议|TLS
发件地址|siltal@qq.com
发件名称|siltal@qq.com


## VMQ (V免签)
https://github.com/szvone/Vmq

目前版本有bug，https://github.com/szvone/Vmq/issues/36
url拼接错误，需要使用 WebService.java:293行?改成&
可以使用我修复后编译的 https://github.com/Siltal/Vmq/releases/tag/normal

- 按照README.md配置打开后台

|||
|-|-|
后台账号|admin
后台密码|6gSgCeoAUNiHBEP3
订单有效期|5
异步回调|https://主机名/pay/vpay/notify_url
同步回调|https://主机名/pay/vpay/return_url
通讯密钥|74a2aa606ad1f114514e2e9b4c77a1f8
区分方式|金额递增

这里后台密码千万**不要带特殊符号**，否则永远提示未输入密码
- 打开 独角数卡后台 配置-支付配置

配置 V免签支付宝、V免签微信

|||
|-|-|
商户 ID|74a2aa606ad1f114514e2e9b4c77a1f8
商户密钥|https://example.com/
支付场景|通用

*没列出的没必要改*
## Cloudflare Argo Tunnel
Cloudflare Zero Trust / Argo Tunnel 可以用于内网穿透并且不用操心ssl证书的事情

https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/


首先需要你有一个域名，绑定到Cloudflare，将DNS修改为Cloudflare提供的
- john.ns.cloudflare.com
- alina.ns.cloudflare.com


建议通过仪表盘而不使用命令行

https://developers.cloudflare.com/cloudflare-one/connections/connect-networks/install-and-setup/tunnel-guide/remote/

多开的办法：执行网页上的命令后会启动服务，先停止服务，改个名字，再装一个
```shell
cloudflared service install eyJhIjoiMTg3MGQ4Mzhk...Bidh7s9dj111
systemctl stop cloudflared.service
cd /etc/systemd/system/
mv cloudflared.service cloudflared_8090.service # 发卡站使用
rm -rf cloudflared-update.service cloudflared-update.timer 
cloudflared service install eyJhIjoiMTg3MGQ4Mzhk...Bidh7s9dj222
mv cloudflared.service cloudflared_8080.service # 支付站使用
systemctl enable cloudflared_8080.service cloudflared_8090.service --now
```


## 参考
- https://docs.docker.com/engine/install/
- https://yeasy.gitbook.io/docker_practice/install
- https://github.com/assimon/dujiaoka
- https://github.com/szvone/Vmq
- https://blog.laoda.de/archives/docker-compose-install-dujiaoka/
- https://pdai.tech/md/spring/springboot/springboot-x-deploy-war.html
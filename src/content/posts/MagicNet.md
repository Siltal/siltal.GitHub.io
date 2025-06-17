---
title: MagicNet
published: 2022-05-02 
tags: [VPN,Net]
category: Net
draft: false
---

> 请替换下面 `${domain}` 部分为你的域名

```bash
domain = example.com
password = 114514
local_port = 443

```

## 初始配置

```bash
sudo vim /etc/ssh/sshd_config
PermitRootLogin yes
PasswordAuthentication yes
sudo vim /root/.ssh/authorized_keys
sudo passwd user
sudo passwd root

```

## 下载

```bash
# 更新系统
apt update -y && apt upgrade -y
# 安装工具
apt install socat nginx wget unzip -y
# https://github.com/p4gefau1t/trojan-go/releases
# 为 trojan-go 安置文件夹
mkdir -p /opt/trojan-go/
# 下载  trojan-go v0.10.6
wget https://github.com/p4gefau1t/trojan-go/releases/download/v0.10.6/trojan-go-linux-amd64.zip -P /opt/trojan-go/
# 解压
unzip /opt/trojan-go/trojan-go-linux-amd64.zip -d /opt/trojan-go/
```

## 配置 `acme.sh`

```bash
# 安装 acme.sh
curl https://get.acme.sh | sh
# 刷新环境
source ~/.bashrc
# 设置默认CA机构
acme.sh --set-default-ca --server letsencrypt
# 申请证书
acme.sh --issue -d $domain --standalone -k ec-256 --force
# 新建外部使用证书文件夹
mkdir -p /ssl/$domain/
# 发布到外部文件夹
acme.sh --installcert -d $domain --fullchainpath /ssl/$domain/fullchain.crt --keypath /ssl/$domain/privkey.key --ecc --force
```

## 配置 `nginx`

```bash
cat > /etc/nginx/sites-available/$domain << EOF
server {
    listen 80;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;
    server_name ${domain};
    location / {
        try_files \$uri \$uri/ =404;
    }

    if ( \$remote_addr != 127.0.0.1 ) {
        rewrite ^/(.*)$ https://${domain}/$1 redirect;
    } 

    access_log /var/log/nginx/${domain}.access.log;
    error_log /var/log/nginx/${domain}.error.log;
}
EOF

sed -i "s/\${domain}/$domain/g" /etc/nginx/sites-available/$domain
# 启用虚拟主机(一个web服务)
ln -s /etc/nginx/sites-available/$domain /etc/nginx/sites-enabled/

systemctl enable nginx --now
systemctl stop nginx
systemctl start nginx
systemctl status nginx
```

## 配置 `trojan-go`

```bash
cat > /opt/trojan-go/server.yaml << EOF
run-type: server
local-addr: 0.0.0.0
local-port: ${local_port} # trojan服务的端口
remote-addr: 127.0.0.1 
remote-port: 80         # 非法请求重定向
password:
  - ${password}         # 密码
ssl:
  cert: /ssl/${domain}/fullchain.crt
  key: /ssl/${domain}/privkey.key
  sni: ${domain}
router:
  enabled: true
  block:
    - 'geoip:private'
  geoip: /opt/trojan-go/geoip.dat
  geosite: /opt/trojan-go/geosite.dat
mux:
  enabled: true
websocket: 
  enabled: false
EOF

sed -i "s/\${domain}/$domain/g" /opt/trojan-go/server.yaml
sed -i "s/\${local_port}/$local_port/g" /opt/trojan-go/server.yaml
sed -i "s/\${password}/$password/g" /opt/trojan-go/server.yaml
```

## 注册 `trojan-go` 系统服务

```bash
cat > /etc/systemd/system/trojan-go.service << EOF
[Unit]
Description=Trojan-Go - An unidentifiable mechanism that helps you bypass GFW
Documentation=https://p4gefau1t.github.io/trojan-go
After=network.target nss-lookup.target

[Service]
CapabilityBoundingSet=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
AmbientCapabilities=CAP_NET_ADMIN CAP_NET_BIND_SERVICE
NoNewPrivileges=true
ExecStart=/opt/trojan-go/trojan-go -config /opt/trojan-go/server.yaml
Restart=on-failure
RestartSec=10s
LimitNOFILE=infinity

[Install]
WantedBy=multi-user.target
EOF
# 刷新守护进程配置
systemctl daemon-reload

# 启用 trojan-go 服务
systemctl enable trojan-go --now
systemctl stop trojan-go
systemctl start trojan-go
systemctl status trojan-go
```

```bash
trojan://${password}@${domain}:${local_port}#Trojan-Server
# https://github.com/CareyWang/sub-web
https://bianyuan.xyz/
```

---
title: ssh登录时的提示信息
published: 2022-07-11 
tags: [Linux, ssh]
category: Linux
draft: false

---

本文按登录时间顺序 介绍了几种 `ssh` 登录提示信息

## `Banner`

> 显示时机：安全认证（密码或是pubkey）前

`Banner` 由 `sshd_config` 配置
```
vim /etc/ssh/sshd_config
```
其中一行

```bash
Banner none
```

修改为
```bash
Banner /etc/ssh/ssh_banner
```
完成后刷新配置
```bash
systemctl reload sshd
```


定义 `Banner`


```bash
vim /etc/ssh/ssh_banner
```

> 

## `update-motd`

> 显示时机：安全认证后

`update-motd` 由 `update-motd.d` 文件夹配置

```bash
root:~/ # ls -l /etc/update-motd.d
total 8
-rwxr-xr-x 1 root root 19 Jul 11 05:44 00-neofetch
-rwxr-xr-x 1 root root 23 Apr  4  2017 10-uname
```

该文件夹下的脚本会以文件名顺序运行
在自定义 `update-motd` 时需要注意
1. 脚本第一行必须用Shebang定义一个shell脚本解析器
2. 文件需要执行权限，默认为755

例如 `/etc/update-motd.d/10-uname`
```bash
#!/bin/sh
uname -snrvm
```

```
chmod 755 /etc/update-motd.d/*
```
修改完可以 `run-parts /etc/update-motd.d` 查看效果

## `motd`

> 显示时机：update-motd后

`motd` 由 `/etc/motd` 文件配置

该文件是静态定义显示的文本

## `LastLog`

该选项定义是否显示最后登录者的信息
> 显示时机：最后

`LastLog` 由 `/etc/ssh/sshd_config` 配置

将
```
vim /etc/ssh/sshd_config
```
中的
```
#PrintLastLog yes
```
改为

```
PrintLastLog no
```

完成后刷新配置
```bash
systemctl reload sshd
```


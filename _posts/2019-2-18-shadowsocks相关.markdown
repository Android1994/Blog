---
title:  "shadowsocks相关"
author: Subaru Lai
date:   2019-02-18 13:12:13 +0800
layout: post
---

一些有关配置shadowsock的命令。

### shadowsocks服务器端安装
- Centos：
```
  yum install python-setuptools && easy_install pip    
  pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

- Debian或者Ubuntu：
```
  apt-get install python-pip
  pip install git+https://github.com/shadowsocks/shadowsocks.git@master
```

- 查看帮助：
```
ssserver -h 
```

### 配置shadowsocks
- 配置shadowsocks（在/etc目录下新建shadowsocks.json文件，并写入如下内容）：
```
{
  "server": "{your-server}",
  "server_port": "xxxx",
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "password": "{your-password}",
  "timeout": 300,
  "method": "aes-256-gcm"
}
```

### shadowsocks的启动与关闭

- 启动：
```
  ssserver -c /etc/shadowsocks/config.json -d start 
```
- 查看端口状态：
```
  netstat -tunlp
```
-关闭：
```
  ssserver -c /etc/shadowsocks/config.json -d stop
```

### 后台运行

```
  nohup ssserver -c /etc/shadowsocks.json > /dev/null 2>&1 &
```

### 开机启动

使用systemctl管理

- 创建shadowsocks.service文件：
```
  vim /lib/systemd/system/shadowsocks.service
```
- 写入以下内容：
```
[Unit]
Description=Shadowsocks Server
After=network.target
[Service]
ExecStart=/usr/local/bin/ssserver -c /etc/shadowsocks.json
Restart=on-abort
[Install]
WantedBy=multi-user.target
```
- 运行shadowsocks.service：
```
systemctl start shadowsocks.service
```
- 允许开机自动启动：
```
systemctl enable shadowsocks.service
```
- 查看运行状态：
```
systemctl status shadowsocks.service
```

### 开启BBR加速
执行lsmod | grep bbr,看到有tcp_bbr模块即说明bbr已启动。

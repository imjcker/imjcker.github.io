---
layout: post
title: 科学上网
category: 科学上网
tags: ss
---
科学上网之安装shadowsocks到centos7。

```shell
# 下载 pip
curl "https://bootstrap.pypa.io/get-pip.py" -o "get-pip.py"
# 安装 pip
python get-pip.py
# 安装shadowsocks
pip install shadowsocks
# 启动shadowsocks
ssserver -p 8388 -k imjcker.com -m aes-256-cfb --user nobody -d start
# 编写配置文件
docker run -d --name ssserver -p 13579:8388 -p 13579:8388/udp --restart always -e PASSWORD=ss.imjcker.com -e METHOD=chacha20 shadowsocks/shadowsocks-libev:latest
```

```json
# 多用户配置
{
    "server": "0.0.0.0",
    "port_password": {
		"10001":"imjcker.com1",
    "10002":"imjcker.com2",
		"10003":"imjcker.com3",
		"10004":"imjcker.com4"
    },
    "timeout": 300,
    "method": "aes-256-cfb"
}
# sslocal
{
    "server":"46.29.165.233",
    "server_port":6666,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"imjcker.com",
    "timeout":600,
    "method":"aes-256-cfb",
    "fast_open": false,
    "workers": 10
}
```


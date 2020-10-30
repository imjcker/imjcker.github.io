---
layout: post
title: 科学上网
category: 科学上网
tags: ss shadowsocks
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
ssserver -p 13579 -k ss.imjcker.com -m chacha20 --user nobody -d start

```

## docker 快速安装

```shell
docker run -d --name ssserver -p 13579:8388 -p 13579:8388/udp --restart always -e PASSWORD=ss.imjcker.com -e METHOD=chacha20 shadowsocks/shadowsocks-libev:latest
```




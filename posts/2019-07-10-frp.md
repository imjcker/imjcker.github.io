---
layout: post
title: frp All in One
category: 网络
tags: frp
---

# 2019-07-10-frp

frp是一个暴力内网到互联网的反向代理开源软，目前托管在同性交友网站 \[GitHub\] 上，如果你也有这样的需求，我推荐你使用这个好玩的软件。

## frps.service

1. 编辑文件

   ```text
     vi /etc/systemd/system/frps.service
   ```

2. 将下列文件复制到frps.service中保存

   ```text
    [Unit]
    Description=FRP Server Daemon

    [Service]
    Type=simple
    ExecStartPre=-/usr/sbin/setcap cap_net_bind_service=+ep /usr/bin/frps
    ExecStart=/usr/bin/frps -c /etc/frp/frps.ini
    Restart=always
    RestartSec=20s
    User=nobody
    PermissionsStartOnly=true

    [Install]
    WantedBy=multi-user.target
   ```

## frpc.service

1. 编辑文件

   ```text
    vi /etc/systemd/system/frpc.service
   ```

2. 将下列文件复制到frpc.service中保存

   ```text
    [Unit]
    Description=FRP Client Daemon
    After=network.target
    Wants=network.target

    [Service]
    Type=simple
    ExecStartPre=-/usr/sbin/setcap cap_net_bind_service=+ep /usr/bin/frpc
    ExecStart=/usr/bin/frpc -c /etc/frp/frpc.ini
    Restart=always
    RestartSec=20s
    User=nobody
    PermissionsStartOnly=true

    [Install]
    WantedBy=multi-user.target
   ```

3. 配置
4. 启动frps.service

   ```text
    sudo systemctl start frps
   ```

5. 开启开机启动

   ```text
    sudo systemctl enable frps
   ```

6. 开启frpc.service, 同1、2步骤，将`frps`改为`frpc`即可。
7. 检查状态

   ```text
    sudo systemctl status frpc
   ```


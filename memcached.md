---
layout: post
title: memcached
category: NoDB
tags: memcached
---

# 2018-03-19-memcached

学习使用memcached，本人使用的是MacBook，服务器使用CentoS，因此会有2个版本的安装使用。本人是初学者，有不对的地方还请看到博客的前辈不吝赐教，在此先感谢各位前辈。

**System**：OS X

工具：homebrew

1. 执行脚本：

> brew install memcached

1. 启动命令：

> memcached -m 64 -p 11211 -u nobody -d start

1. 常用参数：

> -p 指定端口号（默认11211）  
> -t 线程数（默认4）  
> -l 连接的IP地址, 默认是本机  
> -d start 启动memcached服务  
> -d restart 重起memcached服务  
> -d stop\|shutdown 关闭正在运行的memcached服务  
> -m 最大内存使用，单位MB。默认64MB  
> -M 内存耗尽时返回错误，而不是删除项  
> -c 最大同时连接数，默认是1024 -f 块大小增长因子，默认是1.25 -n 最小分配空间，key+value+flags默认是48


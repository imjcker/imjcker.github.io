---
layout: post
title: VirtualBox
category: vm
tags: vm
---
在VirtualBox中安装和克隆CentOS

***前言***

​	最近开始学习ActiveMQ+ZooKeeper的集群，需要建立至少3个服务器，考虑到本人只有2个VPS，遂选择在自己的Mac上安装VirtualBox搭建虚拟机。以下是搭建环境中遇到的一些问题和总结。

1. 快速搭建3个虚拟机

   事实上在安装好一个虚拟机后，便可通过**克隆**的形式快速创建多个虚拟机。

2. 网络问题

3. 原始虚拟机网络

   考虑资源问题，下载的是CentOS Minimal版本的镜像文件。安装好后马上测试了一下联网情况  

```
ping jcker.org
```

发现无法ping通  

***解决方案***

```
vi /etc/sysconfig/network-scripts/ifcfg-xxx
#xxx视具体情况而定，可能是eth0什么的。
```

将**ONBOOT**参数的值改为__yes__ ，然后重启网络服务：

```
service network restart

```

再次尝试ping外网，就能够ping通了。

4. 克隆虚拟机网络

   克隆出来的虚拟机无法联网，这是因为mac地址虽然在克隆时选择了全部重新加载，但是克隆的虚拟机系统里面的文件是没有替换的，故而需要手动取更改。

```shell
vi /etc/sysconfig/network-scripts/ifcfg-xxx
#更改mac地址为实际的虚拟机mac地址，更改后重启网络服务
service nerwork restart

```

如果还是没有ping通，则需要更改网卡配置的mac地址

```shell
vi /etc/udev/rules.d/70-persistent-net.rules

```

改好后重启网络服务，保险起见，重启系统最好。



---
layout: post
title: zookeeper集群
category: MQ
tags: zookeeper
---
zookeeper 集群搭建记录，你懂的！！！

## docker 安装

### 单节点安装

```shell
docker run -d --name zookeeper-server-imjcker -p 2181:2181 -p 2888:2888 -p 3888:3888 -d --restart always zookeeper
# 查看日志
docker logs zookeeper-server-imjcker
# 进入容器
docker exec -it zookeeper-server-imjcker /bin/bash
```

### 集群配置

```yaml
version: '3.1'

services:
  zoo1:
    image: zookeeper
    restart: always
    hostname: zoo1
    ports:
      - 2181:2181
    environment:
      ZOO_MY_ID: 1
      ZOO_SERVERS: server.1=0.0.0.0:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo2:
    image: zookeeper
    restart: always
    hostname: zoo2
    ports:
      - 2182:2181
    environment:
      ZOO_MY_ID: 2
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=0.0.0.0:2888:3888;2181 server.3=zoo3:2888:3888;2181

  zoo3:
    image: zookeeper
    restart: always
    hostname: zoo3
    ports:
      - 2183:2181
    environment:
      ZOO_MY_ID: 3
      ZOO_SERVERS: server.1=zoo1:2888:3888;2181 server.2=zoo2:2888:3888;2181 server.3=0.0.0.0:2888:3888;2181
```



## 手动安装部署

环境，Java安装

准备目录

```shell
mkdir /data/zookeeper/zkdata -p
mkdir /data/zookeeper/zkdatalog
```

```shell
# 下载安装包
wget https://downloads.apache.org/zookeeper/zookeeper-3.6.0/apache-zookeeper-3.6.0-bin.tar.gz
# 解压到指定路径
tar zxf apache-zookeeper-3.6.0-bin.tar.gz -C /usr/local
# 修改配置文件
cd /usr/local/apache-zookeeper-3.6.0-bin/conf
# 拷贝配置文件
cp zoo_sample.cfg zoo.cfg
```



编写配置文件内容

```shell
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/zookeeper/zkdata
dataLogDir=/data/zookeeper/zkdatalog
clientPort=2181
server.1=140.82.49.26:2888:3888
server.2=149.28.207.189:2888:3888
server.3=144.202.97.115:2888:3888
```

配置myid文件

```shell
#server1（172.32.15.114）
echo "1" > /data/zookeeper/zkdata/myid
#server2（172.32.15.112）
echo "2" > /data/zookeeper/zkdata/myid
#server3（172.32.15.113）
echo "3" > /data/zookeeper/zkdata/myid
```

配置环境变量

```shell
vi /etc/profile

export ZOOKEEPER_HOME=/usr/local/apache-zookeeper-3.6.0-bin
export PATH=$PATH:$ZOOKEEPER_HOME/bin

source /etc/profile
```



```shell
# 启动
/usr/local/apache-zookeeper-3.6.0-bin/bin/zkServer.sh start
# 查看
/usr/local/apache-zookeeper-3.6.0-bin/bin/zkServer.sh status
```





## 其它配置

启动失败原因：

1. 可能是虚拟机内存分配太少了
2. 检查配置
3. 如果启动成功，但是集群状态失败，请关闭防火墙后再检查状态

***为什么要关闭防火墙？*** 不用开启连接端口和集群通信端口即可，不必关闭防火墙。

不知道，只知道在配置zookeeper集群是启动失败，根据报错到Google查询得知要关闭防火墙。

```shell
firewall-cmd --add-port=2181/tcp --zone=public --permanent
firewall-cmd --add-port=2888/tcp --zone=public --permanent
firewall-cmd --add-port=3888/tcp --zone=public --permanent
firewall-cmd --add-port=12181/tcp --zone=public --permanent
```



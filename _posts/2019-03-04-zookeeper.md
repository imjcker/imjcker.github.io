---
layout: post
title: zookeeper集群
category: MQ
tags: zookeeper
---
1. 准备目录

```shell
mkdir /data/zookeeper/zkdata -p
mkdir /data/zookeeper/zkdatalog
```

2. 下载安装包

```shell
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.14/zookeeper-3.4.14.tar.gz
```

3. 解压到指定路径

```shell
tar zxf zookeeper-3.4.14.tar.gz -C /usr/local
```

4. 修改配置文件

```shell
cd /usr/local/zookeeper/conf
```

拷贝配置文件

```shell
cp zoo_sample.cfg zoo.cfg
```

编写配置文件内容

```shell
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/zookeeper/zkdata
dataLogDir=/data/zookeeper/zkdatalog
clientPort=12181
server.1=172.32.15.114:12888:13888
server.2=172.32.15.112:12888:13888
server.3=172.32.15.113:12888:13888
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

5. 配置环境变量

```shell
vi /etc/profile

export ZOOKEEPER_HOME=/usr/local/zookeeper
export PATH=$PATH:$ZOOKEEPER_HOME/bin

source /etc/profile
```

6. 启动

```shell
/usr/local/zookeeper/bin/zkServer.sh start

```

7. 查看

```shell
/usr/local/zookeeper/bin/zkServer.sh status

```

8. 错误

启动失败原因：

1. 可能是虚拟机内存分配太少了
2. 检查配置





# ZooKeeper 安装

**下载安装**

```shell
wget http://mirrors.shuosc.org/apache/zookeeper/zookeeper-3.4.9/zookeeper-3.4.9.tar.gz
tar -zxvf zookeeper-3.4.9.tar.gz
```

**配置**

**关闭防火墙**

***为什么要关闭防火墙？*** 不用开启连接端口和集群通信端口即可，不必关闭防火墙。

不知道，只知道在配置zookeeper集群是启动失败，根据报错到Google查询得知要关闭防火墙。

```shell
service iptables stop
chkconfig iptables --list
chkconfig iptables off


```

**zookeeper**集群配置

```shell
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/tmp/zookeeper
clientPort=2181
server.1=192.168.0.104:2888:3888
server.2=192.168.0.105:2888:3888
server.3=192.168.0.106:2888:3888


```

每个节点的配置保持一致。

***节点特殊配置***

在zoo.cfg配置文件的**dataDir**指定的目录下建立一个名为**myid**的文件，到该目录执行下面命令即可。

```shell
echo id > myid #id指的是配置文件中的server.1这个‘1’

```

每个节点都要配置，且id要于该节点保持一致。

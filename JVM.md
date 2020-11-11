---
layout: post
title: sys spring boot starter  
category: spring-cloud
tags: spring
---

UI动态展示系统参数, 基于spring boot，bootstrap4  
run as a stand alone app or a dependency of any spring boot based application.  

![界面截图](https://img-blog.csdnimg.cn/2020010921574533.gif)

## run with docker
run within docker by execute script below.
explanation: run service container as **monitor** and publish port on host server port **8081** or any other available ports you replace with. 
```shell script
sudo docker run -d --name monitor -p 8081:8080 imjcker/sys-spring-boot-starter:latest
```

## Clone on GitHub
```shell
git clone https://github.com/imjcker/sys-spring-boot-starter
```
## 工具推荐
如果你需要一款全面的服务器监视系统，推荐你使用[netdata](https://netdata.cloud)



# 2020-01-12-jvm

Java虚拟机学习和优化。

## 工具

### 安装jps工具

```text
yum list | grep jdk-devel

yum -y install xxx
```

## Java 虚拟机

### jstatd 使用

1. 创建policy文件 

```text
grant codebase "file:${java.home}/../lib/tools.jar" {   
    permission java.security.AllPermission;
};
```

1. 启动

```text
jstatd -J-Djava.security.policy=jstatd.all.policy

jstatd -J-Djava.security.policy=jstatd.all.policy -J-Djava.rmi.server.hostname=192.168.134.128 -p 1099 -J-Djava.rmi.server.logCalls=true
```

1. 本地启动jvisualvm
2. 配置jstatd

### 配置jvm内存

```text
@echo off

start "baseservice" java -jar -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=128m -Xms256m -Xmx256m -Xmn256m -Xss256k -XX:SurvivorRatio=8 -XX:+UseConcMarkSweepGC baseservice.jar
```

#### 配置说明

URL：[https://www.javatt.com/p/51429](https://www.javatt.com/p/51429)

```text
-Xmn1000m -XX:SurvivorRatio=8
```

Eden区占用80%，To Survivor和From Survivor各占剩余一半即各占10%

[https://toutiao.io/posts/132354/app\_preview](https://toutiao.io/posts/132354/app_preview)

[https://mp.weixin.qq.com/s/fFAv6vgoLszEACO6dmRptA](https://mp.weixin.qq.com/s/fFAv6vgoLszEACO6dmRptA)


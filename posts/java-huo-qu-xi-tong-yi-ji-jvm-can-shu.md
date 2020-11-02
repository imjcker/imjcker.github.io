---
layout: post
title: sys spring boot starter
category: spring-cloud
tags: spring
---

# 2016-06-02-sys-spring-boot-starter

UI动态展示系统参数, 基于spring boot，bootstrap4  
run as a stand alone app or a dependency of any spring boot based application.

![&#x754C;&#x9762;&#x622A;&#x56FE;](https://img-blog.csdnimg.cn/2020010921574533.gif)

## run with docker

run within docker by execute script below. explanation: run service container as **monitor** and publish port on host server port **8081** or any other available ports you replace with. \`\`\`shell script sudo docker run -d --name monitor -p 8081:8080 imjcker/sys-spring-boot-starter:latest

```text
## Clone on GitHub
```shell
git clone https://github.com/imjcker/sys-spring-boot-starter
```

## 工具推荐

如果你需要一款全面的服务器监视系统，推荐你使用[netdata](https://netdata.cloud)

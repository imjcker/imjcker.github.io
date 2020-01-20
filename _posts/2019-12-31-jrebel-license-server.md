---
layout: post  
title: Jrebel激活服务  
category: 工具
tags:
  - jrebel
---

This application still working today, but for then reason of copyright, I was not allow to publish this post, this is a way to work around if possible. LMAO...😂

**enjoy::)** 

http://jrebel.imjcker.com/uuid
replace uuid with a real one，[generate UUID online](https://1024tools.com/uuid)

this URL may not work, cause it is running on a private PC, :/ 😂，if possible, please set your own license server.

## Run in docker container
run license server in docker container, this may be the easiest way to deploy it. 
```shell
docker run -d --name jrebel -p 9090:9090 --restart always imjcker/jrebel:latest
```
## Run as a spring boot application
if you do not use docker, there is another way to deploy. clone the repository then run it as a stand alone spring boot application. 

1. clone 

	```shell
	git clone https://github.com/imjcker/jrebel-license-server
	```
2. build
	```shell
	mvn clean package
	```

3. run
	```shell
	java -jar jrebel-1.0-SNAPSHOT-jar-with-dependencies.jar
	```

## last 
如果你觉得有用，可以给这个项目[jrebel-license-server](https://github.com/imjcker/jrebel-license-server)打个小星星哟😊

## last but not least
我开了个公众号，没怎么维护，😂
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200110090308294.jpg)
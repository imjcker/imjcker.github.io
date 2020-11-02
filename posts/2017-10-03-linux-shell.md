---
layout: post
title: shell 脚本使用
category: Linux
tags: shell
bg: 2020/shell.png
---

# 2017-10-03-linux-shell

shell脚本能解放生产力，记录shell脚本使用情况

## shell 编程

### ssh 密钥对使用

在开发和测试的时有很多场景都要用到ssh连接远程服务器，每次都输入密码让人真的很头痛，那么有说明办法可以避免每次都输入密码呢？答案就是：ssh密钥对

```text
# 生成密钥对
ssh-keygen -t rsa -b 4096 -C "this is a ssh key demo" # -t指加密方式，-b指加密长度
# 一直回车默认直到生成密钥对在默认位置，一般为当前用户的根目录下的.ssh目录下
cd ~/.ssh
# 私钥：id_rsa ， 公钥：id_rsa.pub
# 配置私钥服务
# 检查私钥服务状况
eval "$(ssh-agent -s)"
# 打印pid说明正常，开始配置私钥
ssh-add ~/.ssh/id_rsa
# 查看添加状况
ssh-add -l
```

### 脚本入参

```text
$# 入参个数
$1 第一个入参，以此类推
```

### telnet

telnet: Unable to connect to remote host: No route to host

执行：

```text
iptables -F
```

### xsync

```text
#!/bin/bash

pcount=$#
if((pcount==0)); then
echo no args;
exit;
fi

p1=$1
fname=`basename $p1`
echo fname=$fname

pdir=`cd -P $(dirname $p1); pwd`
echo pdir=$pdir

user=`whoami`

for host in hadoop-m hadoop-s1 hadoop-s2 hadoop-s3
do
        echo -------------------hadoop$host --------------
        rsync -rvl $pdir/$fname $user@$host:$pdir
done
```


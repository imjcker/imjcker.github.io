---
layout: post
title: NginX All in One
category: 网关
tags: nginx
---

# 2019-05-06-nginx

个人对使用nginx的一些浅显记录。

```text
# 下载
wget http://nginx.org/download/nginx-1.16.0.tar
# 源码安装
tar zxf nginx-1.16.0.tar.gz
cd nginx-1.16.0
./configure
make
sudo make install
# yum 安装
sudo rpm -ivh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
# 或者
sudo yum -y install nginx
# 启动
./sbin/nginx -c ./conf/nginx.conf
# 重新加载配置
./sbin/nginx -s reload
# 停止
./sbin/nginx -s stop
```

**nginx** on CentOS

```text
sudo rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
sudo yum install -y nginx
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
# default config location: /etc/nginx/conf.d/
nginx -c nginx.conf
nginx -s relaod/stop/reopen
```

**nginx** on Windwos

1. download zip file   
2. unzip  
3. double click nginx.exe or open cmd in the directory the type: **start nginx** then hit enter
4. other commands on windows  

```text
  ./nginx -s open
  ./nginx -s reload
  ./nginx -s stop
```

```javascript
server{
  listen 8080;
  server_name imjkcer_web;
  location / {
      root    /usr/local/dist;
      index    index.html;
      try_files $uri /index.html;
    }
  location /aaa/ {
      proxy_set_header Host $host;
    proxy_pass http://1.2.3.4:8080/aaa/;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
    }
}
```


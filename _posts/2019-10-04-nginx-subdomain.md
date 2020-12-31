---
layout : post
title : "Nginx配置二级域名"
category : "工具"
tags : [Nginx,域名]
---

## 需求描述

在一个项目中，需要将多个项目部署在一台服务器上，使用不同的域名访问。

例如有三个项目A、B、C。对应域名`a.vvkee.com`、`b.vvkee.com`、`c.vvkee.com`，其中A使用端口8081、B使用端口8082、C使用端口8083。

那么在Nginx上如何部署呢？

## 实现

编辑Nginx的配置文件`nginx.conf`，进行如下配置：

```
server {
    listen          80;
    server_name     a.vvkee.com;

    location / {
        proxy_pass   http://localhost:8081;
        proxy_set_header  Host  $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}


server {
    listen          80;
    server_name     b.vvkee.com;

    location / {
        proxy_pass   http://localhost:8082;
        proxy_set_header  Host  $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}

server {
    listen          80;
    server_name     c.vvkee.com;

    location / {
        proxy_pass   http://localhost:8083;
        proxy_set_header  Host  $host;
        proxy_set_header  X-Real-IP  $remote_addr;
        proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```

重新加载Nginx的配置文件。

```
nginx -t
nginx -s reload
```

## 参考

<https://blog.csdn.net/a_helloword/article/details/84316291>
---
title: Nginx配置https
date: 2018-01-16 15:47:06
categories: Nginx
tags:
  - Nginx
  - https
---

Nginx配置https

<!-- more -->

1. 把证书放到 Nginx 安装目录下的 cert(自己创建) 中

2. 配置 http 请求重定向
```nginx
server {
    listen       80;
    server_name  localhost;
    rewrite ^ https://$http_host$request_uri? permanent;
}

```

3. https 配置
以下属性中ssl开头的属性与证书配置有直接关系，其它属性请结合自己的实际情况复制或调整
```nginx
server {
    listen 443;
    server_name localhost;
    ssl on;
    root html;
    index index.html index.htm;
    ssl_certificate   cert/214447565700670.pem;
    ssl_certificate_key  cert/214447565700670.key;
    ssl_session_timeout 5m;
    ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    location / {
        root html;
        index index.html index.htm;
    }
}
```
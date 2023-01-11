---
title: nginx配置https
date: 2023-01-11 12:04:52
tags:
---



## ****SSL 证书部署****

首先申请免费证书,推荐阿里或者七牛云[https://cn.aliyun.com/product/cas?from_alibabacloud=&source=5176.11533457&userCode=ywqc0ubl&type=copy](https://cn.aliyun.com/product/cas?from_alibabacloud=&source=5176.11533457&userCode=ywqc0ubl&type=copy)

下载申请好的 ssl 证书文件压缩包到本地并解压到/etc/nginx/

```jsx
/etc/nginx/certificate.crt;
/etc/nginx/private.key;
```

将这两个文件上传至服务器的/etc/nginx/目录里

```jsx
scp /Users/lilin/Downloads/certificate.crt root@xxx.xx.xxx.xx:/etc/nginx/
scp /Users/lilin/Downloads/private.crt root@xxx.xx.xxx.xx:/etc/nginx/
```

## nginx.conf配置

配置 https [server](https://link.segmentfault.com/?enc=j1N%2B5oPWtgTmj4YYsFd5ww%3D%3D.x32KBqxEXmGOwmMZfGjqRXhbghKgM1LsJhVbG%2B8iji5BQ1P9tGkbxC2u%2FdahY%2BV6).注释掉之前的 http server 配置,新增 https server

```jsx
将http重定向https
server {
        listen 80;
        server_name ptg.life www.ptg.life noval.ptg.life prompt.ptg.life naifu.ptg.life;
        return 301 https://$host$request_uri;
 }

server {
        listen 443 ssl;
        server_name ptg.life www.ptg.life noval.ptg.life prompt.ptg.life naifu.ptg.life;

        # 新版的nginx只用listen 443 ssl就行,需要注释
        # ssl on;
        keepalive_timeout 10m;
        server_tokens off;
        # 缓存SSL握手产生的参数和加密密钥的时长
        ssl_session_timeout 10m;
        # 证书
        ssl_certificate /etc/nginx/certificate.crt;
        ssl_certificate_key /etc/nginx/private.key;
        # 日志
        access_log /var/log/nginx/nginx.vhost.access.log;
        error_log /var/log/nginx/nginx.vhost.error.log;

        # 根用iframe嵌入做个隐式url
        location / {
            index index.html index.htm index.html inde.php;
            root /usr/share/nginx/html;
        }
    }
```

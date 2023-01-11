---
title: nginx配置
date: 2023-01-11 12:05:22
tags:
---


### nginx配置

```js
user www-data;
#启动进程,通常设置成和cpu的数量相等
worker_processes 2;
pid /run/nginx.pid;
include /etc/nginx/modules-enabled/*.conf;

events {
    worker_connections 768;
    # multi_accept on;
}

http {
    ##
    # Basic Settings
    ##

    sendfile on;
    tcp_nopush on;
    types_hash_max_size 2048;
    # server_tokens off;

    # server_names_hash_bucket_size 64;
    # server_name_in_redirect off;

    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    ##
    # SSL Settings
    ##

    ssl_protocols TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # Dropping SSLv3, ref: POODLE
    ssl_prefer_server_ciphers on;

    ##
    # Logging Settings
    ##

    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    # Gzip Settings
    gzip on;
    gzip_min_length 1000;
    gzip_vary on;
    gzip_proxied any;
    gzip_comp_level 6;
    gzip_buffers 16 8k;
    gzip_http_version 1.1;
    gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

    ##
    # Virtual Host Configs
    ##
    include /etc/nginx/conf.d/*.conf;
    include /etc/nginx/sites-enabled/*;


    map $sent_http_content_type $expires {
        "text/html" epoch;
        "text/html; charset=utf-8" epoch;
        default off;
    }

    server {
        listen 80;
        server_name ptg.life www.ptg.life noval.ptg.life prompt.ptg.life;
        return 301 https://$host$request_uri;
        # gzip on;
        # gzip_types text/plain application/xml text/css application/javascript;
        # gzip_min_length 1000;

        # location / {
        #     expires $expires;
        #     proxy_redirect off;
        #     proxy_set_header Host $host;
        #     proxy_set_header X-Real-IP $remote_addr;
        #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #     proxy_set_header X-Forwarded-Proto $scheme;
        #     proxy_read_timeout 1m;
        #     proxy_connect_timeout 1m;
        #     proxy_pass http://127.0.0.1:3000/;
        # }
        # location ~ /.well-known {
        #     allow all;
        # }
        # location ^~ /.well-known/pki-validation/ {
        #     add_header Cache-Control no-cache;
        #     default_type "text/plain";
        #     rewrite /.well-known/pki-validation/(.*) /$1 break;
        #     root /var/www/whatever;
        # }
        # location /nuxt3-tag {
        #     expires $expires;
        #     proxy_redirect off;
        #     proxy_set_header Host $host;
        #     proxy_set_header X-Real-IP $remote_addr;
        #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #     proxy_set_header X-Forwarded-Proto $scheme;
        #     proxy_read_timeout 1m;
        #     proxy_connect_timeout 1m;
        #     proxy_pass http://127.0.0.1:3000/nuxt3-tag;
        # }
        # location /stable {
        #     rewrite ^/(.*) http://www.ptg.life:3000/nuxt3-tag permanent;
        # }
        # location /nuxt3 {
        #     rewrite ^/(.*) http://www.ptg.life:3000/nuxt3-tag permanent;
        # }
        # location /tag {
        #     rewrite ^/(.*) http://www.ptg.life:3000/nuxt3-tag permanent;
        # }
        # location /stable/api {
        #     proxy_pass http://www.ptg.life:5000/api;
        #     proxy_set_header Access-Control-Max-Age 86400;
        #     proxy_set_header Host $host;
        #     proxy_redirect off;
        #     proxy_set_header X-Real-IP $remote_addr;
        #     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        #     proxy_connect_timeout 60;
        #     proxy_read_timeout 60;
        #     proxy_send_timeout 60;
        # }
        # location /static/ {
        #     valid_referers none blocked *.ptg.life;
        #     if ($invalid_referer) {
        #         return 403;
        #         break;
        #     }
        #     expires 30d;
        #     autoindex on;
        #     root /data;
        # }
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

        location ~ /.well-known {
            allow all;
        }

        location ^~ /.well-known/pki-validation/ {
            add_header Cache-Control no-cache;
            default_type "text/plain";
            rewrite /.well-known/pki-validation/(.*) /$1 break;
            root /var/www/whatever;
        }

        location /nuxt3-tag/ {
            expires $expires;
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_read_timeout 1m;
            proxy_connect_timeout 1m;
            proxy_pass http://127.0.0.1:3000/nuxt3-tag/;
        }

        location /naifu/ {
            expires $expires;
            proxy_redirect off;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
            proxy_read_timeout 1m;
            proxy_connect_timeout 1m;
            proxy_pass http://127.0.0.1:3000/naifu/;
        }

        location /nuxt3 {
            rewrite ^/(.*) $host://$http_host/nuxt3-tag permanent;
        }

        location /tag {
            rewrite ^/(.*) $host://$http_host/nuxt3-tag permanent;
        }

        # flask的接口代理
        location /stable/api {
            proxy_pass http://127.0.0.1:5000/api;
            proxy_set_header Access-Control-Max-Age 86400;
            proxy_set_header Host $host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_connect_timeout 60;
            proxy_read_timeout 60;
            proxy_send_timeout 60;
        }

        location /static/ {
            valid_referers none blocked *.ptg.life;
            if ($invalid_referer) {
                return 403;
                break;
            }
            expires 30d;
            autoindex on;
            root /data;
        }
    }
}
```

---
title: 'ubuntu18安装nginx'
date: 2018-05-09 17:39:55
tags: nginx
---

### **apt安装**sudo apt update

```jsx
sudo apt install nginx

nginx -V

默认安装路径 /etc/nginx/nginx.conf
```

### **nginx安装位置**

```jsx
**whereis nginx**
```

### **启动**

```jsx
service nginx start
```

### **检查nginx配置文件**

```jsx
service nginx reload
```

### **重启**

```jsx
nginx -s reopen
```

### **停止**

```jsx
nginx -s stop
```

### 端口占用

```jsx
查看端口
netstat -lntp

nginx端口
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      248057/nginx: master

结束端口
kill 248057

重启nginx
service nginx restart

```

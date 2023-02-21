---
title: Mysql的processlist中处理表连接
date: 2023-01-16 16:22:53
tags: Mysql
---


## processlist中大量sleep

在生产环境切换mysql数据库，切换后数据库连接池爆满，抛出异常“too many connections”；

出现这种现象，比较常见的原因是连接数真不够了，需要设置连接数：

### 连接数不够解决

```js
mysql -u root -p;
show full processlist;
kill id;
```

### 查看最大连接数

```js
show variables like "max_connections";
```

查看最大连接数，应该是与上面查询到的连接数相同，才会出现too many connections的情况

```js
set GLOBAL max_connections=1000;
```

修改最大连接数，但是这不是一劳永逸的方法，应该要让它自动杀死那些sleep的进程。

### 自动杀死那些sleep的进程

```js
show global variables like 'wait_timeout';
```

这个数值指的是mysql在关闭一个非交互的连接之前要等待的秒数，默认是28800s

```js
set global wait_timeout=300; 
```

修改这个数值，这里可以随意，最好控制在几分钟内

```js
set global interactive_timeout=500; 
```

修改这个数值，表示mysql在关闭一个连接之前要等待的秒数，至此可以让mysql自动关闭那些没用的连接，但要注意的是，正在使用的连接到了时间也会被关闭，因此这个时间值要合适

### 笨方法

```js
select concat('KILL ',id,';') from information_schema.processlist where user='root';
```

先把要kill的连接id都查询出来,然后一个个kill

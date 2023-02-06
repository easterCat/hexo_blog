---
title: Npm和Package.json
date: 2023-02-06 17:21:24
tags: npm
---

# npm 和 package.json

npm 是一个包管理器，它让 JavaScript 开发者分享、复用代码更方便。一个网站里通常有几十甚至上百个 package，分散在各处，通常会将这些包按照各自的功能进行划分（类似我们安卓开发中的划分子模块），但是如果重复造一些轮子，不如上传到一个公共平台，让更多的人一起使用、参与这个特定功能的模块。

管理本地安装 npm 包的最好方式就是创建 package.json 文件。

一个 package.json 文件可以有以下几点作用：

-   作为一个描述文件，描述了你的项目依赖哪些包
-   允许我们使用 “语义化版本规则”（后面介绍）指明你项目依赖包的版本
-   让你的构建更好地与其他开发者分享，便于重复使用

## npm 安装

npm 是依附于 node.js 的，我们可以去它的官网 <https://nodejs.org/en/download/> 下载安装 node.js。

```js
node - v;
npm - v;
```

## npm 更新

```js
npm install npm@latest -g
```

## npm 创建 package.json 文件

```js
npm init --yes // 跳过问题
```

package.json

-   name 全部小写，没有空格，可以使用下划线或者横线
-   version x.x.x 的格式符合“语义化版本规则”
-   description：描述信息，有助于搜索
-   main: 入口文件，一般都是 index.js
-   scripts：支持的脚本，默认是一个空的 test
-   keywords：关键字，有助于在人们使用 npm search 搜索时发现你的项目
-   author：作者信息
-   license：默认是 MIT
-   bugs：当前项目的一些错误信息，如果有的话
-   dependencies：在生产环境中需要用到的依赖
-   devDependencies：在开发、测试环境中用到的依赖

## npm 安装本地包

dependencies

```js
npm install <package_name>
or
npm install <package_name> --save
```

devDependencies：在开发、测试环境中用到的依赖

```js
npm install <package_name> --save-dev
```

### 安装指定版本包

```js
npm install nuxt@latest
npm install nuxt@3.0.0
npm install nuxt@">=3.0.0 <3.2.0"
```

### 更新本地包

```js
npm outdated // 查看是否有新版本
npm update // 更新所有包版本
npm update <package_name> // 更新某个包版本
```

npm update 的工作过程

1. 先到远程仓库查询最新版本
2. 然后对比本地版本，如果本地版本不存在，或者远程版本较新
3. 查看 package.json 中对应的语义版本规则
4. 如果当前新版本符合语义规则，就更新，否则不更新

### 拆卸本地包

```js
npm uninstall <package_name>
```

## npm 全局包管理

```js
npm install <package_name> -g // 全局安装一个包
npm outdated -g --depth=0 // 全局检测包版本
npm update <package_name> -g // 全局更新一个包
npm uninstall <package_name> -g // 全局删除一个包
```

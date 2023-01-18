---
title: Git常用命令的使用
date: 2023-01-11 12:09:38
tags:
---

### git名词

- workspace :工作区
- Index/Stage : 暂存区
- Repository : 仓库区(或本地仓库)
- Remote :远程仓库区

### 常用操作分部解析

- 在任何目录创建新的git仓库,执行 git init**,** .git文件就是本地的一个仓库
- git status 查看修改状态
- git add ,是将文件放入了暂存区,可以使用git checkout "文件名" 将文件从暂存区重新拿到工作区
- git commit ,是将文件从暂存区放入到本地.git仓库
- git status, 查看文件的状态,在暂存区还是在本地仓库,还是已经在本地仓库,或者已经提交(通常这些vscode工具一目了然)
- git pull ,拉取的默认是你自己的分支,详细:git pull origin dev, git pull origin master 拉去远程master分支

### git 配置

```
# 显示当前的Git配置
$ git config --list

# 编辑Git配置文件
$ git config -e [--global]

# 设置提交代码时的用户信息
$ git config [--global] user.name "[name]"
$ git config [--global] user.email "[email address]"
```

### 常用流程

- git add .
- git commit -m ' '
- git pull
- git push

### 常用git命令行快捷方式

- gaa ...... git add .
- gcam '' ...... git commit -m ''
- gl ....... git pull
- gp ....... git push

执行cat ~/.oh-my-zsh/plugins/git/git.plugin.zsh 查看更多简写

### 常用命令

- git log
- git reflog(涉及到的所有操作步骤)
- git reset --hard HEAD
- git checkout file 恢复暂存区的文件到工作区
- git checkout branch 切换分支
- git stash
- git stash pop
- git branch -r 查看远程分支
- git branch -a 查看所有分支
- git remote 列出所有的远程主机
- git pull --all 拉取远程所有的分支
- git commit --amend -message="”(修改最近的一次提交注释)
- `git checkout (branchname)` 切换分支命令
- `git branch (branchname)` 创建分支命令
- `git branch -d (branchname)` 删除分支命令
- git push origin --delete Chapater6 删除远程分支命令

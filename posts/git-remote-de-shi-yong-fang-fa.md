---
title: 'git 的使用方法'
date: 2020-04-01 14:16:27
tags: []
published: true
hideInList: true
feature: 
isTop: false
---
### git remote 的使用方法
1. 创建好项目之后使用 git init ;
2. git remote -v 查看仓库的远程
3. 使用 git remote add origin 远程仓库链接
4. git remote -v 再次查看仓库的远程
5. git add ./ 添加要提交的文件
6. git commit -m ‘提交名称’
7. 提交前先拉一下远程仓库，git pull origin master；
8. 若有冲突使用git merge origin/master --allow-unrelated-histories合并冲突
9. git push -u origin master 推送到远程仓库

### 创建分支
1. git checkout -b 分支名
2. git branch
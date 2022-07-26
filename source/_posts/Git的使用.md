---
title: Git命令
toc: True
tag: git
category: Git
date: 2022-07-26
---
# Git使用

1. git init
   1. 初始化仓库文件夹，自动生成.git文件夹
2. git add *文件名*
   1. 将目的文件添加到暂存区
   2. git add .  表示将当前路径下所有文件均添加到暂存区
3. git commit -m "注释"
   1. 将暂存区文件提交到本地仓库当前分支中
   2. git status 可以查看当前暂存区文件，以及文件修改后是否添加到暂存区，未添加则会显示为红色，已添加则会显示为绿色
4. git push
   1. 将本地仓库push到远程仓库当前分支中
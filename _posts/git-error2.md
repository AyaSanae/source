---
title: 解决Git push报shallow update not allowed 
date: 2020-04-07 13:21
categories:
    Fix Error
tags:
    Git
excerpt: "shallow update not allowed"
---

## 原因
克隆远程仓库时加入了--depth=1选项

## 解决方法

### 方法一
删掉本地的.git然后`git init`就可以重新推送了,这种方法会丢失之前的提交

### 方法二
如果不想丢失之前的提交就可以使用
`git filter-branch -- --all `
去掉克隆的提交的 grafted 标记就可以重新推送了.


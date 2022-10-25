---
title: 解决Git合并报-refusing to merge unrelated histories
date: 2020-04-07 12:43
categories:
    Fix Error
tags:
    Git
description: refusing to merge unrelated histories
---

## 原因
导致这种错误是因为两个仓库没有关系,所以在合并的时候远端仓库觉得和本地仓库没有关系,就报了refusing to merge unrelated histories

## 解决方法
加上--allow-unrelated-histories选项,将两个不相干仓库强行合并

```
git pull origin master --allow-unrelated-histories
```

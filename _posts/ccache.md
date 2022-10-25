---
title: 使用Ccache来加速内核的编译
date: 2020-06-20 12:51
categories: Kernel
tags: 偷懒
description: 编译一个Android内核,我需要8min+,太久了,无法忍受.使用Ccache来加速第二次的内核编译时间
---

编译一个Android内核,我需要8min+,太久了,无法忍受.
使用Ccache来加速第二次的内核编译时间

## Ccache简介
ccache 工具通过将头文件高速缓存到源文件之中而改进了构建性能，因而通过减少每一步编译时添加头文件所需要的时间而提高了构建速度.

## 安装Ccache
```
sudo apt install ccache
```

## 配置ccache
1. 设置ccache的缓存目录
```
export CCACHE_DIR=/home/liak/.ccache     //目录可自定
```

2. 设置缓存大小
```
ccache -M 50G      //大小可根据需求自定
```

3. 检查ccache的配置信息
```
ccache -s
```

以下是我的返回信息      //未进行前两步配置
```
liak@Liak:~$ ccache -s
cache directory                     /home/user/.ccache
primary config                      /home/user/.ccache/ccache.conf
secondary config      (readonly)    /etc/ccache.conf
cache hit (direct)                     0
cache hit (preprocessed)               0
cache miss                             0
cache hit rate                      0.00 %
cleanups performed                     0
files in cache                         0
cache size                           0.0 kB
max cache size                       5.0 GB
```

## 应用ccache
```
export USE_CCACHE=1     //启动
```

修改Makefile
类似于
```
CROSS_COMPILE   ?=/home/liak/kernel_work/toolchain_9/aarch64-linux-android-4.9/bin/aarch64-linux-android-
ARCH		?= $(SUBARCH)
CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
```
改成
```
CROSS_COMPILE   ?= ccache /home/liak/kernel_work/toolchain_9/aarch64-linux-android-4.9/bin/aarch64-linux-android-
ARCH		?= $(SUBARCH)
CROSS_COMPILE	?= $(CONFIG_CROSS_COMPILE:"%"=%)
```

当你第二次编译的时候,你就可以享受ccache带来的快感
enjoy:smile:.

## 拓展
ccache不仅仅可以用来加速编译Android内核,它还可以加速Xcode以及gcc还有g++编译的项目
ccache只有在第二次编译或以上的效果才会体现出来
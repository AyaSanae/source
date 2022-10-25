---
title: 在Ubuntu上创建SWAP
date: 2021-07-23 17:51
categories:
    Ubuntu
tags:
    Ubuntu
description: 在腾讯云搞了台服务器,但是内存只有1G,很容易引发OOM;swap可以将硬盘空间转成内存空间但是速度肯定比不上内存,但至少不会OOM；
---

## 0x01 创建swap文件
```
sudo dd if=/dev/zero of=/Swapfile bs=1M count=2048

//if=input file，这里表示往文件中写0
//of=output file，这是在根目录下创建Swapfile文件
//bs=bytes，设置每个块大小，后面的数值大,写的速度相对快一点,但也不是无限的.
//count表示有多少个这种块
//新建的Swap文件大小为bs*count的大小
```

## 0x02 格式化并启用swap文件
```
sudo mkswap /Swapfile    //格式化根目录下的Swapfile文件
sudo swapon /Swapfile    //启用根目录下的Swapfile文件
```

## 0x03 查看结果
```
free -m //当Swap不为0时表示成功
```

Enjoy.
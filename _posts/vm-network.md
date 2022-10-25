---
title: Vm环境下Kali没网
date: 2021-06-27 12:21
categories:
    - Fix Error
tag:
    - Kali
description: 网络配置不当
---
# 解决方法

## 0x01 重置虚拟机网络配置
    这个不是这篇文章的侧重点所以可以看: https://jingyan.baidu.com/article/3c343ff7e366820d3679637f.html

## 0x02 修改IP配置
### 静态配置
配置文件位于 /etc/network/interfaces

sudo vim /etc/network/interfaces
加入以下语句：

auto eth0
iface eth0 inet static
address xxx.xxx.xxx.xxx #IP地址
netmask xxx.xxx.xxx.xxx #子网掩码
gateway xxx.xxx.xxx.xxx #网关

### 动态配置
配置文件位于 /etc/network/interfaces
sudo vim /etc/network/interfaces
加入以下语句：
auto eth0
iface eth0 inet dhcp

推荐动态配置,能少输一点

## 0x03 修改网卡的管理信息
目标文件位于:/etc/NetworkManager/NetworkManager.conf
sudo vim /etc/NetworkManager/NetworkManager.conf
把最后一行的managed=false改为managed=true即可

## 0x04 重启网卡
sudo /etc/init.d/networking restart

# Enjoy.
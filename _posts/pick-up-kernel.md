---
title: 提取Android手机内核
date: 2020-04-07 12:01:21
comments: true
categories: 
    Android
tags:
    Android
    Kernel
description: 手工提取
---

## 1.找到内核位置
手机进入recovery模式,连接电脑,开adb
```
adb shell
cat /etc/recovery.fstab
```
得到内核的位置
以下是我报告

```
# mount point	fstype		device			[device2]

/modem         emmc     /dev/block/platform/msm_sdcc.1/by-name/modem    flags=backup=1
/sbl1          emmc     /dev/block/platform/msm_sdcc.1/by-name/sbl1   flags=backup=1
#OPPO 2014-02-18 hewei delete for without sbl2 and sbl3
#/sbl2          emmc     /dev/block/platform/msm_sdcc.1/by-name/sbl2
#/sbl3          emmc     /dev/block/platform/msm_sdcc.1/by-name/sbl3
/rpm           emmc     /dev/block/platform/msm_sdcc.1/by-name/rpm   flags=backup=1
/tz            emmc     /dev/block/platform/msm_sdcc.1/by-name/tz   flags=backup=1
/modem_st1     emmc     /dev/block/platform/msm_sdcc.1/by-name/modemst1   flags=backup=1
/modem_st2     emmc     /dev/block/platform/msm_sdcc.1/by-name/modemst2   flags=backup=1
/static_nv_bk  emmc     /dev/block/platform/msm_sdcc.1/by-name/oppostanvbk   flags=backup=1
/oppodycnvbk   emmc     /dev/block/platform/msm_sdcc.1/by-name/oppodycnvbk   flags=backup=1
/aboot         emmc     /dev/block/platform/msm_sdcc.1/by-name/aboot   flags=backup=1
/boot		   emmc		/dev/block/platform/msm_sdcc.1/by-name/boot
/system		   ext4		/dev/block/platform/msm_sdcc.1/by-name/system
/system_image      emmc		/dev/block/platform/msm_sdcc.1/by-name/system
/data		   ext4		/dev/block/platform/msm_sdcc.1/by-name/userdata
/cache		   ext4		/dev/block/platform/msm_sdcc.1/by-name/cache
/misc		   emmc		/dev/block/platform/msm_sdcc.1/by-name/misc
/recovery	   emmc		/dev/block/platform/msm_sdcc.1/by-name/recovery    flags=backup=1
#OPPO 2014-01-14 hewei Add for logo volume, reserve4 volume and DRIVER.ISO
/logo          emmc       /dev/block/platform/msm_sdcc.1/by-name/LOGO  flags=backup=1
/reserve4      emmc     /dev/block/platform/msm_sdcc.1/by-name/reserve4  flags=backup=1
#/driver      emmc     /dev/block/platform/msm_sdcc.1/by-name/DRIVER

#OPPO 2014-01-26 hewei modify for 13077  mount  T-card and sdcard
/external_sd		vfat		/dev/block/mmcblk1p1	/dev/block/mmcblk1    flags=display="Micro SDcard";storage;wipeingui;removable;backup=0
/usbdisk        vfat        /dev/block/sda1 /dev/block/sda                 flags=fsflags=utf8;display="OTG-U盘(usbdisk)";storage;removable

```
可以看到boot分区对应的路径是/dev/block/platform/msm_sdcc.1/by-name/boot

## 2.提取内核
使用dd工具
`dd if=boot分区的路径 of=输出路径.img`
我的:
`dd if=/dev/block/platform/msm_sdcc.1/by-name/boot of=/sdcard/kernel.img`

sdcard里的kernel.img就是我提取的内核了
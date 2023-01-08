---
title: 精简Android内核模块
date: 2020-08-01 19:21
categories:
    - Kernel
tags:
    - 优化
excerpt: "去除编译时产生的多余符号."
---

## 原理:
        去除编译时产生的多余符号.
        至于为什么会产生多余符号,暂不深究.

## 做法:
        !: 文件一旦strip后就不能恢复原样了,被strip后的文件不包含调试信息
        使用arm-linux-strip工具,这里用arm的交叉编译工具链做演示,当然你也可以使用strip
```
        arm-linux-strip kernel_name.ko
```
        不保留debug的体积会小点.
        以上,即可. 相当于给内核模块脱衣 A.A
        *不影响使用,体积减少很多*
        经测试,200多m的qcacld-3.0被削到12m.

## 拓展:
        strip不仅仅可以精简内核模块，具体如下:
```
        # include <stdio.h>
            int main(){
            printf("Hello World");
            return 0;
        }       
```
        C语言的Hello World在我的GCC版本编译下大小如下:
```
        ubuntu@VM-0-2-ubuntu:~/test$ ls -l
        total 16
        -rwxrwxr-x 1 ubuntu ubuntu 8312 Aug  1 19:36 a.out
        -rw-rw-r-- 1 ubuntu ubuntu   69 Aug  1 19:36 hello_world.c
        ubuntu@VM-0-2-ubuntu:~/test$ 
```
        精简后:
```
        ubuntu@VM-0-2-ubuntu:~/test$ ls -l
        total 16
        -rwxrwxr-x 1 ubuntu ubuntu 8312 Aug  1 19:38 a.out
        -rw-rw-r-- 1 ubuntu ubuntu   69 Aug  1 19:36 hello_world.c
        ubuntu@VM-0-2-ubuntu:~/test$ strip a.out 
        ubuntu@VM-0-2-ubuntu:~/test$ ls -l
        total 12
        -rwxrwxr-x 1 ubuntu ubuntu 6128 Aug  1 19:39 a.out
        -rw-rw-r-- 1 ubuntu ubuntu   69 Aug  1 19:36 hello_world.c
        ubuntu@VM-0-2-ubuntu:~/test$ ./a.out 
        Hello Worldubuntu@VM-0-2-ubuntu:~/test$ 
        ubuntu@VM-0-2-ubuntu:~/test$ 
```

    大小由8312字节减到6128字节，且不影响使用.

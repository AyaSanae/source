---
title: 关于用char指针表示字符串的一些问题
date: 2022-11-22 19:56:32
tags: C/C++
catagories: 编程语言
description: 有关文字常量区
---
当我们用char指针表示字符串时,如果直接赋值会编译器怎么样?
```
char *str = "Hello";
```
编译时,编译器报了:
```
//禁止将字符串常量转位char*
 warning: ISO C++ forbids converting a string constant to 'char*' [-Wwrite-strings]
     char *str = "Hello";
```
原因很简单,这样的赋值方法会先在文字常量区申请一块新空间然后写入"Hello"最后str指针指向这块空间的首地址.
看起来像这样:
![指向文字常量区](/assets/char-str-pointer/1.jpg)

当我们尝试修改str所指向的值时,就会报错.
```
    char *str = "Hello";

    cout << str<< endl;

    strcpy(str,"World");

    cout << str<< endl;

```
输出结果:
```
Hello
Segmentation fault
```

因为文字常量区的内容不能修改.
那还有其他办法吗?
我们可以将字符串放到栈区或者全局区.
```
    char *str = new char[32];   //在栈区申请空间

    strcpy(str,"Hello");

    cout<< str <<endl;

    strcpy(str,"World");

    cout<< str <<endl;

```
输出结果:
```
Hello
World
```
---
title: 结构体的深拷贝与浅拷贝
date: 2022-11-22 19:18:36
tags: C/C++
catagories: 编程语言
excerpt: "各自优缺点"
---
浅拷贝:将结构体变量内存空间内容复制一份放到另一个相同类型的结构体变量内存空间中.
比如我们有
```c++
struct stu{
    int num;
}a,b;
```
![浅拷贝](/assets/struct-copy/1.jpg)

这是结构体间的默认赋值方式,一般来说不会出现问题,但是如果结构体里有指针成员,这种方式会带来多次释放堆区空间的问题和一定的风险.

如果说stu里有一个指针成员：
```c++
    struct stu{
        int num;
        char *name;
    }a,b;
```
这时候给a初值,并令b=a
```c++
    a.num = 100;
    a.name = new char[32];
    strcpy(a.name,"bob");

    b=a;


    cout << a.name << "  " << b.name << endl;

    delete [] a.name;

    cout << b.name << endl;
```
Result:
```
bob bob
```
很明显第二个cout里的b.name没有输出.

由于是浅拷贝,所以a变量内存空间里的值会原封不动的拷贝一份给到b,那么就有b.name == a.name这么一个关系
两个指针同时指向了同一个地址
看起来像这样:
![指向同一地址](/assets/struct-copy/2.jpg)

如果我们释放a.name所指向的空间时程序不会出错,但是当我们不调整b.name指针指向,释放b.name指向的空间时程序就会出错,造成了重复释放.

即使,没有释放b.name所指向的空间,也有不小风险,同时也是指针的经典风险.
因为我们已经释放了a.name所指向的空间,那么我们给某个变量c划分空间时,系统可能会给c分配了b.name所指向的空间,那么我们对b.name操作时,实际上更改的,是c的内容.读b.name所指向的空间时,读出的是c的值.

*用浅拷贝时结构体内没有指针的话是没有问题的,有指针时,根本问题就是拷贝后,两个结构体成员指针同时指向了同一个地址.*

那么要解决的话,只需要给b.name单独划一块空间,让b.name指向这块空间,并且把a.name指针所指向的内容拷贝到新划分的空间,这样就不会有两个结构体成员指针同时指向了同一个地址了,也就是--深拷贝.
RT:
![深拷贝](/assets/struct-copy/3.jpg)

代码实现:
```c++
#include <iostream>
#include <string.h>

using namespace std;

int main(){
    struct stu{
        int num;
        char *name;
    }a,b;

    a.num = 100;
    a.name = new char[32];
    strcpy(a.name,"bob");

    b=a;
    b.name = new char[32];  //划分新空间,并调整b.name指针指向
    strcpy(b.name,a.name);

    cout << a.name << "  " << b.name << endl;

    delete [] a.name;
    delete [] b.name;
    return 0;
}
```
Result:
```
bob  bob
```

&#128515;


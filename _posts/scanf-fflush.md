---
title: scanf()与循环与缓冲区
date: 2022-10-27 20:43:06
tags: C
catagories: 编程语言
description: 有时Scanf()与循环结合判断输入的值是否合理时会陷入死循环
---
有时Scanf()与循环结合判断输入的值是否合理时会陷入死循环

## Source
```
    float course_1=-1,course_2=-1,result;
    while(1){
        printf("Please enter the score of the first course:");
        scanf("%f",&course_1);
            if(course_1 < 0 || course_1 > 100){
                    printf("Please enter the correct score!\n");
                    continue;
            }
        printf("\nPlease enter the score of the second course:");
        scanf("%f",&course_2);
            if(course_2 < 0 || course_2 > 100){
                    printf("Please enter the correct score!\n");
                    continue;
            }
            break;
    }       
    result = (course_1 + course_2)/2;
    printf("\nThe average score is:%.1f",result);
}    
```
## 输出

如果输入的值为非字符时结果为:
```
Please enter the score of the first course:20

Please enter the score of the second course:30

The average score is:25.0
```
如果输入的值为字符时结果为:
```
Please enter the score of the first course:a

Please enter the score of the first course:Please enter the correct score!
Please enter the score of the first course:Please enter the correct score!
Please enter the score of the first course:Please enter the correct score!
...
```

## 原因

因为scanf里占位符是%d,也就是录入缓冲区中的整形数字.
当缓冲区有字符,scanf便会读取,但是不是整形就不会录入.
        
第一次输入了a,是非整形,scanf略过了但是又没有清空缓冲区,第二次便不会要求我输入
因为缓冲区里有字符,又略过,如此便是死循环

## 解决
可以使用fflush(stdin)来清空缓冲区.
```
   float course_1=-1,course_2=-1,result;
    while(1){
        printf("Please enter the score of the first course:");
        scanf("%f",&course_1);
        fflush(stdin)
            if(course_1 < 0 || course_1 > 100){
                    printf("Please enter the correct score!\n");
                    continue;
            }
        printf("\nPlease enter the score of the second course:");
        scanf("%f",&course_2);
        fflush(stdin)
            if(course_2 < 0 || course_2 > 100){
                    printf("Please enter the correct score!\n");
                    continue;
            }
            break;
    }       
    result = (course_1 + course_2)/2;
    printf("\nThe average score is:%.1f",result);
}    
```

这时候程序就正常了.
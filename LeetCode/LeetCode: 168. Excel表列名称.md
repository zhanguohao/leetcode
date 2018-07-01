# LeetCode: 168. Excel表列名称

[TOC]





## 1、题目描述

```
给定一个正整数，返回它在 Excel 表中相对应的列名称。

例如，

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
    ...
示例 1:

输入: 1
输出: "A"
示例 2:

输入: 28
输出: "AB"
示例 3:

输入: 701
输出: "ZY"
```





## 2、解题思路

​	首先，我们判断这个数需要几位存储，然后判断完以后，从后向前依次计算



​	这里有一点需要注意，就是他不是从0开始的，是从1开始的

​	怎样进行转换呢

​	首先是26以下的数：



- 末位数

  末位数是`1~26`，对应着`A~Z`

  我们想要将之变成字符，通过加A的形式，所以，将1~26与0~25一一对应

  因此，我们将这个数减一，然后取26的余，就能得到想要的字符

- 非末位数

  例如，导数第二位数，这个数表示有多少个26，由于是从1开始计数的，需要减去1才行

  如52，正确结果应该是AZ

  



```c
char* convertToTitle(int n) {
    char *result;
    int length = 1;
    int temp = n;
    while ( (temp-1)/26 > 0 ) {
        temp = (temp-1)/26;
        length++;
    }
    result = (char *) malloc(sizeof(char) * length + 1);
    result[length--] = '\0';

    while (length >= 0) {
        result[length] = (n-1) % 26 + 'A';
        n = (n-1) / 26 ;
        length--;
    }

    return result;
}
```


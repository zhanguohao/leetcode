# LeetCode: 693. 交替位二进制数

[TOC]



## 1、题目描述





给定一个正整数，检查他是否为交替位二进制数：换句话说，就是他的二进制数相邻的两个位数永不相等。

**示例 1:**

```
输入: 5
输出: True
解释:
5的二进制数是: 101
```

**示例 2:**

```
输入: 7
输出: False
解释:
7的二进制数是: 111
```

**示例 3:**

```
输入: 11
输出: False
解释:
11的二进制数是: 1011
```

 **示例 4:**

```
输入: 10
输出: True
解释:
10的二进制数是: 1010
```



## 2、解题思路

​	从左面开始，找到第一个1，然后,不断的进行异或，如果异或是0，返回false

```c
bool hasAlternatingBits(int n) {
    
    if (n == 0) {
        return true;
    }

    int left = 0;
    int right = 0;

    bool start = false;
    for (int i = 31; i > 0; i--) {
        if (!start) {
            if (n & (1 << i)) {
                start = true;
                i++;
            }
        } else {
            left = (n & (1 << i)) == 0 ? 0 : 1;
            right = (n & (1 << (i - 1))) == 0 ? 0 : 1;
            if ((left ^ right) == 0) {
                return false;
            }
        }
    }

    return true;
}
```


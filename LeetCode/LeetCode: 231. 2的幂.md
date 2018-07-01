# LeetCode: 231. 2的幂

[TOC]

## 1、题目描述



给定一个整数，编写一个函数来判断它是否是 2 的幂次方。

**示例 1:**

```
输入: 1
输出: true
解释: 20 = 1
```

**示例 2:**

```
输入: 16
输出: true
解释: 24 = 16
```

**示例 3:**

```
输入: 218
输出: false
```





## 2、解题思路



​	整数如果小于等于0，返回false

​	如果整数中1的个数不为1，返回假

```c
bool isPowerOfTwo(int n) {
    if(n<=0){
        return false;
    }
    
    int count = 0;
    for (int i = 0; i < 32; i++) {
        if (n & 1) {
            count++;
        }
        n = n >> 1;
    }

    if (count != 1) {
        return false;
    } else {
        return true;
    }
    
     

}
```



​	简单写法：

```
 if(n <=0){
        return false;
    }

    return !(n & (n-1));
     
```


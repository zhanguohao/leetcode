# LeetCode: 367. 有效的完全平方数

[TOC]

## 1、题目描述



给定一个正整数 *num*，编写一个函数，如果 *num* 是一个完全平方数，则返回 True，否则返回 False。

**注意：**不要使用任何内置的库函数，如  `sqrt`。

**示例 1：**

```
输入： 16

输出： True
```

 

**示例 2：**

```
输入： 14

输出： False
```

**归功于:**

特别感谢 [@elmirap](https://discuss.leetcode.com/user/elmirap) 添加此问题并创建所有测试用例。







## 2、解题思路

打印出前面的数的平方，发现了规律，

```
num: 0	 pow2:0
num: 1	 pow2:1
num: 2	 pow2:4
num: 3	 pow2:9
num: 4	 pow2:16
num: 5	 pow2:25
num: 6	 pow2:36
num: 7	 pow2:49
num: 8	 pow2:64
num: 9	 pow2:81
num: 10	 pow2:100
num: 11	 pow2:121
num: 12	 pow2:144
num: 13	 pow2:169
num: 14	 pow2:196
num: 15	 pow2:225
num: 16	 pow2:256
num: 17	 pow2:289
num: 18	 pow2:324
num: 19	 pow2:361
num: 20	 pow2:400
num: 21	 pow2:441
num: 22	 pow2:484
num: 23	 pow2:529
num: 24	 pow2:576
num: 25	 pow2:625
num: 26	 pow2:676
num: 27	 pow2:729
num: 28	 pow2:784
num: 29	 pow2:841
num: 30	 pow2:900
num: 31	 pow2:961
```



​	通过列举所有的完全平方数，1，4，9，16，25，36，49，64，81，100…等等，发现完全平方数的差都为奇数，即1，3，5，7，9，11，13，15…等等~所以可以判断完全平方数应该是N个奇数的和。



```c
bool isPerfectSquare(int num) {
    if (num < 0) {
        return false;
    }

    for (int i = 1; num > 0; i += 2) {
        num -= i;
    }
    if(num == 0){
        return true;
    } else{
        return false;
    }
    
}
```








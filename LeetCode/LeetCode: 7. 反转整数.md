# LeetCode: 7. 反转整数

[TOC]

## 1、题目要求

```
Given a 32-bit signed integer, reverse digits of an integer.

Example 1:

Input: 123
Output: 321
Example 2:

Input: -123
Output: -321
Example 3:

Input: 120
Output: 21
Note:
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−231,  231 − 1]. For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.
```



给定一个 32 位有符号整数，将整数中的数字进行反转。

**示例 1:**

```
输入: 123
输出: 321
```

 **示例 2:**

```
输入: -123
输出: -321
```

**示例 3:**

```
输入: 120
输出: 21
```

**注意:**

假设我们的环境只能存储 32 位有符号整数，其数值范围是 [−231,  231 − 1]。根据这个假设，如果反转后的整数溢出，则返回 0。



## 2、解题思路

### 2.1 判断边界条件

```c
int reverse(int x) {
    if (!(x ^  (1<<(sizeof(int)*8 -1)))){
        return 0;
    }
    bool flag_positive = x > 0 ? true : false;
    int result;
    int num = flag_positive ? x  : -x ;
    result = num % 10;
    num /= 10;
    while (num > 0) {
        if (result > (INT_MAX/10)){
            return 0;
        }
        result = result * 10 + num % 10;
        num = num / 10;
    }

    if (!flag_positive) {
        result = -result;
    }

    return result;
}
```

### 2.2 使用long保存数据

### 2.3 根据除法结果

​	判断前面一次

```
	int t = res * 10 + x % 10;
	if (t / 10 != res) return 0;
	res = t;
	x /= 10;
```
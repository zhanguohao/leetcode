# LeetCode: 9. 回文数

[TOC]

## 1、题目描述

```
Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.

Example 1:

Input: 121
Output: true
Example 2:

Input: -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
Example 3:

Input: 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
Follow up:

Coud you solve it without converting the integer to a string?
```

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**示例 1:**

```
输入: 121
输出: true
```

**示例 2:**

```
输入: -121
输出: false
解释: 从左向右读, 为 -121 。 从右向左读, 为 121- 。因此它不是一个回文数。
```

**示例 3:**

```
输入: 10
输出: false
解释: 从右向左读, 为 01 。因此它不是一个回文数。
```

**进阶:**

你能不将整数转为字符串来解决这个问题吗？



## 2、解题思路

### 2.1 按位比较

​	首先判断一共有多少位，例如123，1234，分别是3，4位，实际上，我们只需要比较位数的一般，也就是3位的，比较1次，4位的比较两次

```c
bool isPalindrome(int x) {
    if (x<0){
        return false;
    }
    int nums = 1;
    int temp = x;
    bool flag = true;
    int left_num, right_num;
    if (x < 10) {
        return true;
    }
    while ((temp = temp / 10) > 0) {
        nums++;
    }

    for (int i = 0, j = 1; i < nums / 2; i++) {
        left_num = right_num = x;
        while (nums - i - j++ > 0) {
            left_num /= 10;
        }
        left_num %= 10;
        j = i;
        while (j-- > 0) {
            right_num /= 10;
        }
        right_num %= 10;

        j = 1;
        if (left_num != right_num) {
            flag = false;
            break;
        }
    }
    return flag;
}
```

### 2.2 按位取（减少计算量）

​	上面的方式是按照位数取得，下面按照同时得到最高位的阶数，也就是$10^n$

```c
bool isPalindrome(int x) {
    if (x < 0) {
        return false;
    } else if (x < 10) {
        return true;
    }
    int div = 1;
    int nums = 1;
    int temp = x;
    bool flag = true;
    while ((temp = temp / 10) > 0) {
        nums++;
        div *= 10;
    }

    for (int i = 0; i < nums / 2; i++) {
        if (((x / div) %10) != (x % 10)){
            flag = false;
            break;
        }
        x /=10;
        div /=100;

    }

    return flag;
}
```

​	有一点可以注意一下，在leetcode上提交的运行时间不一定就很准，第一次提交需要124ms，第二次就用了104ms，上面的按理说会多一些，大约120ms左右；



### 2.3 整数比较

​	前面是按照每一位进行比较，实际上有些慢，如果直接取出多位，直接比较数字，会快很多

​	后面的几位很好取出来，前面的就不太好取出来，需要反着取出来

```c
bool isPalindrome(int x) {
    if (x < 0) {
        return false;
    } else if (x < 10) {
        return true;
    }
    int div_left = 1;
    int div_right = 1;
    int nums = 1;
    int temp = x;
    bool flag = true;
    int left_num = 0, right_num = 0;
    int i;
    while ((temp = temp / 10) > 0) {
        nums++;
    }
    i = nums - (nums / 2);
    while (i-- > 0) {
        div_left *= 10;
    }
    if ((nums % 2) == 0) {
        div_right = div_left;
    } else {
        div_right = div_left / 10;
    }


    left_num = x / div_left % 10;
    div_left *= 10;
    for (int i = 0; i < nums / 2 - 1; i++) {
        left_num = 10 * left_num + x / div_left % 10;
        div_left *= 10;
    }


    right_num = x % (div_right);
    if (left_num != right_num) {
        flag = false;
    }
    return flag;
}
```




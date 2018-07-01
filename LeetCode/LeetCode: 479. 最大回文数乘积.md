# LeetCode: 479. 最大回文数乘积

[TOC]

## 1、题目描述



你需要找到由两个 n 位数的乘积组成的最大回文数。

由于结果会很大，你只需返回最大回文数 mod 1337得到的结果。

**示例:**

输入: 2

输出: 987

解释: 99 x 91 = 9009, 9009 % 1337 = 987

**说明:**

n 的取值范围为 [1,8]。



## 2、解题思路

​	首先，如果是两位数，那么我们从99开始，构造回文串，9999，判断是不可以被两位数整除，一直判断到少一位数，也就是比9大就可以

​	也就是说分两步，先构造回文串的数字，然后判断能不能被整除，如果可以，返回



​	用c语言构造回文串写起来有点麻烦，直接用python

```python
class Solution:
    def largestPalindrome(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n == 1:
            return 9;

        upper = 10**n -1
        for i in range(upper, upper//10,-1):
            pal = int(str(i) + "".join(reversed(str(i))))
            for j in range(upper, upper//10,-1):
                if pal // j > upper:
                    break
                if pal % j == 0:
                    return pal % 1337

        return -1
```



​	用python竟然超时了，没办法，还是用c来写一遍

```c
long buildPalindrome( long num, int digits) {
     long result = num * pow(10, digits);
    int temp;
    for (int i = 0; i < digits; i++) {
        temp = (num / (int) pow(10, i)) % 10 * pow(10, digits - 1 - i);
        result += temp;
    }

    return result;
}


int largestPalindrome(int n) {
    if (n == 1) {
        return 9;
    }

    int upper = pow(10, n) - 1;
    int lower = upper / 10;
    long pal = 9;
    for (int i = upper; i > lower; i--) {
        pal = buildPalindrome(i, n);
        for (int j = upper; j > lower; j--) {
            if (pal / j > upper) {
                break;
            }
            if (pal % j == 0) {
                return pal % 1337;
            }
        }
    }

    return pal;

}
```

​	还是用c可以，没有超时




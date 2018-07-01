# LeetCode: 409. 最长回文串

[TOC]



## 1、题目描述



给定一个包含大写字母和小写字母的字符串，找到通过这些字母构造成的最长的回文串。

在构造过程中，请注意区分大小写。比如 `"Aa"` 不能当做一个回文字符串。

**注意:**
假设字符串的长度不会超过 1010。

**示例 1:** 

```
输入:
"abccccdd"

输出:
7

解释:
我们可以构造的最长的回文串是"dccaccd", 它的长度是 7。
```



## 2、解题思路

​	统计所有字母出现的次数，如果是偶数，直接加上

​	如果是奇数，加上奇数然后减去一

​	最后，判断是否出现过奇数，出现过，就加一

```c
int longestPalindrome(char* s) {
     if (!s) {
        return 0;
    }
    int buff['z'-'A'+1];
    for (int i = 0; i < 'z'-'A'+1; i++) {
        buff[i] = 0;
    }
    bool odd = false;

    int result = 0;
    int length = strlen(s);
    for (int i = 0; i < length; i++) {
        buff[s[i] - 'A']++;
    }

    for (int i = 0; i < 'z' - 'A' + 1; i++) {
        if (buff[i] % 2 == 0) {
            result += buff[i];
        } else {

            result += buff[i] - 1;
            odd = true;
        }
    }

    return result + odd;
    
}
```






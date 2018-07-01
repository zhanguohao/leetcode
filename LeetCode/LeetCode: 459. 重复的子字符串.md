# LeetCode: 459. 重复的子字符串

[TOC]



## 1、题目描述



给定一个非空的字符串，判断它是否可以由它的一个子串重复多次构成。给定的字符串只含有小写英文字母，并且长度不超过10000。

**示例 1:**

```
输入: "abab"

输出: True

解释: 可由子字符串 "ab" 重复两次构成。
```

**示例 2:**

```
输入: "aba"

输出: False
```

**示例 3:**

```
输入: "abcabcabcabc"

输出: True

解释: 可由子字符串 "abc" 重复四次构成。 (或者子字符串 "abcabc" 重复两次构成。)
```





## 2、解题思路

​	实际很好判断，假如我们让整个串的子串，重复n次，n要大于1，那么n肯定是长度的因子

​	例如“abcabcabc”

​	我们首先判断如果重复9次，是不是可以，不可以

​	然后是重复3次（2不是9的因子）可以，退出



```c

bool isRepeated(char *s, int step) {
    int length = strlen(s);
    if (step >= length) {
        return false;
    }

    for (int i = 0; i < length - step; i++) {
        if (s[i] != s[i + step]) {
            return false;
        }
    }
    return true;
}

bool repeatedSubstringPattern(char *s) {

    if (!s) {
        return false;
    }

    bool result = false;
    int length = strlen(s);
    int step = 1;
    for (int i = 1; i <= length; i++) {
        if (length % i == 0) {
            step = i;
            if (result = isRepeated(s, step)) {
                break;
            }
        }
    }

    return result;

}

```





​	

​	
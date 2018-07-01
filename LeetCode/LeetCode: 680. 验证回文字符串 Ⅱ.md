# LeetCode: 680. 验证回文字符串 Ⅱ

[TOC]

## 1、题目描述



给定一个非空字符串 `s`，**最多**删除一个字符。判断是否能成为回文字符串。

**示例 1:**

```
输入: "aba"
输出: True
```

**示例 2:**

```
输入: "abca"
输出: True
解释: 你可以删除c字符。
```

**注意:**

1. 字符串只包含从 a-z 的小写字母。字符串的最大长度是50000。



## 2、解题思路



​	先找出第一个不相等的位置，然后分成两种情况，删掉左面的字符，删掉右面的字符，分别判断，如果都不是回文串，返回假



```c
bool validPalindrome(char* s) {
    
    int left1 = 0;
    int right1 = 0;
    int left2 = 0;
    int right2 = 0;


    int length = strlen(s);

    int interval = 0;

    for (int i = 0; i < length / 2; i++) {
        if (s[i] != s[length - 1 - i]) {

            interval = length - 1 - i - i;
            if (interval <= 1) {
                return true;
            } else {

                left1 = i;
                right1 = length - 2 - i;
                left2 = i + 1;
                right2 = length - 1 - i;
                break;
            }

        }
    }

    bool result1 = true;
    bool result2 = true;

    for (int i = 0; i < (right1 - left1 + 1) / 2; i++) {
        if (s[i + left1] != s[right1 - i]) {
            result1 = false;
            break;
        }
    }

    if (result1) {
        return true;
    }

    for (int i = 0; i < (right2 - left2 + 1) / 2; i++) {
        if (s[i + left2] != s[right2 - i]) {
            result2 = false;
            break;
        }
    }

    if (result2) {
        return true;
    }

    return false;
}
```




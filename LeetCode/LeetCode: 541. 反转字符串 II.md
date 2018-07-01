# LeetCode: 541. 反转字符串 II

[TOC]



## 1、题目描述



给定一个字符串和一个整数 k，你需要对从字符串开头算起的每个 2k 个字符的前k个字符进行反转。如果剩余少于 k 个字符，则将剩余的所有全部反转。如果有小于 2k 但大于或等于 k 个字符，则反转前 k 个字符，并将剩余的字符保持原样。

**示例:**

```
输入: s = "abcdefg", k = 2
输出: "bacdfeg"
```

**要求:**

1. 该字符串只包含小写的英文字母。
2. 给定字符串的长度和 k 在[1, 10000]范围内。



## 2、解题思路

```c
char* reverseStr(char* s, int k) {
    
    if (!s) {
        return s;
    }


    int length = strlen(s);
    char *temp_s = (char *) malloc(sizeof(char) * (length + 1));

    strcpy(temp_s,s);

    int pos = 0;

    int less = 0;
    char *temp = temp_s;
    while (pos < length) {
        if (length - pos >= k) {
            for (int i = 0; i < k / 2; i++) {
                *(temp + pos+i) ^= *(temp + pos + k - 1-i);
                *(temp + pos + k - 1 - i) ^= *(temp + pos+i);
                *(temp + pos+i) ^= *(temp + pos + k - 1 - i);
            }

            pos += 2 * k;
        } else {
            less = length - pos;

            for (int i = 0; i < less / 2; i++) {

                *(temp + pos+i) ^= *(temp + pos + less - 1-i);
                *(temp + pos + less - 1 - i) ^= *(temp + pos+i);
                *(temp + pos+i) ^= *(temp + pos + less - 1 - i);
                
            }

            pos += less;

        }
    }

    return temp_s;
    
    
}
```


# LeetCode: 387. 字符串中的第一个唯一字符

[TOC]

## 1、题目描述



给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

**案例:**

```
s = "leetcode"
返回 0.

s = "loveleetcode",
返回 2.
```

 

**注意事项：**您可以假定该字符串只包含小写字母。



## 2、解题思路

```c
int firstUniqChar(char* s) {
    int *index = (int *) calloc(26, sizeof(int));

    int length = (int) strlen(s);
    for (int i = 0; i < length; i++) {
        index[s[i] - 'a']++;
    }

    for (int i = 0; i < length; i++) {
        if (index[s[i] - 'a'] == 1) {
            return i;
        }
    }

    return -1;
}
```


# LeetCode: 389. 找不同

[TOC]



## 1、题目描述





给定两个字符串 **\*s*** 和 **\*t***，它们只包含小写字母。

字符串 **t** 由字符串 **s** 随机重排，然后在随机位置添加一个字母。

请找出在 **\*t*** 中被添加的字母。

 

**示例:**

```
输入：
s = "abcd"
t = "abcde"

输出：
e

解释：
'e' 是那个被添加的字母。
```



## 2、解题思路



```c
char findTheDifference(char* s, char* t) {
    
    int *index_s = (int *) calloc(26, sizeof(int));
    int *index_t = (int *) calloc(26, sizeof(int));

    int length_s = (int) strlen(s);
    int length_t = (int) strlen(t);

    for (int i = 0; i < length_s; i++) {
        index_s[s[i] - 'a']++;
    }
    for (int i = 0; i < length_t; i++) {
        index_t[t[i] - 'a']++;
    }


    for (int i = 0; i < 26; i++) {
        if (index_t[i] - index_s[i] == 1) {
            return 'a' + i;
        }
    }

    return -1;
    
}
```






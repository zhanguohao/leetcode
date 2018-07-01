# LeetCode: 14. 最长公共前缀

[TOC]

## 1、题目描述

```
编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

示例 1:

输入: ["flower","flow","flight"]
输出: "fl"
示例 2:

输入: ["dog","racecar","car"]
输出: ""
解释: 输入不存在公共前缀。
说明:

所有输入只包含小写字母 a-z 。
```

## 2、解题思路

```c
char* longestCommonPrefix(char** strs, int strsSize) {
    if(strsSize <=0){
        return "";
    }
    // 计算最长前缀的缓冲长度，使用所有字符串中最小的那个
    int buff_length = INT32_MAX;

    for (int i = 0; i < strsSize; i++) {
        if (strlen(*(strs + i)) < buff_length) {
            buff_length = strlen(*(strs + i));
        }
    }

    if (buff_length == 0) {
        return "";
    }
    char *result = (char *) malloc(sizeof(char) * (buff_length + 1));

    int count = 0;
    bool found = false;
    while (!found && count < buff_length) {
        // 判断是不是所有的字符串都有这个字符，如果有一个没有，表示已经找到了最长前缀
        for (int i = 0; i < strsSize - 1; i++) {
            if ((*(strs + i))[count] != (*(strs + i + 1))[count]) {
                found = true;
                break;
            }
        }
        // 如果当前字符存在所有的字符串中，放入缓存，继续寻找
        if (!found) {
            result[count] = (*strs)[count];
            count++;
        }

    }
    result[count] = '\0';
    return result;
}
```


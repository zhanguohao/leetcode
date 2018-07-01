# LeetCode: 434. 字符串中的单词数

[TOC]

## 1、题目描述



统计字符串中的单词个数，这里的单词指的是连续的不是空格的字符。

请注意，你可以假定字符串里不包括任何不可打印的字符。

**示例:**

```
输入: "Hello, my name is John"
输出: 5
```



## 2、解题思路





```c
int countSegments(char* s) {
       bool word = false;

    int count = 0;

    while (*s) {

        if (!word) {
            if (*s != ' ') {
                word = true;
            }
        } else {
            if (*s == ' ') {
                count++;
                word = false;
            }
        }
        s++;
    }

    if(word){
        count++;
    }
    
    return count;
}
```


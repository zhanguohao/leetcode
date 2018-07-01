# LeetCode: 557. 反转字符串中的单词 III

[TOC]



## 1、题目描述





给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

**示例 1:**

```
输入: "Let's take LeetCode contest"
输出: "s'teL ekat edoCteeL tsetnoc" 
```

**注意：**在字符串中，每个单词由单个空格分隔，并且字符串中不会有任何额外的空格。





## 2、解题思路

​	

```c
char* reverseWords(char* s) {
    int length = strlen(s);
    if (length <= 1) {
        return s;
    }


    int current_pos = 0;

    int current_count = 0;
    char *temp = s;
    bool reverse = false;
    while (*temp) {
        if (*temp != ' ') {
            current_count++;
        } else {
            reverse = true;
        }
        if (reverse) {
            for (int i = 0; i < current_count / 2; i++) {
                s[current_pos + i] ^= s[current_pos + current_count - 1 - i];
                s[current_pos + current_count - 1 - i] ^= s[current_pos + i];
                s[current_pos + i] ^= s[current_pos + current_count - 1 - i];
            }
            current_pos += current_count +1;
            reverse = false;
            current_count = 0;
        }
        temp++;

    }

    for (int i = 0; i < current_count / 2; i++) {
        s[current_pos + i] ^= s[current_pos + current_count - 1 - i];
        s[current_pos + current_count - 1 - i] ^= s[current_pos + i];
        s[current_pos + i] ^= s[current_pos + current_count - 1 - i];
    }
    
    return s;
}
```


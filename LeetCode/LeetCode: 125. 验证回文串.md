# LeetCode: 125. 验证回文串

[TOC]

## 1、题目描述



给定一个字符串，验证它是否是回文串，只考虑字母和数字字符，可以忽略字母的大小写。

**说明：**本题中，我们将空字符串定义为有效的回文串。

**示例 1:**

```
输入: "A man, a plan, a canal: Panama"
输出: true
```

**示例 2:**

```
输入: "race a car"
输出: false
```



## 2、解题思路

​	

```c
bool isAlphaNumber(char c) {
    if (c >= '0' && c <= '9' || c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z') {
        return true;
    } else {
        return false;
    }
}


bool isPalindrome(char *s) {

    int length = (int) strlen(s);

    if (length <= 1) {
        return true;
    }

    // 构造缓冲字符串,去掉所有的非判断字符，将大写转换为小写
    char *buff = (char *) malloc(sizeof(int) * length + 1);
    int buff_pos = 0;
    for (int i = 0; i < length; i++) {
        if (isAlphaNumber(s[i])) {
            if (s[i] >= 'A' && s[i] <= 'Z') {
                buff[buff_pos++] = s[i] + 32;
            } else {
                buff[buff_pos++] = s[i];
            }
        }
    }

    bool result = true;

    for (int i = 0; i < buff_pos / 2; i++) {
        if (buff[i] != buff[buff_pos - 1 - i]) {
            result = false;
            break;
        }
    }


    return result;
}
```


# LeetCode: 345. 反转字符串中的元音字母

[TOC]



## 1、题目描述



编写一个函数，以字符串作为输入，反转该字符串中的元音字母。

**示例 1：**
给定 s = "hello", 返回 "holle".

**示例 2：**
给定 s = "leetcode", 返回 "leotcede".

**注意:**
元音字母不包括 "y".





## 2、 解题思路



```c
bool isVowel(char ch) {
    switch (ch) {
        case 'a':
        case 'e':
        case 'i':
        case 'o':
        case 'u':
        case 'A':
        case 'E':
        case 'I':
        case 'O':
        case 'U':
            return true;
        default:
            return false;
    }
}


char *reverseVowels(char *s) {
    int length = strlen(s);
    if (length <= 1) {
        return s;
    }

    char *buf = (char *) malloc(sizeof(char) * length + 1);
    strcpy(buf, s);
    int left = 0;
    int right = length - 1;

    for (int i = 0; i < length / 2; i++) {


        while (!isVowel(buf[left]) && left < length) {
            left++;
        }

        while (!isVowel(buf[right]) && right >= 0) {
            right--;
        }

        if (left < right) {

            buf[left] ^= buf[right];
            buf[right] ^= buf[left];
            buf[left] ^= buf[right];
            left++;
            right--;
        } else {
            break;
        }

    }

    return buf;


}
```





​	注意：下面的代码，在mac上直接跑有问题，不支持直接修改字符串

```c
char yuanyin[] = "aeiouAEIOU";

char *reverseVowels(char *s) {
    int length = strlen(s);
    if (length <= 1) {
        return s;
    }

    //char *buf = (char *) malloc(sizeof(char) * length + 1);
    //strcpy(buf, s);
    int left = 0;
    int right = length - 1;

    for (int i = 0; i < length / 2; i++) {


        while (strchr(yuanyin, s[left]) == NULL && left < length) {
            left++;
        }

        while (strchr(yuanyin, s[right]) == NULL && right >= 0) {
            right--;
        }

        if (left < right) {

            s[left] ^= s[right];
            s[right] ^= s[left];
            s[left] ^= s[right];
            left++;
            right--;

        } else {
            break;
        }

    }

    return s;


}
```


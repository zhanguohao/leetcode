# LeetCode: 28. 实现strStr()

[TOC]

## 1、题目描述

```
实现 strStr() 函数。

给定一个 haystack 字符串和一个 needle 字符串，在 haystack 字符串中找出 needle 字符串出现的第一个位置 (从0开始)。如果不存在，则返回  -1。

示例 1:

输入: haystack = "hello", needle = "ll"
输出: 2
示例 2:

输入: haystack = "aaaaa", needle = "bba"
输出: -1
说明:

当 needle 是空字符串时，我们应当返回什么值呢？这是一个在面试中很好的问题。

对于本题而言，当 needle 是空字符串时我们应当返回 0 。这与C语言的 strstr() 以及 Java的 indexOf() 定义相符。
```



## 2、解题思路



​	从前向后一次扫描，首先判断特殊情况

​	如果

```c
int strStr(char* haystack, char* needle) {
    // 当第一个第二个字符串的长度大于第一个的时候，返回-1
    if (strlen(haystack) < strlen(needle)) {
        return -1;
    } else if ((!*needle)) {
        return 0;
    }
    int result_pos = -1;
    int count = 0;
    char *temp = needle;
    // 判断是不是在匹配的过程中
    bool equal = false;

    // 需要注意的是，这个过程是需要回溯的
    // 例如 aaab 和 ab
    // 当前两个aa不能匹配的时候，我们需要从第二个字符开始匹配,也就是第二个a开始新的匹配
    while (haystack[count]) {
        // 判断字符是不是相等的
        if (haystack[count] == *temp) {
            if (!equal) {
                result_pos = count;
                equal = true;
            }
            // 如果相等，并且匹配的字符串已经到了结尾，也就是p匹配字符下一个是'\0',表示找到了
            if (!*(temp+1)) {
                // 将temp指向
                temp++;
                break;
            } else {
                // 如果还有字符要匹配，将temp 增加
                temp++;
            }
        } else {
            // 表示需要重新匹配
            // 需要注意，只有前面第一个字符匹配以后，才会初始化这些变量
            // 如果本来就不相等，则不需要初始化
            if (equal) {
                temp = needle;
                count = result_pos + 1;
                result_pos = -1;
                equal = false;
                continue;
            }
        }

        count++;
    }
    // 如果匹配的到最后，匹配字符还有没有匹配到的，表示不能匹配
    if (*(temp)) {
        result_pos = -1;
    }

    return result_pos;
}
```




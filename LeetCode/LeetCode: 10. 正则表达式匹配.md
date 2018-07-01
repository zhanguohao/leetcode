# LeetCode: 10. 正则表达式匹配

[TOC]

## 1、题目描述

```
给定一个字符串 (s) 和一个字符模式 (p)。实现支持 '.' 和 '*' 的正则表达式匹配。

'.' 匹配任意单个字符。
'*' 匹配零个或多个前面的元素。
匹配应该覆盖整个字符串 (s) ，而不是部分字符串。

说明:

s 可能为空，且只包含从 a-z 的小写字母。
p 可能为空，且只包含从 a-z 的小写字母，以及字符 . 和 *。
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
示例 2:

输入:
s = "aa"
p = "a*"
输出: true
解释: '*' 代表可匹配零个或多个前面的元素, 即可以匹配 'a' 。因此, 重复 'a' 一次, 字符串可变为 "aa"。
示例 3:

输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个('*')任意字符('.')。
示例 4:

输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 'c' 可以不被重复, 'a' 可以被重复一次。因此可以匹配字符串 "aab"。
示例 5:

输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```



## 2、解题思路

### 2.1 递归法

- 先判断特殊情况，如果匹配规则是0，直接根据字符串返回结果
- 如果第2个字符不是*，就判断第一个字符是不是匹配，如果是，就从下一个字符和下一个
- 如果匹配规则第二个是`*`,那么就判断这个`*`过滤了几个字符
- 最后，判断剩下的规则

```c
bool isMatch(char *s, char *p) {
    int length_str = 0;
    int length_pattern = 0;
    int i = 0;
    while (s[i++]) {
        length_str++;
    }
    i = 0;
    while (p[i++]) {
        length_pattern++;
    }

    // 如果匹配规则的长度为0，如果字符串长度为0，返回真，否则返回假；
    if (length_pattern == 0) {
        return (length_str == 0);
    }
    // 如果匹配规则只有一个字符，判断字符串是否是1，
    // 两个字符是不是相等，或者匹配字符是'.'
    if (length_pattern == 1) {
        return (length_str == 1 && (s[0] == p[0] || p[0] == '.'));
    }
    if (p[1] != '*') {
        // 如果匹配规则第一个字符不是*,表示肯定会匹配到字符，这时候如果字符长度是0，返回假
        if (length_str == 0) {
            return false;
        }
        // 这时候，判断第一个字符是否匹配，并判断后续的字符
        return (s[0] == p[0] || p[0] == '.') && isMatch(&s[1], &p[1]);
    }

    // 如果规则第二个字符是*，这时候，就要判断，字符串不为空，如果字符串为空，就直接判断空字符串与后面的规则是不是匹配
    // 还要判断第一个字符是不是匹配，如果不匹配，表示前面*将前面的过滤掉了，重新判断字符串与后面的规则的匹配情况
    // 如果第一个字符匹配，判断匹配了几个字符

    while ((s[0] != '\0') && (s[0] == p[0] || p[0] == '.')) {
        // 如果说这个*匹配了0个，那么就直接判断后面的是不是匹配
        if (isMatch(s, &p[2])) {
            return true;
        }
        // 如果*匹配了多个，判断匹配了几个
        // 将s的指针向前移动一位，表示匹配了一个
        // 这里需要判断的是，是不是将所有的s中的字符都匹配掉了，如果是的话，就要退出来
        s = &s[1];
    }

    // 这时候，知道*匹配了几个字符，判断后面的规则是否成立
    return isMatch(s, &p[2]);
}
```

​	下面是一种化简形式，基本思路是相同的

```c
    if (!*p)
        return !*s;
    if (p[1] == '*')
        return isMatch(s, p+2) || (*p == '.' && *s || *s == *p) && isMatch(s+1, p);
    return (*s == *p || (*p == '.' && *s)) && isMatch(s+1, p+1);
```

### 2.2 动态规划

​	创建一个数组，用来存放中间结果

​	例如，字符串字符数是3，匹配字符是4，那么考虑到都可能是0的情况，也就是说，有下面的对应情况



| 字符数/匹配字符数 | 0      | 1    | 2    | 3    | 4    |
| ----------------- | ------ | ---- | ---- | ---- | ---- |
| 0                 | 匹配   |      |      |      |      |
| 1                 | 不匹配 |      |      |      |      |
| 2                 | 不匹配 |      |      |      |      |
| 3                 | 不匹配 |      |      |      |      |

​	如上表，我们知道，0个字符和0个匹配字符，肯定是匹配的

​	然后，如果匹配字符的第二个字符是"*",也是会匹配的，由此得到下面的情况



| 字符数/匹配字符数 | 0      | 1    | 2    | 3    | 4    |
| ----------------- | ------ | ---- | ---- | ---- | ---- |
| 0                 | 匹配   |      | 匹配 |      | 匹配 |
| 1                 | 不匹配 |      |      |      |      |
| 2                 | 不匹配 |      |      |      |      |
| 3                 | 不匹配 |      |      |      |      |



​	第一列中，出列第一个，其他的都不会匹配，这是初始状况

​	剩下的就是不断地将匹配状况判断出来，并写入表格，直到得到最后一个，字符数是3，匹配字符数是4，这种情况是不是匹配

​	举个例子，`"abc"`和`"a*bc"` ，很明显这个是匹配的

​	下面就来计算一下

因为第二个匹配字符是*,，所以初始化的二维数组就如上面的所示

​	接下来，判断右下方的状况

​	1：1

因为这时候，需要判断的是前面的一个字符是不是匹配，也就是`dp[0][0]`, 并且当前字符是相等的，或者匹配字符是”.“



（如果是奇数个匹配字符，肯定无法匹配0个字符）

| 字符数/匹配字符数 | 0      | 1      | 2    | 3      | 4    |
| ----------------- | ------ | ------ | ---- | ------ | ---- |
| 0                 | 匹配   | 不匹配 | 匹配 | 不匹配 | 匹配 |
| 1                 | 不匹配 | 匹配   | 匹配 |        |      |
| 2                 | 不匹配 |        |      |        |      |
| 3                 | 不匹配 |        |      |        |      |

接下来判断1：2

​	这时候需要判断几种情况，只要有一种能够匹配，就代表可以匹配到

- `*`匹配了0个字符

  匹配字符的第二个字符是* ,首先判断，匹配字符向前推2个，也就是看`dp[1][0]`，明显是不匹配的

- `*` 匹配了一个字符

  - （向左看）

    表示，如果不看*， 前面的字符能不能匹配

    判断向前看一个匹配字符，也就是a，是不是和字符串的能够匹配，如果可以，表示能够匹配，明显在表格里面得到了

  - （向上看）

    如果不看前面的一个字符，当前的匹配串不是能够匹配到，如果能的话，只需要判断*前面的字符能不能匹配到



然后判断1：3

​	因为匹配字符第三个字符是b，因此，只需要判断前面的一个，也就是`dp[0][2]`, 也就是说，前面的匹配如果是成功的，那么看当前字符匹配是不是对的，如果是，那么就是匹配的



​	根据上面的规则，得到下面的矩阵：

s：abc

p：a*bc

| 字符数/匹配字符数 | 0      | 1      | 2      | 3      | 4      |
| ----------------- | ------ | ------ | ------ | ------ | ------ |
| 0                 | 匹配   | 不匹配 | 匹配   | 不匹配 | 不匹配 |
| 1                 | 不匹配 | 匹配   | 匹配   | 不匹配 | 不匹配 |
| 2                 | 不匹配 | 不匹配 | 不匹配 | 匹配   | 不匹配 |
| 3                 | 不匹配 | 不匹配 | 不匹配 | 不匹配 | 匹配   |

​	由此可见，最右下角的依赖于其他的元素进行判断，也就是动态规划的主要思想，依赖于前面的计算结果

​	不管什么样的规则，我们都能够找到这样的一个矩阵进行匹配

​	相比起递归调用的繁琐，动态规划要快

​	不过递归调用的思路要清晰很多



```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include "string.h"

bool isMatch(char *s, char *p) {

    int length_str = (int) strlen(s);
    int length_pattern = (int) strlen(p);

    bool dp[length_str + 1][length_pattern + 1];
    // 初始化边界条件，也就是第0列，第1行
    int i;
    dp[0][0] = true;
    for (i = 1; i < length_str + 1; i++) {
        dp[i][0] = false;
    }

    for (i = 1; i < length_pattern + 1; i++) {
        // 奇数个是不能匹配到0个字符的，因此为假
        if (i % 2) {
            dp[0][i] = false;
        }
            // 当为偶数个的时候，判断隔着一个字符的那个位置，能不能匹配到，如果能，就能
        else if (p[i - 1] == '*') {
            dp[0][i] = dp[0][i - 2];
        } else {
            dp[0][i] = false;
        }
    }

    // 接下来填充数组
    for (i = 1; i < length_str + 1; i++) {
        for (int j = 1; j < length_pattern + 1; ++j) {
            if (p[j - 1] == '*') {
                dp[i][j] = dp[i][j - 2] || dp[i][j - 1] || (dp[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.'));
            } else {
                dp[i][j] = dp[i - 1][j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
            }
        }
    }


    for (i = 0; i < length_str + 1; i++) {
        for (int j = 0; j < length_pattern + 1; ++j) {
            printf("%s\t", dp[i][j] ? "true" : "false");
        }
        printf("\n");
    }

    return dp[length_str][length_pattern];


}


void print(int x) {
    int num = 1;
    for (int i = 31; i >= 0; i--) {
        if (x & (num << i)) {
            printf("1");
        } else {
            printf("0");
        }
    }
    printf("\n");
}

int main() {

    printf("string: %s\t \npattern: %s\t\nresult: %s\n", "abc", "ab*c", isMatch("abc", "a*bc") ? "true" : "false");
    printf("string: %s\t \npattern: %s\t\nresult: %s\n", "aab", "c*a*b", isMatch("aab", "c*a*b") ? "true" : "false");
}

```

```
/Users/zhangguohao/CLionProjects/untitled/cmake-build-debug/untitled
true	false	true	false	false	
false	true	true	false	false	
false	false	false	true	false	
false	false	false	false	true	
string: abc	 
pattern: ab*c	
result: true
true	false	true	false	true	false	
false	false	false	true	true	false	
false	false	false	false	true	false	
false	false	false	false	false	true	
string: aab	 
pattern: c*a*b	
result: true

Process finished with exit code 0
```


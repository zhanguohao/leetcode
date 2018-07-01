# LeetCode: 844. 比较含退格的字符串

[TOC]

## 1、题目描述

```
给定 S 和 T 两个字符串，当它们分别被输入到空白的文本编辑器后，判断二者是否相等，并返回结果。 # 代表退格字符。

示例 1：

输入：S = "ab#c", T = "ad#c"
输出：true
解释：S 和 T 都会变成 “ac”。
示例 2：

输入：S = "ab##", T = "c#d#"
输出：true
解释：S 和 T 都会变成 “”。
示例 3：

输入：S = "a##c", T = "#a#c"
输出：true
解释：S 和 T 都会变成 “c”。
示例 4：

输入：S = "a#c", T = "b"
输出：false
解释：S 会变成 “c”，但 T 仍然是 “b”。
 

提示：

1 <= S.length <= 200
1 <= T.length <= 200
S 和 T 只含有小写字母以及字符 '#'。
```

## 2、 解题思路

### 2.1 从后向前递推法

​	因为字符串中仅包含退格键和小写字母，而退格键则是取消掉前面的一个字符，如果是多个退格键，则取消多个字符

​	基本思路就是，从右面向左进行比较，判断出现去掉退格键的影响以后的第一个字符是什么，如果第一个字符相同，继续判断下一个

​	直到出现不相同的或者字符串比较完毕，返回结果

```c

char findFisrtCharForBackspaceString(char *s, int *length) {
    int backspaces = 0;
    char temp = 0;
    while (!temp && *length > 0) {
        if (s[*length - 1] == '#') {
            backspaces++;
            (*length)--;
        } else if (backspaces > 0) {
            (*length)--;
            backspaces--;
        } else if (*length >=1) {
            temp = s[*length - 1];
        } else {
            break;
        }
    }
    return temp;
}

bool backspaceCompare(char *S, char *T) {
    bool flag = true;
    int length_s = strlen(S);
    int length_t = strlen(T);


    while (length_s > 0 || length_t > 0) {
        if (findFisrtCharForBackspaceString(S, &length_s) != findFisrtCharForBackspaceString(T,&length_t)){
            flag = false;
            break;
        }
        length_s--;
        length_t--;
    }

    return flag;
}
```


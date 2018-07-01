

# LeetCode: 20. 有效的括号

[TOC]



## 1、题目描述

```

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

左括号必须用相同类型的右括号闭合。
左括号必须以正确的顺序闭合。
注意空字符串可被认为是有效字符串。

示例 1:

输入: "()"
输出: true
示例 2:

输入: "()[]{}"
输出: true
示例 3:

输入: "(]"
输出: false
示例 4:

输入: "([)]"
输出: false
示例 5:

输入: "{[]}"
输出: true
```



## 2、解题思路

​	判断字符串长度，奇数肯定是错误的

​	创建一个缓冲数组，用作栈的作用，将每一个左括号，都将其入栈，右括号弹出

​	如果不能弹出，表示有问题，返回失败

​	最后判断栈指针是不是在哨兵上，如果是，则返回成功，不是则是失败

```c
bool isValid(char* s) {
        int length = strlen(s);
    // 奇数个括号肯定是不正确的
    if (length % 2) {
        return false;
    }
    if (!*s) {
        return true;
    }
    // 创建一个缓冲的数组，实现类似于栈的操作，不过最多需要字符串的一半空间
    // 多出一个元素，是考虑到后面的指针有可能越界，所以多放了一个
    char *stack = (char *) malloc(sizeof(char) * (length / 2));
    // 这个是栈的指元素，开始的时候，如果需要放入元素，就放入一个元素，并且将指针加一
    int head = -1;
    while (*s) {
        switch (*s) {
            case '[':
                if (*(s + 1)) {
                    if (*(s + 1) == ']') {
                        s += 2;
                    } else {
                        stack[++head] = *s;
                        s++;
                    }
                } else {
                    return false;
                }
                break;
            case '(':
                if (*(s + 1)) {
                    if (*(s + 1) == ')') {
                        s += 2;
                    } else {
                        stack[++head] = *s;
                        s++;
                    }
                } else {
                    return false;
                }
                break;

            case '{':
                if (*(s + 1)) {
                    if (*(s + 1) == '}') {
                        s += 2;
                    } else {
                        stack[++head] = *s;
                        s++;
                    }
                } else {
                    return false;
                }
                break;
            case ']':
                if (stack[head] == '[') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;

            case ')':
                if (stack[head] == '(') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;

            case '}':
                if (stack[head] == '{') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;
            default:
                return false;

        }
    }

    if (head < 0) {
        return true;
    }

    return false;
}
```



​	上面的判断太多了，效率不高，下面是改进版

```c
bool isValid(char* s) {
    int length = strlen(s);
    // 奇数个括号肯定是不正确的
    if (length % 2) {
        return false;
    }
    if (!*s) {
        return true;
    }
    // 创建一个缓冲的数组，实现类似于栈的操作，不过最多需要字符串的一半空间
    // 多出一个元素，是考虑到后面的指针有可能越界，所以多放了一个
    char *stack = (char *) malloc(sizeof(char) * (length / 2 + 1));
    // 这个是栈的指元素，开始的时候，如果需要放入元素，就放入一个元素，并且将指针加一
    int head = 0;
    // 哨兵
    stack[0] = 0;
    while (*s) {
        switch (*s) {
            case ']':
                if (stack[head] == '[') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;
            case ')':
                if (stack[head] == '(') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;
            case '}':
                if (stack[head] == '{') {
                    s++;
                    head--;
                } else {
                    return false;
                }
                break;
            default:
                stack[++head] = *s;
                s++;
                break;
        }
        if (head > length / 2) {
            return false;
        }
    }
    if (head <= 0) {
        return true;
    }
    return false;
}
```




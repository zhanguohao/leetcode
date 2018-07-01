# LeetCode: 58. 最后一个单词的长度

[TOC]

## 1、题目描述

```

给定一个仅包含大小写字母和空格 ' ' 的字符串，返回其最后一个单词的长度。

如果不存在最后一个单词，请返回 0 。

说明：一个单词是指由字母组成，但不包含任何空格的字符串。

示例:

输入: "Hello World"
输出: 5
```



## 2、解题思路

​	从后往前，先找到第一个非空格的字符的位置，然后从这个位置开始向前找

```c
#include <stdbool.h>
#include <stdio.h>
#include <stdlib.h>
#include <math.h>
#include <memory.h>
#include <string.h>


int lengthOfLastWord(char *s) {
    int result = 0;
    int pos = strlen(s) - 1;
    // 找到最后一个非空格的字符位置
    while (s[pos] == ' ' && pos > 0) {
        pos--;
    }

    while (pos >= 0) {
        if (s[pos] == ' ' || pos < 0) {
            break;

        } else if (s[pos] != ' ') {
            result++;
            pos--;
        }

    }
    return result;
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

    printf("%d\n", lengthOfLastWord("a   "));


}

```

​	

​	略做改进：

```c
int lengthOfLastWord(char* s) {
     int result = 0;
    int pos = strlen(s) - 1;
    bool start_find = false;
    // 找到最后一个非空格的字符位置

    while (pos >= 0) {
        if(!start_find){
            if(s[pos] == ' ' && pos > 0){
                pos--;
            } else{
                start_find = true;
            }
        }else{

            if (s[pos] == ' ' || pos < 0) {
                break;

            } else if (s[pos] != ' ') {
                result++;
                pos--;
            }
        }

    }
    return result;
}
```


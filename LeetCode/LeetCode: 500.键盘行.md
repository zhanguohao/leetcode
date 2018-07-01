# LeetCode: 500.键盘行

[TOC]

## 1、题目描述

给定一个单词列表，只返回可以使用在键盘同一行的字母打印出来的单词。键盘如下图所示。

 

![American keyboard](https://leetcode-cn.com/static/images/problemset/keyboard.png)

 

**示例1:**

```
输入: ["Hello", "Alaska", "Dad", "Peace"]
输出: ["Alaska", "Dad"]
```

**注意:**

1. 你可以重复使用键盘上同一字符。
2. 你可以假设输入的字符串将只包含字母。



## 2、解题思路

​	直接建立3个缓冲数组，存放q这一行，a这一行以及z这一行的值的索引，每一次判断一下



```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
char** findWords(char** words, int wordsSize, int* returnSize) {
    
    char *q = "qwertyuiopQWERTYUIOP";
    char *a = "asdfghjklASDFGHJKL";
    char *z = "zxcvbnmZXCVBNM";


    int *index_q = (int *) calloc(('z' - 'A' + 1), sizeof(int));
    int *index_a = (int *) calloc(('z' - 'A' + 1), sizeof(int));
    int *index_z = (int *) calloc(('z' - 'A' + 1), sizeof(int));

    while (*q || *a || *z) {
        if (*q) {
            index_q[*q - 'A']++;
            q++;
        }

        if (*a) {
            index_a[*a - 'A']++;
            a++;
        }
        if (*z) {
            index_z[*z - 'A']++;
            z++;
        }
    }


    char **result = (char **) malloc(sizeof(char *) * wordsSize);
    int result_pos = 0;
    char *temp;
    char *current_word;
    bool isWord = true;
    int current_line = 0;
    for (int i = 0; i < wordsSize; i++) {
        current_word = temp = words[i];
        isWord = true;
        current_line = 0;
        while (*temp) {

            if (current_line == 0) {
                if (index_q[*temp - 'A'] != 0) {
                    current_line = 1;
                } else if (index_a[*temp - 'A'] != 0) {
                    current_line = 2;
                } else if (index_z[*temp - 'A'] != 0) {
                    current_line = 3;
                } else {
                    isWord = false;
                    break;
                }

            } else {
                if (current_line == 1) {
                    if (index_q[*temp - 'A'] == 0) {
                        isWord = false;
                        break;
                    }
                } else if (current_line == 2) {
                    if (index_a[*temp - 'A'] == 0) {
                        isWord = false;
                        break;
                    }
                } else {
                    if (index_z[*temp - 'A'] == 0) {
                        isWord = false;
                        break;
                    }
                }
            }
            temp++;


        }

        if (isWord) {
            result[result_pos++] = current_word;
        }

    }

    *returnSize = result_pos;
    // result = realloc(result, result_pos);

    return result;
    
}
```

​	注意，在提交的时候，realloc出现问题了




# LeetCode: 520. 检测大写字母

[TOC]



## 1、题目描述

给定一个单词，你需要判断单词的大写使用是否正确。

我们定义，在以下情况时，单词的大写用法是正确的：

1. 全部字母都是大写，比如"USA"。
2. 单词中所有字母都不是大写，比如"leetcode"。
3. 如果单词不只含有一个字母，只有首字母大写， 比如 "Google"。

否则，我们定义这个单词没有正确使用大写字母。

**示例 1:**

```
输入: "USA"
输出: True
```

**示例 2:**

```
输入: "FlaG"
输出: False
```

**注意:** 输入是由大写和小写拉丁字母组成的非空单词。



## 2、解题思路



```c
bool detectCapitalUse(char* word) {
    
    if (!word) {
        return true;
    }
    bool result = true;
    /*
     * type:
     *  1：首字母大写
     *  2：全部大写
     *  3：全部小写
     */
    int type = 0;
    int length = strlen(word);
    while (length-- > 0) {
        if (type == 0) {
            if (*word >= 'A' && *word <= 'Z') {
                type = 1;
                if ((word + 1) && *(word + 1) >= 'A' && *(word + 1) <= 'Z') {
                    type = 2;
                }
                length--;
                word++;
            } else {
                type = 3;
            }
        } else {

            if (type == 1 || type == 3) {
                if (*word < 'a') {
                    result = false;
                    break;
                }
            } else if (type == 2) {
                if (*word > 'Z') {
                    result = false;
                    break;
                }
            }


        }

        word++;
    }

    return result;
}
```






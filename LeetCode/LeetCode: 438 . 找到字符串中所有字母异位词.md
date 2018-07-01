# LeetCode: 438 . 找到字符串中所有字母异位词

[TOC]



## 1、题目描述



给定一个字符串 **s** 和一个非空字符串 **p**，找到 **s** 中所有是 **p** 的字母异位词的子串，返回这些子串的起始索引。

字符串只包含小写英文字母，并且字符串 **s** 和 **p** 的长度都不超过 20100。

**说明：**

- 字母异位词指字母相同，但排列不同的字符串。
- 不考虑答案输出的顺序。

**示例 1:**

```
输入:
s: "cbaebabacd" p: "abc"

输出:
[0, 6]

解释:
起始索引等于 0 的子串是 "cba", 它是 "abc" 的字母异位词。
起始索引等于 6 的子串是 "bac", 它是 "abc" 的字母异位词。
```

 **示例 2:**

```
输入:
s: "abab" p: "ab"

输出:
[0, 1, 2]

解释:
起始索引等于 0 的子串是 "ab", 它是 "ab" 的字母异位词。
起始索引等于 1 的子串是 "ba", 它是 "ab" 的字母异位词。
起始索引等于 2 的子串是 "ab", 它是 "ab" 的字母异位词。
```





## 2、解题思路



```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
int* findAnagrams(char* s, char* p, int* returnSize) {
    
    int length_s = strlen(s);
    int length_p = strlen(p);

    if (!p || length_p > length_s) {
        return NULL;
    }

    int *result = (int *) calloc(length_s - length_p + 1, sizeof(int));

    int *index_p = (int *) calloc(26, sizeof(int));
    int *temp = (int *) calloc(26, sizeof(int));


    for (int i = 0; i < length_p; i++) {
        index_p[p[i] - 'a']++;
    }

    int pos = 0;
    bool equal = true;
    for (int i = 0; i < (length_s - length_p + 1); i++) {

        for (int j = 0; j < length_p; j++) {
            temp[s[i + j] - 'a']++;
        }

        equal = true;


        for (int k = 0; k < 26; k++) {
            if (temp[k] != index_p[k]) {
                equal = false;
                break;
            }
        }

        // temp初始化
        for (int k = 0; k < 26; k++) {
            temp[k] = 0;
        }

        if (equal) {
            result[pos++] = i;
        }
    }

    *returnSize = pos;

    return result;
    

}
```


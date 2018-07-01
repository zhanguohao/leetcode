# LeetCode: 242. 有效的字母异位词

[TOC]



## 1、题目描述

给定两个字符串 *s* 和 *t* ，编写一个函数来判断 *t* 是否是 *s* 的一个字母异位词。

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

**说明:**
你可以假设字符串只包含小写字母。

**进阶:**
如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？



## 2、解题思路

​	实际上，这个题目是要找出两个字符串，她们有相同的字母构成，但是顺序可能不同，通过异或来做，十分简单

​	将第一个字符串所有字母进行异或，

​	将第二个字符串所有字母进行异或，

​	如果两个异或值相等，表示相等，不相等返回false

​	

​	这个思路有一点问题，就是aa，bb这样的也会认为是相等的

​	异或的话，目前还没有好的解决办法



### 2.1 排序法

​	排序后比较

```c
int cmp(const void *a, const void *b) {
    return *(char *) a - *(char *) b;
}

bool isAnagram(char *s, char *t) {
    if (!s && !t) {
        return true;
    }


    if (strlen(s) != strlen(t)) {
        return false;
    }

    int length = strlen(s);

    char *s1 = (char *) malloc(sizeof(char) * strlen(s) + 1);
    char *t1 = (char *) malloc(sizeof(char) * strlen(t) + 1);

    strcpy(s1, s);
    strcpy(t1, t);

    qsort(s1, length, sizeof(char), cmp);
    qsort(t1, length, sizeof(char), cmp);

    for (int i = 0; i < length; i++) {
        if (s1[i] != t1[i]) {
            free(t1);
            free(s1);
            return false;
        }
    }
    free(t1);
    free(s1);

    return true;


}
```





### 2.2 计数法



```c
bool isAnagram(char *s, char *t) {
    
     if (!s && !t) {
        return true;
    }


    if (strlen(s) != strlen(t)) {
        return false;
    }

    int buf[26] = {0};

    int i = 0;
    while (s[i]) {
        buf[s[i++] - 'a']++;

    }

    i = 0;
    while (t[i]) {
        buf[t[i++] - 'a']--;
    }

    for (i = 0; i < 26; i++) {
        if (buf[i] != 0) {
            return false;
        }
    }

    return true;
}
```


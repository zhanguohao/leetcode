# LeetCode: 344. 反转字符串

[TOC]

## 1、题目描述



请编写一个函数，其功能是将输入的字符串反转过来。

**示例：**

```
输入：s = "hello"
返回："olleh"
```



## 2、解题思路





```c
char* reverseString(char* s) {
    
    int length = strlen(s);

    if (length <= 1) {
        return s;
    }

    char * temp = (char*)malloc(sizeof(char)*length+1);
    strcpy(temp,s);

    for (int i = 0; i < length / 2; i++) {
        temp[i] ^= temp[length-1-i];
        temp[length-1-i] ^= temp[i];
        temp[i] ^= temp[length-1-i];
    }

    return temp;
   
}
```


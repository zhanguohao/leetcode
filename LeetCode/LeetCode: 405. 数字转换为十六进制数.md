# LeetCode: 405. 数字转换为十六进制数

[TOC]



## 1、题目描述





给定一个整数，编写一个算法将这个数转换为十六进制数。对于负整数，我们通常使用 [补码运算](https://baike.baidu.com/item/%E8%A1%A5%E7%A0%81/6854613?fr=aladdin) 方法。

**注意:**

1. 十六进制中所有字母(`a-f`)都必须是小写。
2. 十六进制字符串中不能包含多余的前导零。如果要转化的数为0，那么以单个字符`'0'`来表示；对于其他情况，十六进制字符串中的第一个字符将不会是0字符。 
3. 给定的数确保在32位有符号整数范围内。
4. **不能使用任何由库提供的将数字直接转换或格式化为十六进制的方法。**

**示例 1：**

```
输入:
26

输出:
"1a"
```

**示例 2：**

```
输入:
-1

输出:
"ffffffff"
```





## 2、解题思路

```c
char* toHex(int num) {
    
    char *result = (char *) malloc(sizeof(char) * 8+1);
    result[8] = '\0';
    char *buf = "0123456789abcdef";
    int num_pos = 0;
    for (int i = 0; i < 8; i++) {
        num_pos = 0;
        for (int j = 0; j < 4; j++) {
            if (num & (1 << (j + (i * 4)))) {
                num_pos += pow(2, j);
            }
        }
        result[8 - i - 1] = buf[num_pos];
    }

    char *temp = result;
    int zero_num = 0;
    while (*temp == '0') {
        zero_num++;
        temp++;
    }
    if (zero_num == 8) {
        zero_num = 7;
    }
    char *re = (char *) malloc(sizeof(char) * (8 - zero_num + 1));
    re[0] = 0;
    strcpy(re, &result[zero_num]);
    free(result);
    

    return re;
}
```


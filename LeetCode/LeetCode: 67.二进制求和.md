# LeetCode: 67.二进制求和

[TOC]





## 1、题目描述

给定两个二进制字符串，返回他们的和（用二进制表示）。

输入为**非空**字符串且只包含数字 `1` 和 `0`。

**示例 1:**

```
输入: a = "11", b = "1"
输出: "100"
```

**示例 2:**

```
输入: a = "1010", b = "1011"
输出: "10101"
```



## 2、解题思路

​	两个二进制相加，要么进位1，要不不进位，进位的可能性比较大，

​	首先创建一个结果数组，考虑到进位，因此数组长度比起两个二进制数要大一

​	从后向前判断



```c
char* addBinary(char* a, char* b) {
    int length_a = strlen(a);
    int length_b = strlen(b);

    int result_length = length_a > length_b ? length_a : length_b;
    // 默认认为有一个进位
    result_length++;

    int carry = 0;
    char *result = (char *) malloc(sizeof(char) * result_length + 1);
    result[result_length] = '\0';
    for (int i = 0; i < result_length - 1; i++) {
        result[result_length - 1 - i] = '0';
        if (length_a - 1 - i >= 0) {
            result[result_length - 1 - i] = a[length_a - 1 - i];
        }
        if (length_b - 1 - i >= 0) {
            if (carry == 0) {

                if (b[length_b - 1 - i] == result[result_length - 1 - i]) {
                    if (result[result_length - 1 - i] == '1') {
                        carry = 1;
                        result[result_length - 1 - i] = '0';
                    } else {
                        carry = 0;
                        result[result_length - 1 - i] = '0';

                    }
                } else {
                    result[result_length - 1 - i] = '1';
                }

            } else {
                if (b[length_b - 1 - i] == result[result_length - 1 - i]) {
                    if (result[result_length - 1 - i] == '1') {
                        carry = 1;
                        result[result_length - 1 - i] = '1';
                    } else {
                        carry = 0;
                        result[result_length - 1 - i] = '1';

                    }
                } else {
                    carry = 1;
                    result[result_length - 1 - i] = '0';
                }

            }
        } else {
            if (carry == 1) {
                if (result[result_length - 1 - i] == '1') {
                    result[result_length - 1 - i] = '0';
                    carry = 1;
                } else {
                    carry = 0;
                    result[result_length - 1 - i] = '1';
                }
            }

        }


    }

    if (carry == 1 && result_length > length_a && result_length > length_b) {
        result[0] = '1';
    } else {
        result = &result[1];
    }

    return  result;
}
```


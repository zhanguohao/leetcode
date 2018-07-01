# LeetCode: 415. 字符串相加

[TOC]

## 1、题目描述



给定两个字符串形式的非负整数 `num1` 和`num2` ，计算它们的和。

**注意：**

1. `num1` 和`num2` 的长度都小于 5100.
2. `num1` 和`num2` 都只包含数字 `0-9`.
3. `num1` 和`num2` 都不包含任何前导零。
4. **你不能使用任何內建 BigInteger 库， 也不能直接将输入的字符串转换为整数形式。**





## 2、解题思路

​	

​	与前面的一样，一位一位的判断，本位和以及进位



​	

```c
char* addStrings(char* num1, char* num2) {
    
    int length1 = strlen(num1);
    int length2 = strlen(num2);

    int max = length1 > length2 ? length1 : length2;
    int min = length1 > length2 ? length2 : length1;

    char *result = (char *) calloc(max + 2, sizeof(char));
    bool carry = false;

    for (int i = 0; i < max; i++) {
        if (i < min) {

            result[max - i] = ((num1[length1 - 1 - i] - '0') + (num2[length2 - 1 - i] - '0') + carry) % 10 + '0';
            carry = num1[length1 - 1 - i] + num2[length2 - 1 - i] + carry >= 106 ? true : false;

        } else {

            if (length1 < length2) {

                result[max - i] = ((num2[length2 - 1 - i] - '0') + carry) % 10 + '0';
                carry = num2[length2 - 1 - i] + carry > 57 ? true : false;
            } else {
                result[max - i] = ((num1[length1 - 1 - i] - '0') + carry) % 10 + '0';
                carry = num1[length1 - 1 - i] + carry > 57 ? true : false;

            }
        }
    }

    if (carry) {
        result[0] = '1';
    } else {
        char *re = (char *) calloc(max + 1, sizeof(char));
        strcpy(re, &result[1]);
        free(result);
        result = re;

    }
    return result;
}

```


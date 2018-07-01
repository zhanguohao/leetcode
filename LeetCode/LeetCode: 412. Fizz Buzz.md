# LeetCode: 412. Fizz Buzz

[TOC]



## 1、题目描述



写一个程序，输出从 1 到 *n* 数字的字符串表示。

\1. 如果 *n* 是3的倍数，输出“Fizz”；

\2. 如果 *n* 是5的倍数，输出“Buzz”；

3.如果 *n* 同时是3和5的倍数，输出 “FizzBuzz”。

**示例：**

```
n = 15,

返回:
[
    "1",
    "2",
    "Fizz",
    "4",
    "Buzz",
    "Fizz",
    "7",
    "8",
    "Fizz",
    "Buzz",
    "11",
    "Fizz",
    "13",
    "14",
    "FizzBuzz"
]
```





## 2、解题思路



​	基本上就是判断是不是被3整除，被5整除，都能整除，然后将该数字转换成字符串

​	如果想进一步提高效率，就将转换字符串的这一步确定一下，直接选择它的位数，不过会浪费空间；

```c
char *numToString(int num) {
    int digits = 1;

    int temp = num;
    while (temp / 10) {
        digits++;
        temp /= 10;
    }

    char *result = (char *) calloc(digits + 1, sizeof(char));

    sprintf(result, "%d", num);

    return result;

}


/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */
char **fizzBuzz(int n, int *returnSize) {
    char **result = (char **) calloc(n, sizeof(char *));

    char *divThree = "Fizz";
    char *divFive = "Buzz";
    char *divThreeAndFive = "FizzBuzz";


    for (int i = 1; i <= n; i++) {
        if (i % 3 == 0 && i % 5 == 0) {
            result[i - 1] = divThreeAndFive;
        } else if (i % 3 == 0) {
            result[i - 1] = divThree;
        } else if (i % 5 == 0) {
            result[i - 1] = divFive;
        } else {
            result[i - 1] = numToString(i);
        }
    }
    *returnSize = n;

    return result;

}
```


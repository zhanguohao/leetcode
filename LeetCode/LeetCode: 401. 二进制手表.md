# LeetCode: 401. 二进制手表

[TOC]



## 1、题目描述



二进制手表顶部有 4 个 LED 代表**小时（0-11）**，底部的 6 个 LED 代表**分钟（0-59）**。

每个 LED 代表一个 0 或 1，最低位在右侧。

![img](https://upload.wikimedia.org/wikipedia/commons/8/8b/Binary_clock_samui_moon.jpg)

例如，上面的二进制手表读取 “3:25”。

给定一个非负整数 *n* 代表当前 LED 亮着的数量，返回所有可能的时间。

**案例:**

```
输入: n = 1
返回: ["1:00", "2:00", "4:00", "8:00", "0:01", "0:02", "0:04", "0:08", "0:16", "0:32"]
```

 

**注意事项:**

- 输出的顺序没有要求。
- 小时不会以零开头，比如 “01:00” 是不允许的，应为 “1:00”。
- 分钟必须由两位数组成，可能会以零开头，比如 “10:2” 是无效的，应为 “10:02”。



## 2、解题思路

### 2.1 递归法

​	分成两部分

-  创建一个10个位置的数组，表示那几个灯亮
- 然后进行全部组合的遍历，利用递归
- 将满足条件的添加到结果中去



实际上，最多是亮8个灯，上面3个，下面5个



初始状态下，如果有3个灯，那么最多是$C(^3_{10})$ 这些，然后将其中不满足条件的删掉，所以初始的二维指针数组大小可以设置为这个数

```c
#include <stdio.h>
#include <stdbool.h>
#include <stdlib.h>
#include <memory.h>
#include "math.h"


/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */


int pos = 0;

// 将时间编程字符串，并且判断是不是满足要求的
char *timeToString(int *time) {

    int hour = 0;
    for (int i = 0; i < 4; i++) {
        if (time[i] == 1) {
            hour += (int) pow(2, 3 - i);
        }
    }

    if (hour >= 12) {
        return NULL;
    }

    int minite = 0;
    for (int i = 4; i < 10; i++) {
        if (time[i] == 1) {
            minite += (int) pow(2, 9 - i);
        }
    }
    if (minite > 59) {
        return NULL;
    }

    int digits = hour / 10 + 4;
    char *t = (char *) malloc(sizeof(char) * (digits + 1));
    t[digits] = '\0';
    t[digits - 3] = ':';

    if (hour / 10 >= 1) {
        t[0] = '1';
        t[1] = '0' + hour % 10;
    } else {
        t[0] = '0' + hour;
    }


    t[digits - 2] = '0' + minite / 10;
    t[digits - 1] = '0' + minite % 10;


    return t;


}

// 求解组合数
int combination(int c, int u) {
    if (u == 0) {
        return 1;
    }

    int result = 1;
    int down = c - u > u ? u : c - u;

    int down_result = 1;
    for (int i = 0; i < down; i++) {
        result *= c - i;
        down_result *= i + 1;
    }


    return result / down_result;


}

// 递归求界所有的组合，并且转换成字符串，加入结果中
void dfs(char **result, int timePos, int oneNums, int num, int *timeCode) {
    if (oneNums == num) {
        char *time = timeToString(timeCode);
        if (time!=NULL) {
            result[pos++] = time;
        }
        return;
    }

    if (oneNums > num || timePos == 10) {
        return;
    }
    timeCode[timePos] = 1;
    dfs(result, timePos + 1, oneNums + 1, num, timeCode);
    timeCode[timePos] = 0;
    dfs(result, timePos + 1, oneNums, num, timeCode);

}


char **readBinaryWatch(int num, int *returnSize) {
    char **result = (char **) malloc(sizeof(char *) * combination(10, num));
    int *timeCode = (int *) calloc(10, sizeof(int));

    dfs(result, 0, 0, num, timeCode);
    *returnSize = pos;
    char **r = (char **) malloc(sizeof(char *) * pos);

    for (int i = 0; i < pos; i++) {
        r[i] = result[i];
    }

    free(result);
    return r;

}


int main() {

//    printf("%d\n", combination(10, 1));
    int size = 0;
//    char **r = readBinaryWatch(1, &size);
    char **r = readBinaryWatch(2, &size);
    printf("%d\n", size);
    printf("===================\n");
    for (int i = 0; i < size; i++) {
        printf("%s\n", r[i]);
    }


//    int *timeCode = (int *) calloc(10, sizeof(int));
//    timeCode[0] = 1;
//    timeCode[2] = 1;
//    printf("%s\n",timeToString(timeCode));




}
```



​	上面代码本地跑起来没问题，leetCode上面run的时候没事，提交出错，有点困惑



### 2.2 循环法



​	实际上使用递归是很慢的，从另一个角度考虑，只需要判断一下数字中1的数量，并且相加，如果等于指定的1的数量，就转换这个数值，并且保存

```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */


// 将时间编程字符串，并且判断是不是满足要求的
char *timeToString(int time) {

    int hour = 0;
    for (int i = 0; i < 4; i++) {
        if (time & (1 << (9 - i))) {
            hour += (int) pow(2, 3 - i);
        }
    }


    int minute = 0;
    for (int i = 4; i < 10; i++) {
        if (time & (1 << (9 - i))) {
            minute += (int) pow(2, 9 - i);
        }
    }

    int digits = hour / 10 + 4;
    char *t = (char *) malloc(sizeof(char) * (digits + 1));
    t[digits] = '\0';
    t[digits - 3] = ':';

    if (hour / 10 >= 1) {
        t[0] = '1';
        t[1] = '0' + hour % 10;
    } else {
        t[0] = '0' + hour;
    }


    t[digits - 2] = '0' + minute / 10;
    t[digits - 1] = '0' + minute % 10;


    return t;


}

// 求解组合数
int combination(int c, int u) {
    if (u == 0) {
        return 1;
    }

    int result = 1;
    int down = c - u > u ? u : c - u;

    int down_result = 1;
    for (int i = 0; i < down; i++) {
        result *= c - i;
        down_result *= i + 1;
    }


    return result / down_result;


}

// 统计二进制位
int binaryBits(int num) {
    int result = 0;
    for (int i = 0; i < 32; i++) {
        if (num & 1) {
            result++;
        }
        num >>= 1;
        if (num <= 0) {
            break;
        }
    }
    return result;
}


char **readBinaryWatch(int num, int *returnSize) {
    char **result = (char **) malloc(sizeof(char *) * combination(10, num));
    int pos = 0;
    for (int i = 0; i < 12; i++) {
        for (int j = 0; j < 60; j++) {
            if (binaryBits(i) + binaryBits(j) == num) {
                result[pos++] = timeToString((i << 6) | j);

            }
        }
    }

    *returnSize = pos;

    return result;

}



```



​	
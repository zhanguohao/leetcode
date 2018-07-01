# LeetCode: 746. 使用最小花费爬楼梯

[TOC]



## 1、题目描述





数组的每个索引做为一个阶梯，第 `i`个阶梯对应着一个非负数的体力花费值 `cost[i]`(索引从0开始)。

每当你爬上一个阶梯你都要花费对应的体力花费值，然后你可以选择继续爬一个阶梯或者爬两个阶梯。

您需要找到达到楼层顶部的最低花费。在开始时，你可以选择从索引为 0 或 1 的元素作为初始阶梯。

**示例 1:**

```
输入: cost = [10, 15, 20]
输出: 15
解释: 最低花费是从cost[1]开始，然后走两步即可到阶梯顶，一共花费15。
```

 **示例 2:**

```
输入: cost = [1, 100, 1, 1, 1, 100, 1, 1, 100, 1]
输出: 6
解释: 最低花费方式是从cost[0]开始，逐个经过那些1，跳过cost[3]，一共花费6。
```

**注意：**

1. `cost` 的长度将会在 `[2, 1000]`。
2. 每一个 `cost[i]` 将会是一个Integer类型，范围为 `[0, 999]`。



## 2、解题思路

​	实际上就是简单的动态规划问题，可以用一个缓冲数组，也可以不用

```c
#define min_Value(a, b) a<b?a:b

int minCostClimbingStairs(int *cost, int costSize) {
    int buff[costSize + 1];
    buff[0] = 0;
    buff[1] = cost[0];

    int temp;
    for (int i = 1; i < costSize; i++) {

        temp = min_Value(buff[i], buff[i - 1]);
        temp += cost[i];
        buff[i + 1] = temp;
        // printf("%d\n", buff[i + 1]);
    }

    return min_Value(buff[costSize], buff[costSize - 1]);

}

```



​	键值遇到奇葩了，不知道为什么，不用temp总是出错，晕了！！！！
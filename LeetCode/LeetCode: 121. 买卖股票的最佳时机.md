# LeetCode: 121. 买卖股票的最佳时机

[TOC]





## 1、题目描述

给定一个数组，它的第 *i* 个元素是一支给定股票第 *i* 天的价格。

如果你最多只允许完成一笔交易（即买入和卖出一支股票），设计一个算法来计算你所能获取的最大利润。

注意你不能在买入股票前卖出股票。

**示例 1:**

```
输入: [7,1,5,3,6,4]
输出: 5
解释: 在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格。
```

**示例 2:**

```
输入: [7,6,4,3,1]
输出: 0
解释: 在这种情况下, 没有交易完成, 所以最大利润为 0。
```



## 2、解题思路

### 2.1 迭代法

​	一开始的想法，先将所有的右面有大于它的数的下标找出来，然后分别将每个数字对应的最大间隔找出来，进行比较

​	有点浪费时间

```c
int maxProfit(int* prices, int pricesSize) {
   if (!prices || pricesSize <= 1) {
        return 0;
    }

    // 首先找到几个下标，在右面有比当前数大的数字
    bool right_bigger = false;
    int *buf = (int *) malloc(sizeof(int) * pricesSize - 1);
    int buf_size = 0;
    for (int i = 0; i < pricesSize - 1; i++) {
        for (int j = i + 1; j < pricesSize; j++) {
            if (prices[i] < prices[j]) {
                buf[buf_size++] = i;
                break;
            }
        }
    }

    if (buf_size <= 0) {
        return 0;
    }

    int result = 0;
    //int min_pos = buf[0];
    for (int i = 0; i < buf_size; i++) {
        for (int j = buf[i] + 1; j < pricesSize; j++) {
            if (prices[j] - prices[buf[i]] > result) {
                result = prices[j] - prices[buf[i]];
            }
        }

    }


    return result;
}
```



### 2.2 递归法

​	递归法则是从当前值开始判断，找到大于当前值的最大的那个数，得到差值

​	然后从下一个数判判断，得到最大的那个差值，进行判断

​	效率也不是很高

```c
int maxProfit(int* prices, int pricesSize) {
   if (pricesSize <= 1) {
        return 0;
    }

    int result = 0;
    int temp = 0;
    for (int i = 0; i < pricesSize; i++) {

        if (result < prices[i] - prices[0]) {
            result = prices[i] - prices[0];
        }
    }
    temp = maxProfit(&prices[1], pricesSize - 1);
    if (result < temp) {
        result = temp;
    }

    return result;
}
```

### 2.3 前向扫描法

​	前两种办法效率不高，主要是考虑的不到位，

​	实际上，我们可以这样考虑，考虑在数组中有一个最小值，每一次都是当前值和最小值之间的比较

例如，4 3 6 3 7

​	一开始，最小值是1

​	然后发现3比4小，更新最小值，然后继续向下判断

​	如果这个值大于最小值，就算出差值

​	然后继续向下判断，如果如果还是大于最小值，就算出差值，与之前的进行比较

```c
int maxProfit(int* prices, int pricesSize) {
   if (pricesSize <= 1) {
        return 0;
    }

    int min_pos = 0;
    int result = 0;
    for (int i = 1; i < pricesSize; i++) {
        if (prices[i] > prices[min_pos]) {
            result = result > prices[i] - prices[min_pos] ? result : prices[i] - prices[min_pos];
        } else {
            min_pos = i;
        }
    }

    return result;
}
```

```c
int maxProfit(int *prices, int pricesSize) {
    if (pricesSize <= 1) {
        return 0;
    }

    int min_pos = 0;
    int result = 0;
    for (int i = 1; i < pricesSize; i++) {
        min_pos = prices[i] > prices[min_pos] ? min_pos : i;
        result = result > prices[i] - prices[min_pos] ? result : prices[i] - prices[min_pos];
    }

    return result;


}
```


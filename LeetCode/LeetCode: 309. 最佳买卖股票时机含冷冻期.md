# LeetCode: 309. 最佳买卖股票时机含冷冻期

[TOC]



## 1、题目描述

​	给定一个整数数组，其中第 *i* 个元素代表了第 *i* 天的股票价格 。

设计一个算法计算出最大利润。在满足以下约束条件下，你可以尽可能地完成更多的交易（多次买卖一支股票）:

- 你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。
- 卖出股票后，你无法在第二天买入股票 (即冷冻期为 1 天)。

**示例:**

```
输入: [1,2,3,0,2]
输出: 3 
解释: 对应的交易状态为: [买入, 卖出, 冷冻期, 买入, 卖出]
```



## 2、解题思路

​	这道题虽然知道是用动态规划，不过状态转换方程还是不太好想

首先，状态分成两类，每种状态根据可能的条件，又有几种状态

- 持有股票
  - 卖掉股票
  - 继续持有
- 未持有股票
  - 买股票（过了冷冻期）
  - 继续未持有

由此，我们得到两个状态转换方程

```
sdp[i] = max( bdp[i-1] + price , sdp[i-1] )
bdp[i] = max( sdp[i-2] - price , bdp[i-1] )
```

```python
class Solution:
    def maxProfit(self, prices):
        """
        :type prices: List[int]
        :rtype: int
        """
        length = len(prices)
        if length <= 1:
            return 0

        sdp = [0] * length
        bdp = [0] * length

        bdp[0] = -prices[0]

        for i in range(1, length):
            sdp[i] = max(bdp[i - 1] + prices[i], sdp[i - 1])
            if i >= 2:
                bdp[i] = max(sdp[i - 2] - prices[i], bdp[i - 1])

            else:
                bdp[i] = max(-prices[i], bdp[i - 1])

        return sdp[-1]
```






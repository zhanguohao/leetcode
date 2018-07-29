# LeetCode: 322. 零钱兑换

[TOC]



## 1、题目描述



给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 `-1`。

**示例 1:**

```
输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
```

**示例 2:**

```
输入: coins = [2], amount = 3
输出: -1
```

**说明**:
你可以认为每种硬币的数量是无限的。



## 2、解题思路

​	这道题可以使用动态规划来做

dp[i] = min(dp[i-1],dp[i-2],dp[i-5])+1

​	虽然这样可以做，但是会超出时间限制。。。

换个思路，我们直接通过已经有的硬币数，直接生成对应钱数最小的硬币数

- 建立一个dp数组
- 将硬币对应的下标设置为1
- 然后从头到尾开始计算，遇到一个不是-1的数，就用这个数加上硬币，对应的下标设置为当前硬币数加1
- 最后返回最后一个



举个例子

```
coins = [1, 2, 5], amount = 11
dp = [-1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 -1 ]
然后，对应的，1，2，5的位置，这是为1
dp = [-1 1 1 -1 -1 1 -1 -1 -1 -1 -1 -1 -1 ]
然后从前向后进行扫描，
第一个不是-1的数，下标为2，然后 判断
2+1 = 3，下标为3的如果不为是-1，就更新为2
以此类推
```



```python
class Solution:
    def coinChange(self, coins, amount):
        """
        :type coins: List[int]
        :type amount: int
        :rtype: int
        """
        buff = [amount + 1] * (amount + 1)
        buff[0] = 0
        for i in range(1, amount + 1):
            for j in coins:
                if i >= j:
                    buff[i] = min(buff[i], buff[i - j] + 1)

        if buff[-1] == amount + 1:
            return -1

        return buff[-1]
        
```

​	好几个版本，一直超出时间限制

​	

​	好伤心，还是同样的思路，直接用c语言重新写了一遍，就通过了

```c
int coinChange(int* coins, int coinsSize, int amount) {
    int *buff = (int *) malloc(sizeof(int) * (amount + 1));
    for (int i = 1; i <= amount; i++) {
        buff[i] = amount + 1;
    }
    buff[0] = 0;
    for (int i = 1; i < amount + 1; i++) {
        for (int j = 0; j < coinsSize; j++) {
            if (i >= coins[j]) {
                buff[i] = buff[i] > (buff[i - coins[j]] + 1) ? (buff[i - coins[j]] + 1) : buff[i];
            }
        }
    }
    if (buff[amount] == (amount + 1)) {
        return -1;
    }

    return buff[amount];
}
```
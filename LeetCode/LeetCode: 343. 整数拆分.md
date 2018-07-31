# LeetCode: 343. 整数拆分

[TOC]

## 1、题目描述



给定一个正整数 *n*，将其拆分为**至少**两个正整数的和，并使这些整数的乘积最大化。 返回你可以获得的最大乘积。

例如，给定 *n* = 2，返回1（2 = 1 + 1）；给定 *n* = 10，返回36（10 = 3 + 3 + 4）。

注意：你可以假设 *n* 不小于2且不大于58。

**感谢：**
特别感谢 [@jianchao.li.fighter](https://leetcode.com/discuss/user/jianchao.li.fighter) 添加此问题并创建所有测试用例。



## 2、解题思路

我们来分析一下思路

```
2:	1
3:	2
4:	4
5:	6
6:	9
7:	12
8:	16
9:	24
10:	36
```

那么，当前最大的值应该是多少呢？

```
dp[i] = max(dp[1]*(n-1),dp[2]*(n-2),...,dp[n-1]*1,1*(n-1),2*(n-2),...,(n-1)*1)
```

从上面来看，也就是说，判断需要从两个角度来看：

- 前面求解的最优值与差值求和

  - 在这里面，我们发现，实际上并不需要每一项都判断，

- 直接两个数相乘

  

  实际上，从3往后，我们发现乘积之和都是大于它本身的，如果是两个数直接相乘，

- 如果是偶数，就是(n/2)*(n/2)

- 如果是奇数，就是(n/2)*(n/2+1)

  上面的情况肯定是直接相乘最大的



然后，2，3是比较特殊的，因为他们的乘积是小于本身的，因此，要考虑这两种情况`dp[n-2]*2, dp[n-3]*3`

```python
class Solution:
    def integerBreak(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp = [0, 1, 1, 2, 4, 6, 9, 12, 18]

        length = len(dp)
        if n < length:
            return dp[n]

        dp.extend([0] * (n - length + 1))

        for i in range(length, n + 1):
            half = i // 2
            if i % 2 == 0:

                dp[i] = max(dp[i], dp[half] * dp[half], half * half, dp[i - 2] * 2, dp[i - 3] * 3)
            else:
                dp[i] = max(dp[i], dp[half] * dp[half + 1], half * (half + 1), dp[i - 2] * 2, dp[i - 3] * 3)

        return dp[n]
```





​	实际上，只有2，3的情况特殊，其他的情况，都能够用中间的两个最大的值所得到

```python
class Solution:
    def integerBreak(self, n):
        """
        :type n: int
        :rtype: int
        """
        dp = [0, 1, 1, 2, 4, 6, 9, 12, 18]

        length = len(dp)
        if n < length:
            return dp[n]

        dp.extend([0] * (n - length + 1))

        for i in range(length, n + 1):
            half = i // 2
            if i % 2 == 0:

                dp[i] = max(dp[half] * dp[half], dp[i - 2] * 2, dp[i - 3] * 3)
            else:
                dp[i] = max(dp[half] * dp[half + 1], dp[i - 2] * 2, dp[i - 3] * 3)

        return dp[n]
```











​	又到网上查了一下，整数拆分实际上是一个数学问题，

[整数拆分]: https://baike.baidu.com/item/整数分拆/91991?fr=aladdin&amp;amp;fromid=15991027&amp;amp;fromtitle=整数拆分




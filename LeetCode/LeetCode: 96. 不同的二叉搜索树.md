# LeetCode: 96. 不同的二叉搜索树

[TOC]



## 1、题目描述



给定一个整数 *n*，求以 1 ... *n* 为节点组成的二叉搜索树有多少种？

**示例:**

```
输入: 3
输出: 5
解释:
给定 n = 3, 一共有 5 种不同结构的二叉搜索树:

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
```



## 2、解题思路

​	这道题实际上就是前面95题的一个简化版，我呢只需要知道左右节点的数量即可

​	确定根节点，然后是左右子树种类相乘，然后乘以根节点的数量，直接返回即可

```python
class Solution:
    def generateTrees(self, n):
        """
        :type n: int
        :rtype: List[TreeNode]
        """
        if n <= 0:
            return 0

        return self.dfs(1, n)

    def dfs(self, left, right):
        if left > right:
            return 1
        result = 0
        for i in range(left, right + 1):
            left_num = self.dfs(left, i - 1)
            right_num = self.dfs(i + 1, right)
            result = result + left_num * right_num

        return result
```

​	遗憾的是，这种方法得到的结果超过了时间限制，那么需要换个思路



​	分析一下，一棵树，如果有4个节点，假设已经确定了根节点，那么左子树有几种情况，右子树有几种情况呢？

- 3，0
- 2，1
- 1，2
- 0，3





​	假设我们已经知道了0，1，2，3个节点的树的组合方法，就能直接计算出来，而不需要用递归了

如果只有1个节点，左子树有0，右子树有0。所以，动态规划是从0开始的，到n结束，一共n+1

> 所以，4个节点的所有状态就是：

> [3] *[0] + [2] * [1] + [1] * [2] + [0]*[3]



```python
class Solution:
    def numTrees(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n <= 0:
            return 0

        if n == 1:
            return 1

        dp = [0 for i in range(n + 1)]
        dp[0] = 1
        dp[1] = 1
        for i in range(2, n + 1):
            for j in range(i):
                dp[i] = dp[i] + dp[j] * dp[i - 1 - j]
        return dp[n]
```



​	






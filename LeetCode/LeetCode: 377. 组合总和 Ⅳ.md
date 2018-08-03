# LeetCode: 377. 组合总和 Ⅳ

[TOC]

## 1、题目描述

​	

给定一个由正整数组成且不存在重复数字的数组，找出和为给定目标正整数的组合的个数。

**示例:**

```
nums = [1, 2, 3]
target = 4

所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)

请注意，顺序不同的序列被视作不同的组合。

因此输出为 7。
```

**进阶：**
如果给定的数组中含有负数会怎么样？
问题会产生什么变化？
我们需要在题目中添加什么限制来允许负数的出现？

**致谢：**
特别感谢 [@pbrother](https://leetcode.com/pbrother/) 添加此问题并创建所有测试用例。

## 2、解题思路

​	这道题使用动态规划来做

- 首先，将书数组中的所有的数字，建立哈希，方便查找

- 然后，建立dp[target+1]

- 然后对每一个下标开始判断

  - 例如，对和为5进行判断

  - ```
    dp[5]  =  	dp[0]+5(if 5 in nums) 
    		+	dp[1]+4(if 4 in nums) 
    		+ 	dp[2]+3(if 3 in nums)
    		+	dp[3]+2(if 2 in nums) 
    		+	dp[4]+1(if 1 in nums) 
    ```

- 以此类推，最后得到结果



​	上面的思路好像有点问题，还超时了

​	换个思路，再来一次，这一次我们使用构造法，

首先，将dp数组创建出来，然后将所有的nums数组中出现的数字对应的下标置一

接着，从前面向后进行扫描，遇到不为0的数，就用这个数字，加上nums中的数字，将对应的下标的数字增加1

举个例子

```
nums = [1, 2, 3]
target = 4
首先，创建dp
[0 0 0 0 0]
然后初始化
[0 1 1 1 0]
然后开始扫描，找到第一个不为0的数，也就是下标为1的那个1
然后判断 加上对应的nums的数字，是不是小于等于target
1+1
1+2
1+3
如果小于，对应的要加1
得到新的dp
[0 1 2 2 1]
然后继续判断，到最后位置
```



```python
class Solution:
    def combinationSum4(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        dp = [0] * (target + 1)

        for i in nums:
            if i <= target:
                dp[i] = 1

        for i in range(target + 1):
            if dp[i] == 0:
                continue

            for j in nums:
                if i + j <= target:
                    dp[i + j] += dp[i]
        return dp[-1]
```




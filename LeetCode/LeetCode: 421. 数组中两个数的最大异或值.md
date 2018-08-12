# LeetCode: 421. 数组中两个数的最大异或值

[TOC]

## 1、题目描述

给定一个非空数组，数组中元素为 a0, a1, a2, … , an-1，其中 0 ≤ ai < 231 。

找到 ai 和aj 最大的异或 (XOR) 运算结果，其中0 ≤ *i*,  *j* < *n* 。

你能在O(*n*)的时间解决这个问题吗？

**示例:**

```
输入: [3, 10, 5, 25, 2, 8]

输出: 28

解释: 最大的结果是 5 ^ 25 = 28.
```

## 2、解题思路

​	首先明确一下，数组中怎样的两个数才能异或得到最大值

​	首先，第一个数肯定是所有数中，最高位为1的，另一个数则是最高位不为1的

​	一般的想法是建立一颗字典树，然后利用二进制的性质，进行搜索

​	按位操作

​	在LeetCode的diss里面，看到了一个十分精彩的解法，只有6行：

```python
def findMaximumXOR(self, nums):
    answer = 0
    for i in range(32)[::-1]:
        answer <<= 1
        prefixes = {num >> i for num in nums}
        answer += any(answer^1 ^ p in prefixes for p in prefixes)
    return answer
```

​	直接看上去，有些困惑，不过只要把握这个算法的主要思想就就可以想到了

​	首先，这个数可定是少于32位的，如果结果中每一位异或的结果都是1，肯定能得到最大值，这个算法的思想就是这个

- 首先是第32位，看看能不能有两个数，异或以后得到1，如果能够得到，那么ans设置为1
- 然后ans左移一位，也就是判断第31位，这时候，ans=10，也就是2，
  - 重点来了，这时候，判断有没有两个数，能够在第一位为1的情况下，将第二位也设置为1
  - 利用的公式为`a^b=c,a=b^c`
  - 算法的重点在于，我只要判断能够得到最高位的那些数字就可以了，其他的数字都可以不看，因为你们得不到最高位为1，这里的最高位，指的是数组中能得到的1的最高位

```python
answer^1 ^ p in prefixes for p in prefixes
```

算法的核心在上面这一句，就是用来判断能否得到当前位为的两个数存在，

​	实际上，在判断的时候，还能够进行一点优化，因为先将所有的前缀计算出来了，实际上，只需要判断能够得到answer即可，只有所有的结果都不能得到answer的时候，这种情况才会将所有的数遍历一遍进行计算

```python
class Solution:
    def findMaximumXOR(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        ans = 0
        for i in range(32)[::-1]:
            ans = (ans << 1) ^ 1
            pre = set()
            for num in nums:
                temp = num >> i
                if (temp ^ ans) in pre:
                    break

                pre.add(temp)
            else:
                ans -= 1
        return ans
```

​	上面的就是减少了计算量，进一步提高了效率。
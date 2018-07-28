# LeetCode: 313. 超级丑数

[TOC]



## 1、题目描述

编写一段程序来查找第 *n* 个超级丑数。

超级丑数是指其所有质因数都是长度为 `k` 的质数列表 `primes` 中的正整数。

**示例:**

```
输入: n = 12, primes = [2,7,13,19]
输出: 32 
解释: 给定长度为 4 的质数列表 primes = [2,7,13,19]，前 12 个超级丑数序列为：[1,2,4,7,8,13,14,16,19,26,28,32] 。
```

**说明:**

- `1` 是任何给定 `primes` 的超级丑数。
-  给定 `primes` 中的数字以升序排列。
- 0 < `k` ≤ 100, 0 < `n` ≤ 106, 0 < `primes[i]` < 1000 。
- 第 `n` 个超级丑数确保在 32 位有符整数范围内。



## 2、解题思路

​	之前做的丑数题目， 2，3，5作为质因子，使用的是下标，然后不断地向前推进，每一次只需要找出最小的值，然后更新对应的下标即可

​	

​	采用一个思路，假如说，我们将现在已经有的数，乘以所有的质因子，然后排序，每次取出最小值，也能得到

​	使用一个堆，将这些数据都放在一起

```python
class Solution:
    def nthSuperUglyNumber(self, n, primes):
        """
        :type n: int
        :type primes: List[int]
        :rtype: int
        """
        
        uglies = [1]

        def gen(prime):
            for ugly in uglies:
                yield ugly * prime

        merged = heapq.merge(*map(gen, primes))
        while len(uglies) < n:
            ugly = next(merged)
            if ugly != uglies[-1]:
                uglies.append(ugly)
        return uglies[-1]
```



​	下面的思路是之前做丑数的思路：

```python
class Solution:
    def nthSuperUglyNumber(self, n, primes):
        """
        :type n: int
        :type primes: List[int]
        :rtype: int
        """
        
        uglies = [1]

        pos = [0] * len(primes)

        for i in range(1, n):

            first = True
            temp = 0
            change_pos = 0

            index = 0

            while index < len(pos):
                value = uglies[pos[index]] * primes[index]
                if value <= uglies[-1]:
                    pos[index] += 1
                else:
                    if first:
                        first = False
                        temp = value
                        change_pos = index
                    elif value < temp:
                        change_pos = index
                        temp = value
                    index += 1

            uglies.append(temp)
            pos[change_pos] += 1
        return uglies[-1]
```



​	
# LeetCode: 279. 完全平方数

[TOC]



## 1、题目描述

给定正整数 *n*，找到若干个完全平方数（比如 `1, 4, 9, 16, ...`）使得它们的和等于 *n*。你需要让组成和的完全平方数的个数最少。

**示例 1:**

```
输入: n = 12
输出: 3 
解释: 12 = 4 + 4 + 4.
```

**示例 2:**

```
输入: n = 13
输出: 2
解释: 13 = 4 + 9.
```



## 2、解题思路

​	使用dp来做，

- 如果当前的这个整数直接是平方数，直接返回1即可

- 如果不是，就将当前数之前的所有的平方数找出来，加上当前数减去平方数对应的索引值，更新结果，找到最小值



举个例子

```
n = 12
[0 1 0 0 1 0 0 0 0 1 0 0 0 ]
首先，从1开始，因为是1，跳过
到2，2之前的平方数是1，2-1=1，因此，更新一下结果，1+1，2
[0 1 2 0 1 0 0 0 0 1 0 0 0 ]
然后是3
3之前的平方数只有1，3-1=2，位置2对应的结果是2，2+1=3，更新为3
以此类推
```



第一个版本，超时了

```python
class Solution:
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        buff = [0] * (n + 1)

        before_square = -1
        for i in range(1, n + 1):
            temp = i * i
            if temp <= n:
                buff[temp] = 1
                before_square = temp
            else:
                break

        if before_square == n:
            return 1

        for i in range(1, n + 1):
            if buff[i] == 0:
                temp = i
                for j in range(1, i):
                    if buff[j] == 1:
                        temp = min(temp, buff[j] + buff[i - j])
                buff[i] = temp

        return buff[-1]
        
```



​	为了提升效率，将所有的完全平方数存起来



```python
class Solution:

        
    buff = [0]

    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """

        self.buff += [0] * (n - len(self.buff) + 1)

        square_buff = []

        before_square = -1
        for i in range(1, n + 1):
            temp = i * i
            if temp <= n:
                self.buff[temp] = 1
                square_buff.append(temp)
            else:
                break

        if before_square == n:
            return 1

        for i in range(1, n + 1):
            if self.buff[i] == 0:
                temp = i
                for j in square_buff:
                    if j < i:
                        temp = min(temp, self.buff[j] + self.buff[i - j])
                self.buff[i] = temp

        return self.buff[n]
       
```

​	因为可能需要需要多次运行，因此，只需要计算新那一部分



看了下别人的算法，发现，原来是数学不好（脸红。。。）

​	四平方和定理说明每个正整数均可表示为4个整数的平方和。它是费马多边形数定理和华林问题的特例。注意有些整数不可表示为3个整数的平方和，例如7。

​	根据3平方和定理，当且仅当

```
if and only if n is not of the form {\displaystyle n=4^{a}(8b+7)} n = 4^a(8b + 7) for integers a and b.
```

​	也就是说，上面形式的数字，是不能够表示成3平方和的，只能是4平方和

​	

```python
class Solution:
    def numSquares(self, n):
        """
        :type n: int
        :rtype: int
        """
        if (int(n ** 0.5)) ** 2 == n:
            return 1

        while (n & 3) == 0:
            n >>= 2
        if (n & 7) == 7:
            return 4

        sqrt_n = int(n ** 0.5)
        for i in range(1, sqrt_n + 1):
            temp = n - i * i
            if (int(temp ** 0.5)) ** 2 == temp:
                return 2

        return 3

```



[two square ]: https://en.wikipedia.org/wiki/Fermat%27s_theorem_on_sums_of_two_squares
[three square]: https://en.wikipedia.org/wiki/Legendre%27s_three-square_theorem
[four square]: https://en.wikipedia.org/wiki/Lagrange%27s_four-square_theorem


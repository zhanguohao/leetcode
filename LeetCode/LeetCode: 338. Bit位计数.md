# LeetCode: 338. Bit位计数

[TOC]



## 1、题目描述



给定一个非负整数 **num**。 对于范围 **0 ≤ i ≤ num** 中的每个数字 **i** ，计算其二进制数中的1的数目并将它们作为数组返回。

**示例：**
比如给定 `num = 5` ，应该返回 `[0,1,1,2,1,2]`.

**进阶：**

- 给出时间复杂度为**O(n \* sizeof(integer))** 的解答非常容易。 但是你可以在线性时间**O(n)**内用一次遍历做到吗？
- 要求算法的空间复杂度为**O(n)**。
- 你能进一步完善解法吗？ 在c ++或任何其他语言中不使用任何内置函数（如c++里的 **__builtin_popcount**）来执行此操作。

**致谢：**
特别感谢 [@syedee ](https://leetcode.com/discuss/user/syedee)添加此问题及所有测试用例。



## 2、解题思路

​	实际上，这个是有规律的，规律如下

首先，我们得到0-7的bit数

```
0 1 1 2 1 2 2 3
```

然后求解8-15的比特数

```
1 2 2 3 2 3 3 4
```

发现刚好是前面的0-7的比特数加一

现在结果中应该是这样的

```
0 1 1 2 1 2 2 3 1 2 2 3 2 3 3 4
```

于是，我们继续求解16-31的比特数

```
1 2 2 3 2 3 3 4 2 3 3 4 3 4 4 5
```

看出来了吧，就是前面的数据加一就能得到

其实从0开始也是这样的

```
0
1
====
0 1
1 2

```

```python
class Solution:
    def countBits(self, num):
        """
        :type num: int
        :rtype: List[int]
        """
        result = [0]
        count = 0
        cur_length = 1
        for i in range(num):
            if count >= cur_length:
                count = 0
                cur_length = len(result)
            result.append(result[count] + 1)
            count += 1
        return result
```


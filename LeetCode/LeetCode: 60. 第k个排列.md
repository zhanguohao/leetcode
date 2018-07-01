# LeetCode: 60. 第k个排列

[TOC]



## 1、题目描述



给出集合 `[1,2,3,…,*n*]`，其所有元素共有 *n*! 种排列。

按大小顺序列出所有排列情况，并一一标记，当 *n* = 3 时, 所有排列如下：

1. `"123"`
2. `"132"`
3. `"213"`
4. `"231"`
5. `"312"`
6. `"321"`

给定 *n* 和 *k*，返回第 *k* 个排列。

**说明：**

- 给定 *n* 的范围是 [1, 9]。
- 给定 *k* 的范围是[1,  *n*!]。

**示例 1:**

```
输入: n = 3, k = 3
输出: "213"
```

**示例 2:**

```
输入: n = 4, k = 9
输出: "2314"
```



## 2、解题思路

​	

这一个实际上我们能够直接计算出来

1 2 3

例如，我们想要得到第1个



首先，算出第一位，1/3= 0

因此，得到下标为0的数,也就是1，将1从原数组中移除

第二位，0%3 = 0

第二位，0/2 = 0，继续取下标为0的数，2，并将2移除

不断的计算，最终得到想要的结果



假设缓冲数组是

```
[1 2 3]
```

结果数组为

```
[]
```

想要求第1个

第一步，缓冲数组和结果数组为

```
[2 3]
[1]
```

第二步，缓冲数组和结果数组为

```
[3]
[1 2]
```

第三步，缓冲数组和结果数组为

```
[]
[1 2 3]
```



实际上，求结果数组的第一位，我们要用要求的第几个



```python
class Solution:
    def getPermutation(self, n, k):
        """
        :type n: int
        :type k: int
        :rtype: str
        """
        bits_order = [0] * (n - 1)
        for i in range(1, n):
            if i == 1:
                bits_order[i - 1] = i
            else:
                bits_order[i - 1] = i * bits_order[i - 2]

        bits_order = list(reversed(bits_order))
        buff = [i for i in range(1, n + 1)]
        result = []

        for i in range(n - 1):
            index = (k-1) // bits_order[i]
            result.append(buff[index])
            buff.pop(index)
            k %= bits_order[i]

        result.append(buff[0])
        return "".join([chr(i+48) for i in result])
```






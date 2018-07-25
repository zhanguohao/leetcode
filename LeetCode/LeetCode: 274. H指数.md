# LeetCode: 274. H指数

[TOC]

## 1、题目描述

给定一位研究者论文被引用次数的数组（被引用次数是非负整数）。编写一个方法，计算出研究者的 *h* 指数。

[h 指数的定义](https://baike.baidu.com/item/h-index/3991452?fr=aladdin): “一位有 *h* 指数的学者，代表他（她）的 *N* 篇论文中**至多**有 *h* 篇论文，分别被引用了**至少** *h* 次，其余的 *N - h* 篇论文每篇被引用次数**不多于** *h* 次。”

**示例:**

```
输入: citations = [3,0,6,1,5]
输出: 3 
解释: 给定数组表示研究者总共有 5 篇论文，每篇论文相应的被引用了 3, 0, 6, 1, 5 次。
     由于研究者有 3 篇论文每篇至少被引用了 3 次，其余两篇论文每篇被引用不多于 3 次，所以她的 h 指数是 3。
```

**说明:** 如果 *h* 有多种可能的值，*h* 指数是其中最大的那个。

## 2、解题思路

​	根据题意，如果将论文引用进行倒排序

- 如果当前值大于索引值，也就是这个是满足条件的H值，更新结果为i+1

```
[3,0,6,1,5]
排序后的结果
[6,5,3,1,0]
然后开始判断，

```



```python
class Solution:
    def hIndex(self, citations):
        """
        :type citations: List[int]
        :rtype: int
        """
        citations.sort(reverse=True)
        result = 0

        for i, k in enumerate(citations):
            if k > i:
                result = i + 1

        return result
```



​	
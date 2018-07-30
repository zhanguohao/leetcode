# LeetCode: 332. 重新安排行程

[TOC]

## 1、题目描述

给定一个机票的字符串二维数组 `[from, to]`，子数组中的两个成员分别表示飞机出发和降落的机场地点，对该行程进行重新规划排序。所有这些机票都属于一个从JFK（肯尼迪国际机场）出发的先生，所以该行程必须从 JFK 出发。

**说明:**

1. 如果存在多种有效的行程，你可以按字符自然排序返回最小的行程组合。例如，行程 ["JFK", "LGA"] 与 ["JFK", "LGB"] 相比就更小，排序更靠前
2. 所有的机场都用三个大写字母表示（机场代码）。
3. 假定所有机票至少存在一种合理的行程

 

**示例 1：**
`tickets` = `[["MUC", "LHR"], ["JFK", "MUC"], ["SFO", "SJC"], ["LHR", "SFO"]]`
返回`["JFK", "MUC", "LHR", "SFO", "SJC"]`

**示例 2：**
`tickets` = `[["JFK","SFO"],["JFK","ATL"],["SFO","ATL"],["ATL","JFK"],["ATL","SFO"]]`
返回`["JFK","ATL","JFK","SFO","ATL","SFO"]`
另一种有效的行程是 `["JFK","SFO","ATL","JFK","ATL","SFO"]`。但是它自然排序更大更靠后。

 

**贡献：**
特别感谢 [@dietpepsi](https://leetcode.com/discuss/user/dietpepsi) 提供了该问题和测试用例

## 2、解题思路

​	使用深度优先搜索，或者更简单的，我们直接对所有的票做哈希，然后每张票后面都跟着至少一个目的地，如果有多个目的地的，我们就进行排序，然后取出第一个出来

```python
class Solution:
    def findItinerary(self, tickets):
        """
        :type tickets: List[List[str]]
        :rtype: List[str]
        """
        
        tickets_buff = {}
        for ticket in tickets:
            if ticket[0] not in tickets_buff:
                tickets_buff[ticket[0]] = [ticket[1]]
            else:
                tickets_buff[ticket[0]].append(ticket[1])

        for ticket in tickets_buff:
            tickets_buff[ticket].sort()

        res = ['JFK']
        end = []
        while tickets_buff:
            if res[-1] not in tickets_buff:
                end.append(res.pop())
                continue
            fr, to = res[-1], tickets_buff[res[-1]].pop(0)
            res.append(to)
            if len(tickets_buff[fr]) == 0:
                tickets_buff.pop(fr)
        if end:
            res.extend(end[::-1])
        return res
        
        
```

​	

​	这道题实际上是深度优先搜索的题目，上面则是将走不通的路径终点进行缓存了

​	

​	直接使用深度优先搜索，倒序输出

```python
class Solution:
    def findItinerary(self, tickets):
        """
        :type tickets: List[List[str]]
        :rtype: List[str]
        """
        tickets_dict = collections.defaultdict(list)
        for a, b in sorted(tickets)[::-1]:
            tickets_dict[a] += [b]

        res = []

        def dfs(current):
            while tickets_dict[current]:
                dfs(tickets_dict[current].pop())
            res.append(current)

        dfs('JFK')
        return res[::-1]
```


# LeetCode: 433. 最小基因变化

[TOC]

## 1、题目描述

一条基因序列由一个带有8个字符的字符串表示，其中每个字符都属于 `"A"`, `"C"`, `"G"`, `"T"`中的任意一个。

假设我们要调查一个基因序列的变化。**一次**基因变化意味着这个基因序列中的**一个**字符发生了变化。

例如，基因序列由`"AACCGGTT"` 变化至 `"AACCGGTA" `即发生了一次基因变化。

与此同时，每一次基因变化的结果，都需要是一个合法的基因串，即该结果属于一个基因库。

现在给定3个参数 — start, end, bank，分别代表起始基因序列，目标基因序列及基因库，请找出能够使起始基因序列变化为目标基因序列所需的最少变化次数。如果无法实现目标变化，请返回 -1。

**注意:**

1. 起始基因序列默认是合法的，但是它并不一定会出现在基因库中。
2. 所有的目标基因序列必须是合法的。
3. 假定起始基因序列与目标基因序列是不一样的。

**示例 1:**

```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]

返回值: 1
```

**示例 2:**

```
start: "AACCGGTT"
end:   "AAACGGTA"
bank: ["AACCGGTA", "AACCGCTA", "AAACGGTA"]

返回值: 2
```

**示例 3:**

```
start: "AAAAACCC"
end:   "AACCCCCC"
bank: ["AAAACCCC", "AAACCCCC", "AACCCCCC"]

返回值: 3
```



## 2、解题思路

​	这道题跟127题很相似,直接应该使用广度优先搜索来做，直接使用127提的解法就可以

举个例子

```
start: "AACCGGTT"
end:   "AACCGGTA"
bank: ["AACCGGTA"]
首先，我们将bank进行建模
{"AACCGGT_":["AACCGGTA"]}
然后，设置两个set()，visited和not_visited
利用这两个set进行广度优先遍历
首先，对于visited中的每一个元素，我们改变一位，判断是不是在bank里面出现，如果出现了，就放到temp中
然后，判断temp中是否还有元素，如果没有，表示找不到了，返回-1
如果有元素，继续判断，判断end是不是出现在temp中，如果出现了，返回计数，没有出现，返回第一步，继续判断
```

```python
class Solution(object):
    def minMutation(self, start, end, bank):
        """
        :type start: str
        :type end: str
        :type bank: List[str]
        :rtype: int
        """
        d = {}
        for gene in bank:
            for i in range(len(gene)):
                s = gene[:i] + "_" + gene[i + 1:]
                d[s] = d.get(s, []) + [gene]

        visited = set([start])
        temp = set()
        not_visited = set(bank)
        count = 0
        while visited:
            for i in visited:
                for j in range(len(i)):
                    s = i[:j] + "_" + i[j + 1:]
                    temp = temp.union(set(d.get(s, [])))
                    d.pop(s, 0)
            if len(temp) == 0:
                return -1

            count += 1
            if end in temp:
                return count

            not_visited = not_visited - temp
            visited = temp
            temp = set()

        return -1
```




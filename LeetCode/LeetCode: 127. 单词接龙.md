# LeetCode: 127. 单词接龙

[TOC]



## 1、题目描述



​	给定两个单词（*beginWord* 和 *endWord*）和一个字典，找到从 *beginWord* 到 *endWord* 的最短转换序列的长度。转换需遵循如下规则：

1. 每次转换只能改变一个字母。
2. 转换过程中的中间单词必须是字典中的单词。

**说明:**

- 如果不存在这样的转换序列，返回 0。
- 所有单词具有相同的长度。
- 所有单词只由小写字母组成。
- 字典中不存在重复的单词。
- 你可以假设 *beginWord* 和 *endWord* 是非空的，且二者不相同。

**示例 1:**

```
输入:
beginWord = "hit",
endWord = "cog",
wordList = ["hot","dot","dog","lot","log","cog"]

输出: 5

解释: 一个最短转换序列是 "hit" -> "hot" -> "dot" -> "dog" -> "cog",
     返回它的长度 5。
```

**示例 2:**

```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: 0

解释: endWord "cog" 不在字典中，所以无法进行转换。
```





## 2、解题思路

​	这道题目类似于图中的最小路径搜索，使用的是广度优先搜索

​	当然，这里并不需要将数组变成图，我们先构造一个已经访问过的集合，一个待访问集合，还有一个是广度优先搜索的中间值

1. 首先将开始单词放入访问过集合中
2. 然后对每一个在访问过集合中的元素，到未访问过的集合中去寻找，如果存在仅相差一个字母对的单词，就放到临时集合中，判断临时集合中是不是存在目标单词，如果不存在，就继续判断
3. 如果临时集合为空，还没有找到返回0，表示未找到

```python
class Solution:
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        if endWord not in wordList:
            return 0

        visited = set([beginWord])
        temp = set()
        not_visited = set(wordList)
        count = 1
        while visited:
            for i in visited:
                for j in not_visited:
                    if [i[k] == j[k] for k in range(len(i))].count(False) == 1:
                        temp.add(j)

            if len(temp) == 0:
                return 0

            count += 1
            if endWord in temp:
                return count

            not_visited = not_visited - temp
            visited = temp
            temp = set()

        return 0
```

​	很遗憾，前面的这种方法超出了时间限制，分析一下，瓶颈可能就在判断查找改变一个字符的word上面，并且每一次对比目标单词也很耗费时间

​	为了加快搜索速度，使用哈希比较快，使用字典来存储，某一个单词改变一个字母的的时候，能够变成哪些单词

​	例如，

```
["hot","dot","dog","lot","log"]
```

例如上面的单词表，首先来看hot

如果改变第一个字母，_ot，能够变成哪些单词呢？

可以是hot，dot，lot

那么就可以在字典中存储这些单词：

{"_ot":["hot","dot","lot"]}

一次类推，对每一个单词都这样判断一下

```python
class Solution:
    def ladderLength(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: int
        """
        
        if endWord not in wordList:
            return 0

        d = {}
        for word in wordList:
            for i in range(len(word)):
                s = word[:i] + "_" + word[i + 1:]
                d[s] = d.get(s, []) + [word]

        visited = set([beginWord])
        temp = set()
        not_visited = set(wordList)
        count = 1
        while visited:
            for i in visited:
                for j in range(len(i)):
                    s = i[:j] + "_" + i[j + 1:]
                    temp = temp.union(set(d.get(s, [])))
                    d.pop(s, 0)
            if len(temp) == 0:
                return 0

            count += 1
            if endWord in temp:
                return count

            not_visited = not_visited - temp
            visited = temp
            temp = set()

        return 0
```

​	这一次虽然通过了，但是耗费时间还是很长，还需要优化


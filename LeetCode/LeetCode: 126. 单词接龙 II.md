# LeetCode: 126. 单词接龙 II

[TOC]

## 1、题目描述

给定两个单词（*beginWord* 和 *endWord*）和一个字典 *wordList*，找出所有从 *beginWord* 到 *endWord* 的最短转换序列。转换需遵循如下规则：

1. 每次转换只能改变一个字母。
2. 转换过程中的中间单词必须是字典中的单词。

**说明:**

- 如果不存在这样的转换序列，返回一个空列表。
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

输出:
[
  ["hit","hot","dot","dog","cog"],
  ["hit","hot","lot","log","cog"]
]
```

**示例 2:**

```
输入:
beginWord = "hit"
endWord = "cog"
wordList = ["hot","dot","dog","lot","log"]

输出: []

解释: endWord "cog" 不在字典中，所以不存在符合要求的转换序列。
```



## 2、解题思路

​	这道题与127题单词接龙和433题基因变化相似，区别就在与，我们需要保存路径的前向节点，最后遍历一遍，就得到结果了

​	换个思路解决问题，首先来构建一个结构，每一个都保存这当前值的前向节点

分两步来做

- 首先，将数组进行转换

  ```python
  d = collections.defaultdict(list)
          wordList.append(beginWord)
          for word in wordList:
              for i in range(len(word)):
                  s = word[:i] + "_" + word[i + 1:]
                  d[s] += [word]
  ```

  ```
  # 经过上面的变换，得到下面的结果
  defaultdict(<class 'list'>, {'l_g': ['log'], 'd_g': ['dog'], '_og': ['dog', 'log', 'cog'], 'l_t': ['lot'], 'do_': ['dot', 'dog'], 'ho_': ['hot'], 'co_': ['cog'], 'hi_': ['hit'], 'h_t': ['hot', 'hit'], '_it': ['hit'], 'lo_': ['lot', 'log'], 'd_t': ['dot'], 'c_g': ['cog'], '_ot': ['hot', 'dot', 'lot']})
  ```

- 然后，将前向节点字典生成出来

  ```python
  pre = collections.defaultdict(set)
          for _, value in d.items():
              for i in value:
                  pre[i] = pre[i].union(set(value))
                  pre[i].remove(i)
  ```

  ```
  defaultdict(<class 'set'>, {'dog': {'log', 'dot', 'cog'}, 'log': {'dog', 'lot', 'cog'}, 'lot': {'hot', 'log', 'dot'}, 'hot': {'lot', 'dot', 'hit'}, 'dot': {'dog', 'hot', 'lot'}, 'hit': {'hot'}, 'cog': {'dog', 'log'}})
  ```

- 这样，我们就能够利用树的性质，进行层次遍历，需要注意的就是，已经访问过的节点不能继续访问，不然会产生回环，因此，需要保存一下访问过的路径

  这样，我们就能够从endword开始，向前寻找beginword，找到以后，就加入到结果集中



​	这个思路可以，不过最后是在127题的解题思路上修改了一下，如下

​	如果能够正向的找到endword，那么我们在重新反向找到beginword，就得到路径了，下面就是基于这个思路来做的：

- 首先，将数组进行变换，同前面一样

- 然后，利用两个set，一个是访问过，一个是未访问过，然后进行层次遍历

  在遍历的过程中，建立一个字典，前向节点的字典，方便我们根据这个字典，回溯路径

如果层次遍历无法得到，最后直接返回[]

如果第一次找到了，我们就跳出来，根据前向节点的字典，进行路径回溯，然后加入到结果集中



```python
class Solution:
    def findLadders(self, beginWord, endWord, wordList):
        """
        :type beginWord: str
        :type endWord: str
        :type wordList: List[str]
        :rtype: List[List[str]]
        """
        
        if endWord not in wordList:
            return []

        d = collections.defaultdict(list)
        for word in wordList:
            for i in range(len(word)):
                s = word[:i] + "_" + word[i + 1:]
                d[s] += [word]

        path_node = set()

        visited = set([beginWord])
        temp = set()
        not_visited = set(wordList)

        pre = collections.defaultdict(set)

        pop_item = set()
        count = 0
        while visited:
            for i in visited:
                for j in range(len(i)):
                    s = i[:j] + "_" + i[j + 1:]
                    temp = temp.union(set(d[s]))
                    for w in d[s]:
                        path_node.add(w)
                        if i != w:
                            pre[w].add(i)
                    pop_item.add(s)

            for p in pop_item:
                d.pop(p)
            pop_item.clear()

            if len(temp) == 0:
                return []
            count += 1
            if endWord in temp:
                break

            not_visited = not_visited - temp
            visited = temp
            temp = set()

        ans = [[endWord]]
        temp_ans = []
        visited_path = set([endWord])
        for i in range(count):
            for path in ans:
                temp_next = pre[path[-1]] - visited_path
                for node in temp_next:
                    temp_ans.append(path.copy())
                    temp_ans[-1].append(node)
                visited_path.add(path[-1])
            ans = temp_ans
            temp_ans = []

        return [ list(reversed(x)) for x in ans if x[-1] == beginWord ]
```








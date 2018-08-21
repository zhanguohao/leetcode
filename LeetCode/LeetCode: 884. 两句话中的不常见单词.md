# LeetCode: 884. 两句话中的不常见单词

[TOC]

## 1、题目描述

给定两个句子 `A` 和 `B` 。 （*句子*是一串由空格分隔的单词。每个*单词*仅由小写字母组成。）

如果一个单词在其中一个句子中只出现一次，在另一个句子中却没有出现，那么这个单词就是*不常见的*。

返回所有不常用单词的列表。

您可以按任何顺序返回列表。

 


**示例 1：**

```
输入：A = "this apple is sweet", B = "this apple is sour"
输出：["sweet","sour"]
```

**示例 2：**

```
输入：A = "apple apple", B = "banana"
输出：["banana"]
```

 

**提示：**

1. `0 <= A.length <= 200`
2. `0 <= B.length <= 200`
3. `A` 和 `B` 都只包含空格和小写字母。



## 2、解题思路

	这道题是简单题，哈哈（简单题好简单）

- 首先使用哈希统计出两个句子的单词数量
- 将句子1中出现1次，并且不在句子2中的单词放入结果数组
- 将句子2中出现1次，并且不在句子1中的单词放入结果数组

```python
class Solution:
    def uncommonFromSentences(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: List[str]
        """
        ans = []

        count_A = collections.Counter(A.split(" "))
        count_B = collections.Counter(B.split(" "))

        ans += [x for x in count_A if count_A[x] == 1 and x not in count_B]
        ans += [x for x in count_B if count_B[x] == 1 and x not in count_A]

        return ans
```
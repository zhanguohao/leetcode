# LeetCode: 318. 最大单词长度乘积

[TOC]



# 1、题目描述

给定一个字符串数组 `words`，找到 `length(word[i]) * length(word[j])` 的最大值，并且这两个单词不含有公共字母。你可以认为每个单词只包含小写字母。如果不存在这样的两个单词，返回 0。

**示例 1:**

```
输入: ["abcw","baz","foo","bar","xtfn","abcdef"]
输出: 16 
解释: 这两个单词为 "abcw", "xtfn"。
```

**示例 2:**

```
输入: ["a","ab","abc","d","cd","bcd","abcd"]
输出: 4 
解释: 这两个单词为 "ab", "cd"。
```

**示例 3:**

```
输入: ["a","aa","aaa","aaaa"]
输出: 0 
解释: 不存在这样的两个单词。
```

​	

## 2、解题思路

​	首先，判断当前要判断的字符串的长度之积是不是大于结果，如果是，继续判断有没有交集，如果没有，更新结果

​	这个题目最麻烦的一点就是判断两个字符串有没有交集，利用哈希来做是很快的



​	我们对每一个字符串做哈希，因为只有26个英文小写字母，因此，采用整数位运算的形式，如果出现a，表示第0位置一，如果出现b，表示第1位置一

​	比较两个字符串，就是将哈希值进行与，如果得到0，表示没有相同的字符串，如果不是0 ，表示得到了相同的字符串

​	

```python
class Solution:
    def maxProduct(self, words):
        """
        :type words: List[str]
        :rtype: int
        """
        
        res = 0

        hash_word = [self.hash_string(s) for s in words]

        for i in range(len(words)):
            for j in range(i + 1, len(words)):
                temp = len(words[i]) * len(words[j])
                if temp > res:
                    if hash_word[i] & hash_word[j] == 0:
                        res = temp
        return res

    def hash_string(self, s):

        res = 0

        for i in s:
            res |= (1 << (ord(i) - 97))

        return res
```

​	改进一下，实际上，我们可以使用他的哈希值，作为字段的键，字符串长度作为值，判断如果键的与为0，更新结果

​	并且需要注意的就是，如果两个字符串有相同的哈希，那么我们取更长的长度作为它的值，这样才能找到最大的乘积



```python
class Solution:
    def maxProduct(self, words):
        """
        :type words: List[str]
        :rtype: int
        """
        
        res = 0

        hash_dict = {}

        for word in words:
            hash_value = self.hash_string(word)
            if hash_dict.get(hash_value) is None:
                hash_dict[hash_value] = len(word)
            else:
                if hash_dict[hash_value] < len(word):
                    hash_dict[hash_value] = len(word)

        for i, j in itertools.combinations(hash_dict.keys(), 2):
            if not (i & j):
                res = max(res, hash_dict[i] * hash_dict[j])

        return res

    def hash_string(self, s):

        res = 0

        for i in s:
            res |= (1 << (ord(i) - 97))

        return res  
```

​	这次提升了很多，战胜了90.32%。。

​	因为减少了很多组合数，所以执行速度变快了
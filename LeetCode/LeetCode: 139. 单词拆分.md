# LeetCode: 139. 单词拆分

[TOC]



## 1、题目描述



给定一个**非空**字符串 *s* 和一个包含**非空**单词列表的字典 *wordDict*，判定 *s* 是否可以被空格拆分为一个或多个在字典中出现的单词。

**说明：**

- 拆分时可以重复使用字典中的单词。
- 你可以假设字典中没有重复的单词。

**示例 1：**

```
输入: s = "leetcode", wordDict = ["leet", "code"]
输出: true
解释: 返回 true 因为 "leetcode" 可以被拆分成 "leet code"。
```

**示例 2：**

```
输入: s = "applepenapple", wordDict = ["apple", "pen"]
输出: true
解释: 返回 true 因为 "applepenapple" 可以被拆分成 "apple pen apple"。
     注意你可以重复使用字典中的单词。
```

**示例 3：**

```
输入: s = "catsandog", wordDict = ["cats", "dog", "sand", "and", "cat"]
输出: false
```



## 2、解题思路

​	这道题也可以用深度优先搜索，如果整个单词都被搜索完毕，返回真，否则返回假

​	因为在数组中找到单词比较慢，我们直接使用字典来做，先将wordDict转换为字典

​	很遗憾，用递归超出了时间限制。。。

```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        word_dict = {}
        for i in wordDict:
            word_dict[i] = 1
        return self.dfs(s, word_dict)

    def dfs(self, s, word_dict):
        if s == "":
            return True

        result = False
        for i in range(1, len(s) + 1):
            if s[:i] in word_dict:
                result = result or self.dfs(s[i:], word_dict)

        return result
```

​	

​	再来分析一下这个题目，实际上，我们发现，假如前面i-1个字符能够用字典中的单词匹配，那么前i个能不能呢



​	假如 0<=j <= i-1, 我们发现j的时候能够匹配，并且s[j:i+1]也在字典中，那么当前的肯定能够匹配成功

​	为了匹配第一个字母，dp的长度是len(s)+1

​	举个例子，

```
s = "leetcode", wordDict = ["leet", "code"]
```

```
dp = [True, False, False, False, False, False, False, False, False]
首先判断第一个字母，'l',不存在
dp = [True, False, False, False, False, False, False, False, False]
然后是判断前两个字母，这时候，有两种情况，
dp[0] and "le"	False
dp[1] and "e"	False
接着是前三个字母
有三种情况
dp[0] and "lee" False
dp[1] and "ee"	False
dp[2] and "e"	False
以此类推，每一次判断同样是基于前面判断的结果
为了加快判断效率，一旦遇到True，直接break

```



```python
class Solution(object):
    def wordBreak(self, s, wordDict):
        """
        :type s: str
        :type wordDict: List[str]
        :rtype: bool
        """
        word_dict = {}
        for i in wordDict:
            word_dict[i] = 1
            
        length = len(s)
        
        dp = [False] * (length + 1)

        dp[0] = True

        for i in range(1, length + 1):
            temp = False
            for j in range(i):
                temp = dp[j] and s[j:i] in word_dict
                if temp:
                    break
            dp[i] = temp

        return dp[len(s)]
```


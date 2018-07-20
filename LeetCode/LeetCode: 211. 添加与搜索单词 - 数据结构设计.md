# LeetCode: 211. 添加与搜索单词 - 数据结构设计

[TOC]



## 1、题目描述

设计一个支持以下两种操作的数据结构：

```
void addWord(word)
bool search(word)
```

search(word) 可以搜索文字或正则表达式字符串，字符串只包含字母 `.` 或 `a-z` 。 `.` 可以表示任何一个字母。

**示例:**

```
addWord("bad")
addWord("dad")
addWord("mad")
search("pad") -> false
search("bad") -> true
search(".ad") -> true
search("b..") -> true
```

**说明:**

你可以假设所有单词都是由小写字母 `a-z` 组成的。

------

## 2、解题思路

​	这道题目和前面的前缀树Trie差不多，只是在前面的基础上，增加一个.的判断，也就是正则

​	如果遇到的是点，就将当前的那个字符的左右的后续字符全部判断一遍



```python
class WordDictionary:
    
    
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.head = {}
        self.head['.'] = []

    def addWord(self, word):
        """
        Adds a word into the data structure.
        :type word: str
        :rtype: void
        """
        cur = self.head
        for i in word:
            if cur.get(i) is None:
                cur[i] = {}
                cur['.'].append(cur[i])
                cur[i]['.'] = []
            cur = cur[i]
        if cur.get("end") is None:
            cur["end"] = 1

    def search(self, word):
        """
        Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter.
        :type word: str
        :rtype: bool
        """
        return self.getWord(word, self.head, 0)

    def getWord(self, word, d, index):
        if index >= len(word):
            if d.get("end") is not None:
                return True
            else:
                return False

        if d.get(word[index]) is None:
            return False
        else:
            if word[index] == '.':
                for i in d.get('.'):
                    if self.getWord(word, i, index + 1):
                        return True
                return False
            else:
                return self.getWord(word, d.get(word[index]), index + 1)
    
    
    
    
    

# Your WordDictionary object will be instantiated and called as such:
# obj = WordDictionary()
# obj.addWord(word)
# param_2 = obj.search(word)
```


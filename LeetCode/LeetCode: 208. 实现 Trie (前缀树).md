# LeetCode: 208. 实现 Trie (前缀树)

[TOC]



## 1、题目描述



实现一个 Trie (前缀树)，包含 `insert`, `search`, 和 `startsWith` 这三个操作。

**示例:**

```
Trie trie = new Trie();

trie.insert("apple");
trie.search("apple");   // 返回 true
trie.search("app");     // 返回 false
trie.startsWith("app"); // 返回 true
trie.insert("app");   
trie.search("app");     // 返回 true
```

**说明:**

- 你可以假设所有的输入都是由小写字母 `a-z` 构成的。
- 保证所有输入均为非空字符串。



## 2、解题思路

​	直接用哈希来做，python的话，就用字典实现即可

- 初始化

  一开始，头指针，指向一个字典，字典里面什么都没有

  插入的时候，就判断字符在不在，如果不在，就加入一个字符，值为{}

  如果插入到最后一个字符，就在下一个字典中放入一个"end"，用来标识单词结束

  

```python
class Trie:
    
    
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.head = {}

    def insert(self, word):
        """
        Inserts a word into the trie.
        :type word: str
        :rtype: void
        """
        cur = self.head
        for i in word:
            if cur.get(i) is None:
                cur[i] = {}
            cur = cur[i]
        if cur.get("end") is None:
            cur["end"] = 1

    def search(self, word):
        """
        Returns if the word is in the trie.
        :type word: str
        :rtype: bool
        """
        cur = self.head
        for i in word:
            if i not in cur:
                return False
            cur = cur[i]

        if cur.get("end") is not None:
            return True
        else:
            return False

    def startsWith(self, prefix):
        """
        Returns if there is any word in the trie that starts with the given prefix.
        :type prefix: str
        :rtype: bool
        """
        cur = self.head
        for i in prefix:
            if i not in cur:
                return False
            cur = cur[i]


        return True
    
    

# Your Trie object will be instantiated and called as such:
# obj = Trie()
# obj.insert(word)
# param_2 = obj.search(word)
# param_3 = obj.startsWith(prefix)
```


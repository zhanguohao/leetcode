# LeetCode: 91. 解码方法

[TOC]

## 1、题目描述



一条包含字母 `A-Z` 的消息通过以下方式进行了编码：

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

给定一个只包含数字的**非空**字符串，请计算解码方法的总数。

**示例 1:**

```
输入: "12"
输出: 2
解释: 它可以解码为 "AB"（1 2）或者 "L"（12）。
```

**示例 2:**

```
输入: "226"
输出: 3
解释: 它可以解码为 "BZ" (2 26), "VF" (22 6), 或者 "BBF" (2 2 6) 。
```





## 2、解题思路

​	看到题目描述就能想到的就是深度遍历，每一次前进一个有效的字符，向下传递，直到全部解码，解码数加一

​	可惜的是，下面的递归方法耗费时间太长了，超出了时间限制

```python
class Solution:
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        return self.dfs(s, 0)

    def dfs(self, s, index):
        if index >= len(s):
            return 1

        if ord(s[index]) <= ord('0'):
            return 0
        
        count = 0

        if index < len(s) - 1:
            if ord(s[index]) <= ord('2') and ord(s[index + 1]) <= ord('6') or ord(s[index]) <= ord('1'):
                count += self.dfs(s, index + 2)

        count += self.dfs(s, index + 1) 

        return count
```



​	换个思路，这道题明显是后面的结果依赖于前面的结果，这与动态规划很相似，用动态规划做一下试试

​	关键是递推公式该是如何

​	结果数组：result

​	result[i] 表示，前i个字符的解码数量，所以result的长度是字符串长度+1

​	假如说当前字符的前面两个字符能够合并，表示前面的解码方式能够叠加，也就是说，i-2的字符的解码方式加上i-1的字符的解码方式，

​	为什么能够叠加呢，因为i-2的字符与i-1的字符合并成一个，这样来说，我们看前面的解码方式，就有两个了

​	如果不能叠加在一起，就直接看前面的那个就好了，也就是i-1

```python
class Solution:
    def numDecodings(self, s):
        """
        :type s: str
        :rtype: int
        """
        
        
        length = len(s)
        if length == 0:
            return 0

        result = [0 for i in range(length + 1)]

        result[0] = 1

        for i in range(1, length + 1):
            if s[i - 1] == '0':
                result[i - 1] = 0

            if i >= 2 and (s[i - 2] == '1' or s[i - 2] == '2' and s[i - 1] <= '6'):
                result[i] = result[i - 1] + result[i - 2]

            else:
                result[i] = result[i - 1]
        return result[length]
```



​	


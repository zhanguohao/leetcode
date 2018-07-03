# LeetCode: 93. 复原IP地址

[TOC]

## 1、题目描述

​	



给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

**示例:**

```
输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]
```



## 2、解题思路

​	先想到的是深度优先搜索

​	

每一次，取一个字符，2个字符，3个字符，分别判断，如果满足条件，继续判断

如果刚好用尽了所有字符，并且数组里面元素刚刚好4个，表示满足条件，加入结果数组

需要注意，如果有0，那么前两位不能是0





```python
class Solution:
    def restoreIpAddresses(self, s):
        """
        :type s: str
        :rtype: List[str]
        """
        
        result = []
        self.dfs(s, 0, [], result)
        return list(set(result))

    def dfs(self, s, index, current, result):
        if index >= len(s):
            if len(current) == 4:
                result.append(".".join(current))
            return
        elif len(current) >= 4:
            return

        temp = current[:]
        temp_num = int(s[index:index + 1])
        if temp_num <= 255 and (len(s) - index - 1) <= (3 - len(current)) * 3:
            temp.append(s[index:index + 1])
            self.dfs(s, index + 1, temp, result)

        temp = current[:]
        temp_num = int(s[index:index + 2])
        if temp_num <= 255 and s[index] != '0' and (len(s) - index - 2) <= (3 - len(current)) * 3:
            temp.append(s[index:index + 2])
            self.dfs(s, index + 2, temp, result)

        temp = current[:]
        temp_num = int(s[index:index + 3])
        if temp_num <= 255 and s[index:index + 2] != '00' and s[index] != '0' and (len(s) - index - 3) <= (
            3 - len(current)) * 3:
            temp.append(s[index:index + 3])
            self.dfs(s, index + 3, temp, result)
```


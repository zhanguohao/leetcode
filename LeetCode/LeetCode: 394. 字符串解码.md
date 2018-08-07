# LeetCode: 394. 字符串解码

[TOC]

## 1、题目描述

​	

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 *encoded_string* 正好重复 *k* 次。注意 *k* 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 *k* ，例如不会出现像 `3a` 或 `2[4]` 的输入。

**示例:**

```
s = "3[a]2[bc]", 返回 "aaabcbc".
s = "3[a2[c]]", 返回 "accaccacc".
s = "2[abc]3[cd]ef", 返回 "abcabccdcdcdef".
```



## 2、解题思路

​	使用深度优先搜索

- 设置结果list
- 如果当前字符是字母，添加到结果数组中
- 如果是数字，开始进入识别数字模式
- 如果当前字符是'['，开始寻找它对应的']'所在的位置，将这一段递归
- 使用数字乘以上面'[]'中字符串的返回值，放入结果数组中
- 最后将结果数组连接起来，返回



举个例子

```
"3[a]2[bc]"
首先，识别到3，然后，进入子串识别，识别子串'a'
'a'
子串的返回值是'a'
因此，将3*'a'放入结果数组
['aaa']
同样的道理，识别到后面的字符串，然后放到结果数组中
['aaa','bcbc']
最后连接起来，返回
'aaabcbc'
```

```python
class Solution:
    def decodeString(self, s):
        """
        :type s: str
        :rtype: str
        """
        
        res = []

        index = 0

        num = ''
        while index < len(s):

            if s[index].isalpha():
                res.append(s[index])
            elif s[index].isnumeric():
                num += s[index]
                index += 1
                while s[index].isnumeric():
                    num += s[index]
                    index += 1
                index -= 1
            elif s[index] == '[':
                index += 1
                begin = index
                count = 1
                while count:
                    if s[index] == '[':
                        count += 1
                    elif s[index] == ']':
                        count -= 1
                    index += 1
                index -= 1
                res.append(int(num) * self.decodeString(s[begin:index]))
                num = ''
            index += 1
        return ''.join(res)
        
```


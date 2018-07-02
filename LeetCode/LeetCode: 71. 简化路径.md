# LeetCode: 71. 简化路径

[TOC]

## 1、题目描述

​	

给定一个文档 (Unix-style) 的完全路径，请进行路径简化。

例如，
**path** = `"/home/"`, => `"/home"`
**path** = `"/a/./b/../../c/"`, => `"/c"`

**边界情况:**

- 你是否考虑了 路径 = `"/../"` 的情况？
  在这种情况下，你需返回 `"/"` 。
- 此外，路径中也可能包含多个斜杠 `'/'` ，如 `"/home//foo/"` 。
  在这种情况下，你可忽略多余的斜杠，返回 `"/home/foo"` 。



## 2、解题思路

​	这道题实际上很简单，不需要借助栈，也不需要其他的工具

- 首先，使用`/` 将字符串拆分，然后，从后面向前扫描
- 如果是空串，也就表示出现的是//,直接删掉
- 如果是`..`，那么表示前面有一个目录需要简化，用一个计数器计数
- 如果遇到的是非空串，他可以抵消一个`..`，
- 最后,使用`/`将各个元素隔开，并在前面添加`/`即可



```python
class Solution:
    def simplifyPath(self, path):
        """
        :type path: str
        :rtype: str
        """
        temp = path.split('/')
        length = len(temp)

        count2Points = 0

        while length > 0:
            if temp[length - 1] == "":
                temp.pop(length - 1)
            elif temp[length - 1] == '..':
                count2Points += 1
                temp.pop(length - 1)
            elif temp[length - 1] == '.':
                temp.pop(length - 1)
            else:
                if count2Points > 0:
                    temp.pop(length - 1)
                    count2Points -= 1

            length -= 1

        return "/" + "/".join(temp)
```


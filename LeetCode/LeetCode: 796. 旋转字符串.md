# LeetCode: 796. 旋转字符串

[TOC]

## 1、题目描述





给定两个字符串, `A` 和 `B`。

`A` 的旋转操作就是将 `A` 最左边的字符移动到最右边。 例如, 若 `A = 'abcde'`，在移动一次之后结果就是`'bcdea'` 。如果在若干次旋转操作之后，`A` 能变成`B`，那么返回`True`。

```
示例 1:
输入: A = 'abcde', B = 'cdeab'
输出: true

示例 2:
输入: A = 'abcde', B = 'abced'
输出: false
```

**注意：**

- `A` 和 `B` 长度不超过 `100`。





## 2、解题思路

​	

```python
class Solution:
    def rotateString(self, A, B):
        """
        :type A: str
        :type B: str
        :rtype: bool
        """
        if A == B:
            return True
        
        
        if len(A) != len(B):
            return False

        index = 0
        for i in B:
            if A[0] == i:
                length = len(A) - index
                if A[:length] == B[index:] and A[length:] == B[:index]:
                    return True
            index += 1
        return False
        
```


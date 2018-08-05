# LeetCode: 385. Mini Parser

[TOC]

## 1、题目描述

Given a nested list of integers represented as a string, implement a parser to deserialize it.

Each element is either an integer, or a list -- whose elements may also be integers or other lists.

**Note:** You may assume that the string is well-formed:

- String is non-empty.
- String does not contain white spaces.
- String contains only digits `0-9`, `[`, `-` `,`, `]`.

**Example 1:**

```
Given s = "324",

You should return a NestedInteger object which contains a single integer 324.
```

**Example 2:**

```
Given s = "[123,[456,[789]]]",

Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.
```

## 2、解题思路

​	首先来理解一下，这是一个嵌套的对象的反序列化，需要关注的几个点：

- 数字的识别
  - 如果一个字母是数字的话，就要不断的向后移动，直到这个不是数字为止
- list的识别
  - 遇到'[]'的时候，将这个字母入栈，等到遇到']'的时候，出栈

```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

class Solution:
    def deserialize(self, s):
        """
        :type s: str
        :rtype: NestedInteger
        """
        
        res = NestedInteger()
        nest_stack = [res]

        if s[0].isnumeric() or s[0] == '-':
            num_buff = s[0]
            num_sign = True
        else:
            num_sign = False
            num_buff = ''
        index = 1

        while index < len(s):
            if num_sign:
                if s[index].isnumeric():
                    num_buff += s[index]
                else:
                    num_sign = False
                    nest_stack[-1].setInteger(int(num_buff))
                    nest_stack.pop()
                    index -= 1
                    num_buff = ''

            else:

                if s[index].isnumeric() or s[index] == '-':
                    num_sign = True
                    num_buff = s[index]
                    temp = NestedInteger()
                    nest_stack[-1].add(temp)
                    nest_stack.append(temp)
                else:
                    if s[index] == '[':
                        temp = NestedInteger()
                        nest_stack[-1].add(temp)
                        nest_stack.append(temp)
                    elif s[index] == ']':
                        nest_stack.pop()

            index += 1

        if num_buff:
            nest_stack[-1].setInteger(int(num_buff))
            nest_stack.pop()
        return res
```


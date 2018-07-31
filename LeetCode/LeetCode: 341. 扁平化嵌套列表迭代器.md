# LeetCode: 341. 扁平化嵌套列表迭代器

[TOC]

## 1、题目描述



给出一个嵌套的整型列表。设计一个迭代器，遍历这个整型列表中的所有整数。

列表中的项或者为一个整数，或者是另一个列表。

**示例 1:**
给定列表 `[[1,1],2,[1,1]]`,

通过重复调用 *next* 直到 *hasNex*t 返回false，*next* 返回的元素的顺序应该是: `[1,1,2,1,1]`.

**示例 2:**
给定列表 `[1,[4,[6]]]`,

通过重复调用 *next* 直到 *hasNex*t 返回false，*next* 返回的元素的顺序应该是: `[1,4,6]`.

## 2、解题思路

​	设计一个栈，如果是一个list，就放到栈里面，栈顶存放一个数字

​	但是，我们还需要知道他的下标，因此，我们就要同时保存下标



```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger(object):
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
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

class NestedIterator(object):
    
    def __init__(self, nestedList):
        """
        Initialize your data structure here.
        :type nestedList: List[NestedInteger]
        """

        self.stack = [[nestedList, 0]]

    def next(self):
        """
        :rtype: int
        """

        self.hasNext()
        nestedList, index = self.stack[-1]
        self.stack[-1][1] += 1
        return nestedList[index].getInteger()

    def hasNext(self):
        """
        :rtype: bool
        """

        stack = self.stack
        while stack:
            nestedList, index = self.stack[-1]
            if index == len(nestedList):
                stack.pop()
            else:
                cur = nestedList[index]
                if cur.isInteger():
                    return True
                stack[-1][1] += 1
                stack.append([cur.getList(), 0])
        return False
        

# Your NestedIterator object will be instantiated and called as such:
# i, v = NestedIterator(nestedList), []
# while i.hasNext(): v.append(i.next())
```



​	
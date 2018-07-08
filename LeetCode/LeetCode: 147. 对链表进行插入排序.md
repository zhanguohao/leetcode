# LeetCode: 147. 对链表进行插入排序

[TOC]

## 1、题目描述

对链表进行插入排序。

![img](https://upload.wikimedia.org/wikipedia/commons/0/0f/Insertion-sort-example-300px.gif)
插入排序的动画演示如上。从第一个元素开始，该链表可以被认为已经部分排序（用黑色表示）。
每次迭代时，从输入数据中移除一个元素（用红色表示），并原地将其插入到已排好序的链表中。

 

**插入排序算法：**

1. 插入排序是迭代的，每次只移动一个元素，直到所有元素可以形成一个有序的输出列表。
2. 每次迭代中，插入排序只从输入数据中移除一个待排序的元素，找到它在序列中适当的位置，并将其插入。
3. 重复直到所有输入数据插入完为止。

 

**示例 1：**

```
输入: 4->2->1->3
输出: 1->2->3->4
```

**示例 2：**

```
输入: -1->5->3->4->0
输出: -1->0->3->4->5
```



## 2、 解题思路

​	这个与直接的插入排序没有区别，只是需要注意不能直接赋值而已

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def insertionSortList(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        if not head or not head.next:
            return head

        result = ListNode(0)
        result.next = head

        

        sort_node = head.next
        temp = sort_node
        
        head.next = None
        
        
        while temp:
            temp = temp.next
            find = result.next
            prev = result
            while find:
                if sort_node.val < find.val:
                    prev.next = sort_node
                    sort_node.next = find
                    break
                prev = find
                find = find.next

            if not find:
                prev.next = sort_node
                sort_node.next = None
            sort_node = temp

        return result.next
```

​	这道题的解题思路很多，后面再来补充
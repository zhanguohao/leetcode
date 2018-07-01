# LeetCode: 160 . 相交链表

[TOC]

## 1、题目描述



编写一个程序，找到两个单链表相交的起始节点。

 

例如，下面的两个链表**：**

```
A:          a1 → a2
                   ↘
                     c1 → c2 → c3
                   ↗            
B:     b1 → b2 → b3
```

在节点 c1 开始相交。

 

**注意：**

- 如果两个链表没有交点，返回 `null`.
- 在返回结果后，两个链表仍须保持原有的结构。
- 可假定整个链表结构中没有循环。
- 程序尽量满足 O(*n*) 时间复杂度，且仅用 O(*1*) 内存。

 

## 2、解题思路

### 2.1 哈希表法

​	直接将第一个表的所有的节点的地址放到哈希表中，然后第二链表从头开始，如果当前地址已经存在在哈希表中，表示找到了，返回该地址

​	不过这种办法的空间复杂度是$O(m)$的，并不是$O(1)$



## 2.2 长度查找法

​	首先明确一个问题，就是加入两个单链表相交，那么最后面的节点肯定是相同的，也就是从相交节点后，一定是相同的，因此，从头找到两个链表的长度，m，n，假设n>m

​	让第二个链表先走n-m步，然后两个链表开始匹配，如果匹配到了，表示找到了

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode *getIntersectionNode(struct ListNode *headA, struct ListNode *headB) {
    int length_a = 0;
    int length_b = 0;

    struct ListNode *dir_a = headA;
    struct ListNode *dir_b = headB;

    while (dir_a || dir_b) {
        if (dir_a) {
            length_a++;
            dir_a = dir_a->next;
        }

        if (dir_b) {
            length_b++;
            dir_b = dir_b->next;
        }
    }

    if (length_a <= 0 || length_b <= 0) {
        return NULL;
    }

    dir_a = headA;
    dir_b = headB;

    int differ = length_a > length_b ? length_a - length_b : length_b - length_a;
    int equal = length_a > length_b ? length_b : length_a;
    if (length_a > length_b) {
        while (differ-- > 0) {
            dir_a = dir_a->next;
        }
    } else {
        while (differ-- > 0) {
            dir_b = dir_b->next;
        }
    }

    while (equal-- > 0) {
        if (dir_a == dir_b) {
            return dir_a;
        } else {
            dir_a = dir_a->next;
            dir_b = dir_b->next;
        }
    }

    return NULL;
}
```



### 2.3 回环法

​	回环法则是将第一个链表的末尾放到第二个链表的头上，如果两个链表相交，就找那个回环节点

​	实际上，回环法有问题的，因为找到的回环点不一定是第一个相交点

​	回环法可以用来检测是否有回环，但不能用来找到第一个相交点

==注意：！！！==





​	
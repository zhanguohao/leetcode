# LeetCode: 234. 回文链表

[TOC]

## 1、题目描述





请判断一个链表是否为回文链表。

**示例 1:**

```
输入: 1->2
输出: false
```

**示例 2:**

```
输入: 1->2->2->1
输出: true
```

**进阶：**
你能否用 O(n) 时间复杂度和 O(1) 空间复杂度解决此题？



## 2、解题思路



​	为额尽量实现$O(1)$ 的空间复杂度，而且尽可能降低时间复杂度，选择将链表的左边部分反转，判断完以后，反转回来



- 确认链表的长度
- 根据长度，找到开始比较的值，例如，长度是4，如果从1开始计数，就从中间开始比较，先比较第2个和第3个，然后是第1个和第4个
- 如果是5个，第3个不用比较， 先比较第2个和第3个，然后是第1个和第4个
- 将前面一半元素进行反转，然后得到两个链表进行比较，她们的末尾都是null
- 比较完毕得到结果，将前面的一半元素恢复原状
- 返回结果



```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
bool isPalindrome(struct ListNode* head) {
    if (!head || !head->next) {
        return true;
    }

    int length = 0;
    struct ListNode *node = head;


    while (node) {
        length++;
        node = node->next;
    }

    bool result = true;

    // 查找需要比较的位置
    struct ListNode *left = head;
    struct ListNode *right = head;

    int left_pos = length / 2;
    int right_pos = length - length / 2 + 1;

    int compare_count = left_pos;

    while (--left_pos) {
        left = left->next;
    }

    while (--right_pos) {
        right = right->next;
    }




    // 反转前面的length/2


    struct ListNode *node_next = head->next;
    struct ListNode *node_prev = NULL;
    struct ListNode *current = head;


    while (current != left) {
        current->next = node_prev;
        node_prev = current;
        current = node_next;
        node_next = node_next->next;
    }
    current->next = node_prev;

    // 进行比较

    while (compare_count--) {
        if (left->val != right->val) {
            result = false;
            break;
        }
        left = left->next;
        right = right->next;
    }

    // 左面反转回来
    while (node_prev != NULL) {
        current->next = node_next;
        node_next = current;
        current = node_prev;
        node_prev = node_prev->next;
    }

    current->next = NULL;


    return result;
}
```

​	
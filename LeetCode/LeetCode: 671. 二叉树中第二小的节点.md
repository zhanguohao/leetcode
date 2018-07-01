# LeetCode: 671. 二叉树中第二小的节点

[TOC]



## 1、题目描述

给定一个非空特殊的二叉树，每个节点都是正数，并且每个节点的子节点数量只能为 `2` 或 `0`。如果一个节点有两个子节点的话，那么这个节点的值不大于它的子节点的值。 

给出这样的一个二叉树，你需要输出所有节点中的**第二小的值。**如果第二小的值不存在的话，输出 -1 **。**

**示例 1:**

```
输入: 
    2
   / \
  2   5
     / \
    5   7

输出: 5
说明: 最小的值是 2 ，第二小的值是 5 。
```

**示例 2:**

```
输入: 
    2
   / \
  2   2

输出: -1
说明: 最小的值是 2, 但是不存在第二小的值。
```



## 2、解题思路

​	注意，这里是第二小，不是第二大。。。。



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

void find_Second(struct TreeNode *root, int *max_min, int *second) {
    if (!root) {
        return;
    }

    if (root->val < *max_min) {
        *second = *max_min;
        *max_min = root->val;
    } else if (root->val > *max_min){
        if (root->val < *second) {
            *second = root->val;
        }
    }

    find_Second(root->left, max_min, second);
    find_Second(root->right, max_min, second);
}


int findSecondMinimumValue(struct TreeNode *root) {
    if (!root) {
        return -1;
    }

    int max_min = INT32_MAX;
    int second = INT32_MAX;
    find_Second(root, &max_min, &second);
    
    if(second == INT32_MAX){
        return -1;
    }
    return second;

}
```


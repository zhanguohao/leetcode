# LeetCode: 104. 二叉树的最大深度

[TOC]

## 1、题目描述

给定一个二叉树，找出其最大深度。

二叉树的深度为根节点到最远叶子节点的最长路径上的节点数。

**说明:** 叶子节点是指没有子节点的节点。

**示例：**
给定二叉树 `[3,9,20,null,null,15,7]`，

```
    3
   / \
  9  20
    /  \
   15   7
```

返回它的最大深度 3 。



## 2、解题思路

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int maxDepth(struct TreeNode* root) {
    if (!root) {
        return 0;
    }
    int left = 0;
    int right = 0;
    if (root->left) {
        left = maxDepth(root->left);
    }
    if (root->right) {
        right = maxDepth(root->right);
    }
    return left > right ? left + 1 : right + 1;
}
```


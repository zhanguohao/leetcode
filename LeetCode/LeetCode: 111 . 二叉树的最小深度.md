# LeetCode: 111 . 二叉树的最小深度

[TOC]

## 1、题目描述

```
给定一个二叉树，找出其最小深度。

最小深度是从根节点到最近叶子节点的最短路径上的节点数量。

说明: 叶子节点是指没有子节点的节点。

示例:

给定二叉树 [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回它的最小深度  2.
```



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
int minDepth(struct TreeNode* root) {
    if (!root) {
        return 0;
    }
    int left = 0;
    int right = 0;
    int result = 0;
    if (root->left) {
        left = minDepth(root->left);
        result = left+1;
    }else{
        result = left = INT_MAX;
    }
    if (root->right) {
        right = minDepth(root->right);
        result = result > right+1 ? right + 1 : result;
    }
    if(result == INT_MAX){
        result = 1;
    }
    
    return result;
}
```


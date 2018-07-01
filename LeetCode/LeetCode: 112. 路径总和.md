# LeetCode: 112. 路径总和

[TOC]

## 1、题目描述

给定一个二叉树和一个目标和，判断该树中是否存在根节点到叶子节点的路径，这条路径上所有节点值相加等于目标和。

**说明:** 叶子节点是指没有子节点的节点。

**示例:** 
给定如下二叉树，以及目标和 `sum = 22`，

```
              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
```

返回 `true`, 因为存在目标和为 22 的根节点到叶子节点的路径 `5->4->11->2`。



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
bool hasPathSum(struct TreeNode* root, int sum) {
    if (!root) {
        return false;
    }
    int sub_num = sum - root->val;
    bool left_path = false;
    bool right_path = false;

    if (root->left) {
        left_path = hasPathSum(root->left, sub_num);
    }
    if (root->right) {
        right_path = hasPathSum(root->right, sub_num);
    }

    if (!root->left && !root->right) {
        if (sub_num == 0) {
            return true;
        }else{
            return false;
        }
    } else {
        return left_path || right_path;
    }
}
```


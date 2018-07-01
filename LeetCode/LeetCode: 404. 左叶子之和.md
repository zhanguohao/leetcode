# LeetCode: 404. 左叶子之和

[TOC]





## 1、题目描述



计算给定二叉树的所有左叶子之和。

**示例：**

```
    3
   / \
  9  20
    /  \
   15   7

在这个二叉树中，有两个左叶子，分别是 9 和 15，所以返回 24
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
int sumOfLeftLeaves(struct TreeNode* root) {
    
     if (!root) {
        return 0;
    }

    int result = 0;
    if (root->left) {
        if (!root->left->left && !root->left->right) {

            result += root->left->val + sumOfLeftLeaves(root->left);
        } else {
            result += sumOfLeftLeaves(root->left);
        }
    }

    if (root->right) {
        result += sumOfLeftLeaves((root->right));
    }

    return result;
    
}
```


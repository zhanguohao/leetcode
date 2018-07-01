# LeetCode: 669. 修剪二叉搜索树

[TOC]



## 1、题目描述



给定一个二叉搜索树，同时给定最小边界`L` 和最大边界 `R`。通过修剪二叉搜索树，使得所有节点的值在`[L, R]`中 (R>=L) 。你可能需要改变树的根节点，所以结果应当返回修剪好的二叉搜索树的新的根节点。

**示例 1:**

```
输入: 
    1
   / \
  0   2

  L = 1
  R = 2

输出: 
    1
      \
       2
```

**示例 2:**

```
输入: 
    3
   / \
  0   4
   \
    2
   /
  1

  L = 1
  R = 3

输出: 
      3
     / 
   2   
  /
 1
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
struct TreeNode* trimBST(struct TreeNode* root, int L, int R) {
    if (!root) {
        return NULL;
    }

    if (root->val >= L && root->val <= R) {
        root->left = trimBST(root->left, L, R);
        root->right = trimBST(root->right, L, R);
    } else if (root->val > R) {
        root = trimBST(root->left, L, R);
    } else {
        root = trimBST(root->right, L, R);
    }

    return root;
}
```


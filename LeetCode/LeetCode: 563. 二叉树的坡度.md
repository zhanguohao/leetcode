# LeetCode: 563. 二叉树的坡度

[TOC]

## 1、题目描述



给定一个二叉树，计算**整个树**的坡度。

一个树的**节点的坡度**定义即为，该节点左子树的结点之和和右子树结点之和的**差的绝对值**。空结点的的坡度是0。

**整个树**的坡度就是其所有节点的坡度之和。

**示例:**

```
输入: 
         1
       /   \
      2     3
输出: 1
解释: 
结点的坡度 2 : 0
结点的坡度 3 : 0
结点的坡度 1 : |2-3| = 1
树的坡度 : 0 + 0 + 1 = 1
```

**注意:**

1. 任何子树的结点的和不会超过32位整数的范围。
2. 坡度的值不会超过32位整数的范围。

## 2、解题思路

​	

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int getTreeSum(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    return root->val + getTreeSum(root->left) + getTreeSum(root->right);
}


int findTilt(struct TreeNode *root) {
    if (!root || !root->right && !root->left) {
        return 0;
    }


    return abs(getTreeSum(root->left) - getTreeSum(root->right)) + findTilt(root->left) + findTilt(root->right);


}
```


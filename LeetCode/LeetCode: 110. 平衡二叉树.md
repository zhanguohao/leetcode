# LeetCode: 110. 平衡二叉树

[TOC]

## 1、题目描述



给定一个二叉树，判断它是否是高度平衡的二叉树。

本题中，一棵高度平衡二叉树定义为：

> 一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过1。

**示例 1:**

给定二叉树 `[3,9,20,null,null,15,7]`

```
    3
   / \
  9  20
    /  \
   15   7
```

返回 `true` 。
**示例 2:**

给定二叉树 `[1,2,2,3,3,null,null,4,4]`



## 2、解题思路

​	判断左右子树的深度是多少，如果左右子树是平衡树，并且左右子树的高度差不超过1，返回真



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int getDepth(struct TreeNode* root) {
    if (!root) {
        return 0;
    }
    int left = 0;
    int right = 0;
    if (root->left) {
        left = getDepth(root->left);
    }
    if (root->right) {
        right = getDepth(root->right);
    }
    return left > right ? left + 1 : right + 1;
}

bool isBalanced(struct TreeNode* root) {
    if(!root){
        return true;
    }
    bool result = false;
    int left = 0;
    int right = 0;
    if(root->left){
        left = getDepth(root->left);
    }
    if (root->right){
        right = getDepth(root->right);
    }

    if ((left-right >= -1) && (left-right <= 1)){
        result = true;
    }
    result = result && isBalanced(root->left) && isBalanced(root->right);

    return result;


}
```


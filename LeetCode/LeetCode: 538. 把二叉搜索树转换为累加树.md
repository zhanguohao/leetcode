# LeetCode: 538. 把二叉搜索树转换为累加树

[TOC]

## 1、题目描述





给定一个二叉搜索树（Binary Search Tree），把它转换成为累加树（Greater Tree)，使得每个节点的值是原来的节点值加上所有大于它的节点值之和。

**例如：**

```
输入: 二叉搜索树:
              5
            /   \
           2     13

输出: 转换为累加树:
             18
            /   \
          20     13
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
int getTreeSum(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    return root->val + getTreeSum(root->left) + getTreeSum(root->right);
}


void convert_BST(struct TreeNode *root, int father) {
    if (!root) {
        return;
    }

    root->val =root->val +  father + getTreeSum(root->right);
    convert_BST(root->left, root->val);
    convert_BST(root->right, father);
    
    

}

struct TreeNode *convertBST(struct TreeNode *root) {

    convert_BST(root, 0);
    return root;
}

```





​	
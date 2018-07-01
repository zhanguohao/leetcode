# LeetCode: 653. 两数之和 IV - 输入 BST

[TOC]

## 1、题目描述





给定一个二叉搜索树和一个目标结果，如果 BST 中存在两个元素且它们的和等于给定的目标结果，则返回 true。

**案例 1:**

```
输入: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 9

输出: True
```

 

**案例 2:**

```
输入: 
    5
   / \
  3   6
 / \   \
2   4   7

Target = 28

输出: False
```

 

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

bool findValue(struct TreeNode *root, int value, struct TreeNode *current) {

    if (!root) {
        return false;
    }

    // 不是当前节点，并且值是要找的值
    // 规避了两倍的情况
    if (current != root && root->val == value) {
        return true;
    }

    return findValue(root->left, value, current) || findValue(root->right, value, current);
}

bool find_Target(struct TreeNode *current, struct TreeNode *root, int k) {
    if (!current) {
        return false;
    }
    bool result = false;
    int tar = k - current->val;

    result = result || findValue(root, tar, current) || findValue(root, tar, current);
    

    return result || find_Target(current->left, root, k) || find_Target(current->right, root, k);
}

bool findTarget(struct TreeNode *root, int k) {
    if (!root) {
        return false;
    }

    return find_Target(root, root, k);
}
```


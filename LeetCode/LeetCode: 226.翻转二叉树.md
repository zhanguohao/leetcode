# LeetCode: 226.翻转二叉树

[TOC]



## 1、题目描述



翻转一棵二叉树。

**示例：**

输入：

```
     4
   /   \
  2     7
 / \   / \
1   3 6   9
```

输出：

```
     4
   /   \
  7     2
 / \   / \
9   6 3   1
```

**备注:**
这个问题是受到 [Max Howell ](https://twitter.com/mxcl)的 [原问题](https://twitter.com/mxcl/status/608682016205344768) 启发的 ：

> 谷歌：我们90％的工程师使用您编写的软件(Homebrew)，但是您却无法在面试时在白板上写出翻转二叉树这道题，这太糟糕了。





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
struct TreeNode* invertTree(struct TreeNode* root) {
    if (!root) {
        return NULL;
    }

    struct TreeNode *temp;
    if (root->left || root->right) {
        temp = root->left;
        root->left = root->right;
        root->right = temp;
    }

    invertTree(root->left);
    invertTree(root->right);
    return root;
}
```






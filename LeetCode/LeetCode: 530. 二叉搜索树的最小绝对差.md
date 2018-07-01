# LeetCode: 530. 二叉搜索树的最小绝对差

[TOC]



## 1、题目描述

给定一个所有节点为非负值的二叉搜索树，求树中任意两节点的差的绝对值的最小值。

**示例 :**

```
输入:

   1
    \
     3
    /
   2

输出:
1

解释:
最小绝对差为1，其中 2 和 1 的差的绝对值为 1（或者 2 和 3）。
```

**注意:** 树中至少有2个节点。



## 2、解题思路

​	实际上，因为是二叉搜索树，使用一下中序遍历就可以了

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


void in_order(struct TreeNode *root, int *prev, int *abs_max) {
    if (!root) {
        return;
    }
    in_order(root->left, prev, abs_max);

    if (*prev != -1) {
        *abs_max = *abs_max < root->val - *prev ? *abs_max : root->val - *prev;
        *prev = root->val;
    } else {
        *prev = root->val;
    }

    in_order(root->right, prev, abs_max);

}


int getMinimumDifference(struct TreeNode *root) {
    int prev = -1;
    int abs_max = INT32_MAX;
    in_order(root, &prev, &abs_max);
    return abs_max;
}
```





一开始，没有使用传参的方式，使用全局变量，报错，需要注意




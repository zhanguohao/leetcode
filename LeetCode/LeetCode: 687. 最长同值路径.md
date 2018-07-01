# LeetCode: 687. 最长同值路径

[TOC]



## 1、题目描述





给定一个二叉树，找到最长的路径，这个路径中的每个节点具有相同值。 这条路径可以经过也可以不经过根节点。

**注意**：两个节点之间的路径长度由它们之间的边数表示。

**示例 1:**

输入:

```
              5
             / \
            4   5
           / \   \
          1   1   5
```

输出:

```
2
```

**示例 2:**

输入:

```
              1
             / \
            4   5
           / \   \
          4   4   5
```

输出:

```
2
```

**注意:** 给定的二叉树不超过10000个结点。 树的高度不超过1000。





## 2、解题思路



​	设计一个递归函数，返回的值是当前路径下左右节点中最长的路径

​	对每个节点来讲，经过这个节点的路径，是左右最长路径之和

​	注意一点就是，什么时候不相等，就从这个点重新计算最大值

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int longPath(struct TreeNode *root, int *max) {
    int left = 0;
    int right = 0;


    if (root->left && root->val == root->left->val) {
        left = 1 + longPath(root->left, max);
    } else if (root->left) {
        longPath(root->left, max);
    }


    if (root->right && root->val == root->right->val) {
        right = 1 + longPath(root->right, max);
    } else if (root->right) {
        longPath(root->right, max);
    }

    if (left + right > *max) {
        *max = left + right;
    }


    return left > right ? left : right;


}


int longestUnivaluePath(struct TreeNode *root) {
    if (!root) {
        return 0;
    }

    int max = 0;

    longPath(root, &max);

    return max;


}
```



​	
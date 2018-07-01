# LeetCode: 101. 对称二叉树

[TOC]

## 1、题目描述



给定一个二叉树，检查它是否是镜像对称的。

例如，二叉树 `[1,2,2,3,4,4,3]` 是对称的。

```
    1
   / \
  2   2
 / \ / \
3  4 4  3
```

但是下面这个 `[1,2,2,null,3,null,3]` 则不是镜像对称的:

```
    1
   / \
  2   2
   \   \
   3    3
```

**说明:**

如果你可以运用递归和迭代两种方法解决这个问题，会很加分。



## 2、 解题思路

### 2.1 DFS

​	深度优先搜索

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
bool isSymmetric(struct TreeNode* root) {
     // 判断终止条件
    if (!root) {
        return true;

    }
    // 左右节点都不存在
    if (!root->left && !root->right) {
        return true;
    }
    // 左节点存在，右节点不存在
    // 左节点不存在，右节点存在
    // 左右节点存在，但是值不相等
    if (root->left && !root->right || !root->left && root->right || root->left->val != root->right->val) {
        return false;
    }
    bool result = true;
    struct TreeNode *temp = (struct TreeNode *) malloc(sizeof(struct TreeNode));
    temp->left = root->left->left;
    temp->right = root->right->right;

    result &= isSymmetric(temp);
    temp->left = root->left->right;
    temp->right = root->right->left;
    result &= isSymmetric(temp);
    return result;

}
```



​	换一个思路

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
bool is_Symmetric(struct TreeNode *left, struct TreeNode *right) {
    if (!left && !right) {
        return true;
    } else if (!left || !right) {
        return false;
    } else {
        return (left->val == right->val) && is_Symmetric(left->left, right->right) &&
               is_Symmetric(left->right, right->left);
    }


}

bool isSymmetric(struct TreeNode *root) {
    if (!root) {
        return true;
    }
    return is_Symmetric(root->left, root->right);

}
```







### 2.2 BFS

​	广度优先搜索

```

```


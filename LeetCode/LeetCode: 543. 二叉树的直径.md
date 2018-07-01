# LeetCode: 543. 二叉树的直径

[TOC]



## 1、题目描述





给定一棵二叉树，你需要计算它的直径长度。一棵二叉树的直径长度是任意两个结点路径长度中的最大值。这条路径可能穿过根结点。

**示例 :**
给定二叉树

```
          1
         / \
        2   3
       / \     
      4   5    
```

返回 **3**, 它的长度是路径 [4,2,1,3] 或者 [5,2,1,3]。

**注意：**两结点之间的路径长度是以它们之间边的数目表示。





## 2、解题思路





​	设计一个函数，找出当前节点中最长路径

​	再用一个函数，找出经过当前节点的最长路径，实际上就是利用上面的函数，得到左子树加上右子树的最长路径，再加上2

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


// 找到经过当前节点的最大的路径长度
int getMaxdiameter(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    int left = 0;
    int right = 0;
    if (root->left) {
        left += 1 + getMaxdiameter(root->left);
    }

    if (root->right) {
        right += 1 + getMaxdiameter(root->right);

    }

    return left > right ? left : right;
}

void getMax(struct TreeNode *root, int *max_length) {
    int result = 0;
    if (root->left) {
        result = 1 + getMaxdiameter(root->left);
        getMax(root->left, max_length);
    }
    if (root->right) {
        result += 1 + getMaxdiameter(root->right);
        getMax(root->right, max_length);
    }

    if (result > *max_length) {
        *max_length = result;
    }

}


int diameterOfBinaryTree(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    int max_length = 0;
    getMax(root, &max_length);
    return max_length;
}
```


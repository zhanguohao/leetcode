# LeetCode: 783. 二叉搜索树结点最小距离

[TOC]



## 1、题目描述





给定一个二叉搜索树的根结点 `root`, 返回树中任意两节点的差的最小值。

**示例：**

```
输入: root = [4,2,6,1,3,null,null]
输出: 1
解释:
注意，root是树结点对象(TreeNode object)，而不是数组。

给定的树 [4,2,6,1,3,null,null] 可表示为下图:

          4
        /   \
      2      6
     / \    
    1   3  

最小的差值是 1, 它是节点1和节点2的差值, 也是节点3和节点2的差值。
```

**注意：**

1. 二叉树的大小范围在 `2` 到 `100`。
2. 二叉树总是有效的，每个节点的值都是整数，且不重复。



## 2、解题思路

​	实际上这是一个中序遍历，很简单就出来了



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */


void dfs(struct TreeNode *root, int *min_value, int *prev_value) {
    if (!root) {
        return;
    }

    dfs(root->left, min_value, prev_value);
    if (*prev_value != -1) {
        *min_value = *min_value < root->val - *prev_value ? *min_value : root->val - *prev_value;
    }
    *prev_value = root->val;

    dfs(root->right, min_value, prev_value);


}

int minDiffInBST(struct TreeNode *root) {

    int min_value = INT32_MAX;
    int prev_value = -1;
    dfs(root, &min_value, &prev_value);
    return min_value;

}


```


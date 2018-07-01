# LeetCode: 108. 将有序数组转换为二叉搜索树

[TOC]

## 1、题目描述



将一个按照升序排列的有序数组，转换为一棵高度平衡二叉搜索树。

本题中，一个高度平衡二叉树是指一个二叉树*每个节点* 的左右两个子树的高度差的绝对值不超过 1。

**示例:**

```
给定有序数组: [-10,-3,0,5,9],

一个可能的答案是：[0,-3,9,-10,null,5]，它可以表示下面这个高度平衡二叉搜索树：

      0
     / \
   -3   9
   /   /
 -10  5
```

------



## 2、解题思路

### 2.1 递归法

​	判断当前节点是不是空，是则返回NULL

​	构造一个节点，使用中间的元素做为value

​	然后递归求解左子树和右子树

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
struct TreeNode* sortedArrayToBST(int* nums, int numsSize) {
    if (numsSize == 0) {
        return NULL;
    }

    struct TreeNode *result = (struct TreeNode *) malloc(sizeof(struct TreeNode));

    result->val = nums[numsSize / 2];
    if (numsSize / 2 == 0) {
        result->left = NULL;
    } else {
        result->left = sortedArrayToBST(nums, numsSize / 2);
    }
    // 已经到了最右面
    if (numsSize / 2 >= (numsSize - 1)) {
        result->right = NULL;
    } else {
        result->right = sortedArrayToBST(&nums[numsSize / 2 + 1], numsSize - numsSize / 2 - 1);
    }

    return result;
}
```


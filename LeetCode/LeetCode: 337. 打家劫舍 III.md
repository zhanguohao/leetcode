# LeetCode: 337. 打家劫舍 III

[TOC]



## 1、题目描述

小偷又发现一个新的可行窃的地点。 这个地区只有一个入口，称为“根”。 除了根部之外，每栋房子有且只有一个父房子。 一番侦察之后，聪明的小偷意识到“这个地方的所有房屋形成了一棵二叉树”。 如果两个直接相连的房子在同一天晚上被打劫，房屋将自动报警。

在不触动警报的情况下，计算小偷一晚能盗取的最高金额。

**示例 1:**

```
     3
    / \
   2   3
    \   \ 
     3   1
```

能盗取的最高金额 = 3 + 3 + 1 = **7**.

**示例 2:**

```
     3
    / \
   4   5
  / \   \ 
 1   3   1
```

能盗取的最高金额 = 4 + 5 = **9**.

**致谢:**
特别感谢 [@dietpepsi](https://leetcode.com/discuss/user/dietpepsi) 添加此题并创建所有测试用例。



## 2、解题思路

​	使用深度优先搜索，我们针对每一层，都需要这样判断

- 偷当前层，那么就是当前层的值，加上左子树的



​	下面的这个超时了，换个方式重新写

```python
class Solution:
    def rob(self, root):
        """
        :type root: TreeNode
        :rtype: int
        """

        if not root:
            return 0

        sum0 = root.val

        sum1 = self.rob(root.left) + self.rob(root.right)

        if root.left:
            sum0 += self.rob(root.left.left) + self.rob(root.left.right)

        if root.right:
            sum0 += self.rob(root.right.left) + self.rob(root.right.right)

        return max(sum0, sum1)

```

​	

​	嘿嘿嘿，使用大招，用C写一遍

```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
int rob(struct TreeNode* root) {
    if (root == NULL) {
        return 0;
    }

    int sum0 = root->val;

    int sum1 = 0;

    sum1 += rob(root->left) + rob(root->right);

    if (root->left != NULL) {
        sum0 += rob(root->left->left) + rob(root->left->right);
    }

    if (root->right != NULL) {
        sum0 += rob(root->right->left) + rob(root->right->right);
    }

    return sum0 > sum1 ? sum0 : sum1;
}
```

​	通过了，不过耗费时间太长了，学习一下别人的思路



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */

int dfs(struct TreeNode *root, int *l, int *r) {
    if (!root) {
        return 0;

    }
    int ll = 0;
    int lr = 0;
    int rl = 0;
    int rr = 0;

    *l = dfs(root->left, &ll, &lr);
    *r = dfs(root->right, &rl, &rr);

    int sum0 = root->val + ll + lr + rl + rr;
    int sum1 = *l + *r;

    return sum0 > sum1 ? sum0 : sum1;
}
int rob(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    int l = 0;
    int r = 0;
    return dfs(root, &l, &r);

}
```

​	思路差不多，分析一下区别在哪里，主要是减少了判断的情况



基本思路就是，对当前节点而言，当前节点的值，加上下下层的值

然后与下一层进行判断

这与直接的打家劫舍那道题就差不多了
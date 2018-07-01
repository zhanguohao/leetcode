# LeetCode: 572. 另一个树的子树

[TOC]



## 1、题目描述



给定两个非空二叉树 **s** 和 **t**，检验 **s** 中是否包含和 **t** 具有相同结构和节点值的子树。**s** 的一个子树包括 **s** 的一个节点和这个节点的所有子孙。**s** 也可以看做它自身的一棵子树。

**示例 1:**
给定的树 s:

```
     3
    / \
   4   5
  / \
 1   2
```

给定的树 t：

```
   4 
  / \
 1   2
```

返回 **true**，因为 t 与 s 的一个子树拥有相同的结构和节点值。

**示例 2:**
给定的树 s：

```
     3
    / \
   4   5
  / \
 1   2
    /
   0
```

给定的树 t：

```
   4
  / \
 1   2
```

返回 **false**。



## 2、解题思路

​	

```c
bool isEqual(struct TreeNode *s, struct TreeNode *t) {
    if (!s && !t) {
        return true;
    } else if (!s && t || s && !t || s->val != t->val) {
        return false;
    }

    return isEqual(s->left, t->left) && isEqual(s->right, t->right);
}


bool isSubtree(struct TreeNode *s, struct TreeNode *t) {
    if (!s && !t) {
        return true;
    }

    bool result = false;
    result = result || isEqual(s, t);
    if (s->left) {
        result = result || isSubtree(s->left, t);
    }

    if (s->right) {
        result = result || isSubtree(s->right, t);
    }

    return result;

}
```


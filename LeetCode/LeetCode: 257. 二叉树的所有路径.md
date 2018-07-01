# LeetCode: 257. 二叉树的所有路径

[TOC]

## 1、题目描述

给定一个二叉树，返回所有从根节点到叶子节点的路径。

**说明:** 叶子节点是指没有子节点的节点。

**示例:**

```
输入:

   1
 /   \
2     3
 \
  5

输出: ["1->2->5", "1->3"]

解释: 所有根节点到叶子节点的路径为: 1->2->5, 1->3
```







## 2、解题思路

​	递归查找

​	如果有子节点，就继续向下找	

​	如果是叶子节点，构造字符串，返回

​	不是叶子节点，将左右子节点的所有的字符串重新构造，向上返回



```c
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     struct TreeNode *left;
 *     struct TreeNode *right;
 * };
 */
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */



const char *direction = "->";

char *numToString(int n) {
    int length = 1;
    int temp = n;
    while (temp > 0) {
        temp /= 10;
        length++;
    }

    char *result = (char *) malloc(sizeof(char) * (length + 1));
    sprintf(result, "%d", n);
    
    return result;
}


void freeBuff(char **buf, int size) {
    for (int i = 0; i < size; i++) {
        free(buf[i]);
    }
}

char **binaryTreePath(struct TreeNode *root, int level, int *returnSize) {

    char **result;
    // 没有左右节点

    // 当前节点的值
    char *num = numToString(root->val);

    // 如果是根节点
    if (root->left == NULL && root->right == NULL) {
        result = (char **) malloc(sizeof(char *));
        if (level == 0) {
            *result = num;
        } else {
            *result = (char *) malloc(sizeof(char) * (strlen(num) + 3));
            (*result)[0] = '\0';
            strcat(*result, direction);
            strcat(*result, num);
            free(num);
        }
        
        *returnSize = 1;
        return result;

    }


    // 有左右节点，不是根节点
    char **left;
    char **right;
    int left_size = 0;
    int right_size = 0;

    if (root->left) {
        left = binaryTreePath(root->left, level + 1, returnSize);
        left_size = *returnSize;
    }

    if (root->right) {
        right = binaryTreePath(root->right, level + 1, returnSize);
        right_size = *returnSize;
    }

    *returnSize = left_size + right_size;

    result = (char **) malloc(sizeof(char *) * (left_size + right_size));


    if (level == 0) {

        for (int i = 0; i < left_size; i++) {
            result[i] = (char *) malloc(sizeof(char) * (strlen(left[i]) + strlen(num) + 1));
            result[i][0] = 0;
            strcat(result[i], num);
            strcat(result[i], left[i]);

        }
        for (int i = 0; i < right_size; i++) {
            result[i + left_size] = (char *) malloc(sizeof(char) * (strlen(right[i]) + strlen(num) + 1));
            result[i + left_size][0] = 0;
            strcat(result[i + left_size], num);
            strcat(result[i + left_size], right[i]);
        }


    } else {
        for (int i = 0; i < left_size; i++) {
            result[i] = (char *) malloc(sizeof(char) * (strlen(left[i]) + strlen(num) + 3));
            result[i][0] = 0;
            strcat(result[i], direction);
            strcat(result[i], num);
            strcat(result[i], left[i]);
        }
        for (int i = 0; i < right_size; i++) {
            result[i + left_size] = (char *) malloc(sizeof(char) * (strlen(right[i]) + strlen(num) + 3));
            result[i + left_size][0] = 0;
            strcat(result[i + left_size], direction);
            strcat(result[i + left_size], num);
            strcat(result[i + left_size], right[i]);

        }
    }

    free(num);
    freeBuff(left, left_size);
    freeBuff(right, right_size);
    return result;

}

char **binaryTreePaths(struct TreeNode *root, int *returnSize) {

    if (!root) {
        *returnSize = 0;
        return NULL;
    }

    return binaryTreePath(root, 0, returnSize);

}



```


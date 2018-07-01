# LeetCode: 107. 二叉树的层次遍历 II

[TOC]



给定一个二叉树，返回其节点值自底向上的层次遍历。 （即按从叶子节点所在层到根节点所在的层，逐层从左向右遍历）

例如：
给定二叉树 `[3,9,20,null,null,15,7]`,

```
    3
   / \
  9  20
    /  \
   15   7
```

返回其自底向上的层次遍历为：

```
[
  [15,7],
  [9,20],
  [3]
]
```



## 2、解题思路

​	首先判断是不是空，为空直接返回

​	然后创建一个队列，将根节点放到队列中

​	然后判断烈烈中有几个元素，就是当前层次的节点数

​	接着，依次取出队列中的元素，并且将其子节点加入到队列中

​	如果队列为空，表示已经便利完成	

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> list = new ArrayList<>();  
    if(root == null){  
        return list;  
    }  
    Queue<TreeNode> queue = new ArrayDeque<>();  
    queue.offer(root);  
    while(!queue.isEmpty()){  
        //获取当前层的节点数  
        int levelNum = queue.size();  
        List<Integer> subList = new ArrayList<>();  
        for (int i = 0; i < levelNum; i++) {  
            TreeNode node = queue.poll();  
            subList.add(node.val);  
            if(node.left != null){  
                queue.offer(node.left);  
            }  
            if(node.right != null){  
                queue.offer(node.right);  
            }  
        }  
        list.add(0,subList);  
    }  
    return list;  
    }
}
```



​	从根节点开始，判断当前的层次是不是在数组中，如果不在，new一个

​	然后将当前值加入对应的层次中去

​	不断递归

​	最后将集合逆序返回

```java
class Solution {  
    public List<List<Integer>> levelOrderBottom(TreeNode root) {  
        List<List<Integer>> list=new ArrayList<List<Integer>>();  
        addLevel(list,0,root);  
        Collections.reverse(list);  
        return list;  
    }  
    public void addLevel(List<List<Integer>> list, int level, TreeNode node){  
        if(node==null)return ;  
        if(list.size()-1<level)list.add(new ArrayList<Integer>());  
        list.get(level).add(node.val);  
          
        addLevel(list,level+1,node.left);  
        addLevel(list,level+1,node.right);  
    }  
}   
```



C语言实现

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
 * Return an array of arrays of size *returnSize.
 * The sizes of the arrays are returned as *columnSizes array.
 * Note: Both returned array and *columnSizes array must be malloced, assume caller calls free().
 */

int maxDepth(struct TreeNode *root) {
    if (!root) {
        return 0;
    }
    int left = 0;
    int right = 0;
    if (root->left) {
        left = maxDepth(root->left);
    }
    if (root->right) {
        right = maxDepth(root->right);
    }
    return left > right ? left + 1 : right + 1;
}

void init_pointer(struct TreeNode **buf, int size) {
    for (int i = 0; i < size; i++) {
        buf[i] = NULL;
    }
}

int **levelOrderBottom(struct TreeNode *root, int **columnSizes, int *returnSize) {
    if (!root) {
        return NULL;
    }
    *returnSize = maxDepth(root);
    int **return_array = (int **) malloc(sizeof(int *) * (*returnSize));
    *columnSizes = (int *) malloc(sizeof(int) * (*returnSize));
    int line_nums = 1;
    int line_nums_child = 0;
    int buf_size = 1;
    // 用buf_array存储每一行所有的元素的指针
    struct TreeNode **buf_array_father = (struct TreeNode **) malloc(sizeof(struct TreeNode *) * 2);
    struct TreeNode **buf_array_child = (struct TreeNode **) malloc(sizeof(struct TreeNode *) * 3);
    init_pointer(buf_array_father, 2);
    init_pointer(buf_array_child, 3);

    buf_array_father[0] = root;
    int *cur_line;

    struct TreeNode **temp;


    for (int line_count = 0; line_count < *returnSize; line_count++) {
        cur_line = (int *) malloc(sizeof(int) * line_nums);
        return_array[line_count] = cur_line;
        // 存放每一行的元素个数
        int *cur_column = (*columnSizes) + line_count;
        *cur_column = 0;

        temp = buf_array_father;
        // 将下一行放到缓冲里面
        while (*buf_array_father) {
            if ((*buf_array_father)->left) {
                buf_array_child[line_nums_child++] = (*buf_array_father)->left;
            }
            if ((*buf_array_father)->right) {
                buf_array_child[line_nums_child++] = (*buf_array_father)->right;
            }
            cur_line[(*cur_column)++] = (*buf_array_father)->val;
            buf_array_father++;
        }

        free(temp);
        buf_array_father = buf_array_child;
        buf_array_child = (struct TreeNode **) malloc(sizeof(struct TreeNode *) * (line_nums_child * 2 + 1));
        init_pointer(buf_array_child, (line_nums_child * 2 + 1));

        line_nums = line_nums_child;
        line_nums_child = 0;

    }
    free(buf_array_father);
    free(buf_array_child);
    
    int *temp_reverse;
    int size_temp;
    for (int i = 0; i < ((*returnSize) / 2); i++) {
        temp_reverse = return_array[i];
        return_array[i] = return_array[(*returnSize) - 1 - i];
        return_array[(*returnSize) - 1 - i] = temp_reverse;

        size_temp = *((*columnSizes) + i);
        *((*columnSizes) + i) = *((*columnSizes) + (*returnSize) - 1 - i);
        *((*columnSizes) + (*returnSize) - 1 - i) = size_temp;
    }

    return return_array;


}
```


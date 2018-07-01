# LeetCode: 217. 存在重复元素

[TOC]

## 1、题目描述



给定一个整数数组，判断是否存在重复元素。

如果任何值在数组中出现至少两次，函数返回 true。如果数组中每个元素都不相同，则返回 false。

**示例 1:**

```
输入: [1,2,3,1]
输出: true
```

**示例 2:**

```
输入: [1,2,3,4]
输出: false
```

**示例 3:**

```
输入: [1,1,1,3,3,4,3,2,4,2]
输出: true
```



## 2、解题思路

### 2.1 异或

​	还记得前面求解众数的题目，使用了异或的思想，这里也可以使用，如果异或以后，与前一个值相等，表示找到了相等的值

​	这个使用了双重循环，很慢

```c
bool containsDuplicate(int *nums, int numsSize) {


    for (int i = 0; i < numsSize; i++) {
        for (int j = i + 1; j < numsSize; j++) {
            if ((nums[i] ^ nums[j] )== 0) {
                return true;
            }
        }
    }
    return false;
}
```



### 2.2 排序法

​	先排序，在查找一遍

```c
int compare(int *a, int *b) {
    return *a - *b;
}

bool containsDuplicate(int *nums, int numsSize) {
    if (!nums || numsSize <= 1) {
        return false;
    }

    qsort(nums, numsSize, sizeof(int), compare);

    for (int i = 0; i < numsSize - 1; i++) {
        if(nums[i] == nums[i+1]){
            return true;
        }
    }
    return false;

}
```



### 2.3 哈希表法

​	将数字放到哈希表中，每个数字判断一次，如果存在，表示





### 2.4 去重长度比较

​	这个用python实现，非常简单

```python
class Solution:
    def containsDuplicate(self, nums):
        """
        :type nums: List[int]
        :rtype: bool
        """
        return len(nums) != len(set(nums))
```

​	只有一句话，但是效率不算很高，但是比起双重循环好多了


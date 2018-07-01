# LeetCode: 506. 相对名次

[TOC]

## 1、题目描述



给出 **N** 名运动员的成绩，找出他们的相对名次并授予前三名对应的奖牌。前三名运动员将会被分别授予 “金牌”，“银牌” 和“ 铜牌”（"Gold Medal", "Silver Medal", "Bronze Medal"）。

(注：分数越高的选手，排名越靠前。)

**示例 1:**

```
输入: [5, 4, 3, 2, 1]
输出: ["Gold Medal", "Silver Medal", "Bronze Medal", "4", "5"]
解释: 前三名运动员的成绩为前三高的，因此将会分别被授予 “金牌”，“银牌”和“铜牌” ("Gold Medal", "Silver Medal" and "Bronze Medal").
余下的两名运动员，我们只需要通过他们的成绩计算将其相对名次即可。
```

**提示:**

1. N 是一个正整数并且不会超过 10000。
2. 所有运动员的成绩都不相同。

#  



## 2、解题思路



```c
/**
 * Return an array of size *returnSize.
 * Note: The returned array must be malloced, assume caller calls free().
 */


char *numToString(int num) {
    int digits = 1;

    int temp = num;
    while (temp / 10) {
        digits++;
        temp /= 10;
    }

    char *result = (char *) calloc(digits + 1, sizeof(char));

    sprintf(result, "%d", num);

    return result;

}

int cmp(int *a, int *b) {
    return *b - *a;
}


char **findRelativeRanks(int *nums, int numsSize, int *returnSize) {
    int *sort_num = (int *) malloc(sizeof(int) * numsSize);
    memcpy(sort_num, nums, sizeof(int) * numsSize);

    qsort(sort_num, numsSize, sizeof(int), cmp);

    char **result = (char **) malloc(sizeof(char *) * numsSize);
    for (int i = 0; i < numsSize; i++) {
        for (int j = 0; j < numsSize; j++) {
            if (nums[j] == sort_num[i]) {
                if (i == 0) {
                    result[j] = "Gold Medal";
                } else if (i == 1) {
                    result[j] = "Silver Medal";
                } else if (i == 2) {
                    result[j] = "Bronze Medal";
                } else {
                    result[j] = numToString(i+1);
                }
            break;
            }
        }
    }

    *returnSize = numsSize;
    return result;

}
```


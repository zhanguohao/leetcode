# LeetCode: 599. 两个列表的最小索引总和

[TOC]

## 1、题目描述





假设Andy和Doris想在晚餐时选择一家餐厅，并且他们都有一个表示最喜爱餐厅的列表，每个餐厅的名字用字符串表示。

你需要帮助他们用**最少的索引和**找出他们**共同喜爱的餐厅**。 如果答案不止一个，则输出所有答案并且不考虑顺序。 你可以假设总是存在一个答案。

**示例 1:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["Piatti", "The Grill at Torrey Pines", "Hungry Hunter Steakhouse", "Shogun"]
输出: ["Shogun"]
解释: 他们唯一共同喜爱的餐厅是“Shogun”。
```

**示例 2:**

```
输入:
["Shogun", "Tapioca Express", "Burger King", "KFC"]
["KFC", "Shogun", "Burger King"]
输出: ["Shogun"]
解释: 他们共同喜爱且具有最小索引和的餐厅是“Shogun”，它有最小的索引和1(0+1)。
```

**提示:**

1. 两个列表的长度范围都在 [1, 1000]内。
2. 两个列表中的字符串的长度将在[1，30]的范围内。
3. 下标从0开始，到列表的长度减1。
4. 两个列表都没有重复的元素。



## 2、解题思路

​	首先对第一个list创建哈希表，然后通过对第二个表遍历，找出最小的索引值之和

​	然后遍历一遍，找到索引值之和等于最小索引值之和的那个元素，放到结果list中

```python
class Solution:
    def findRestaurant(self, list1, list2):
        """
        :type list1: List[str]
        :type list2: List[str]
        :rtype: List[str]
        """
        
        result = []

        hash_list = {}

        index = 0;
        min_index = 2001;

        for i in list1:
            hash_list[i] = index;
            index += 1
        index = 0;
        for i in list2:
            if hash_list.get(i) != None :
                if  hash_list.get(i) + index < min_index:
                    min_index = hash_list.get(i) + index
            index += 1

        index = 0;
        for i in list2:
            if hash_list.get(i) != None  and hash_list.get(i) + index == min_index:
                result.append(i)
            index += 1
        return result
```






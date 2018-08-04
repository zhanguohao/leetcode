# LeetCode: 380. 常数时间插入、删除和获取随机元素

[TOC]

## 1、题目描述

设计一个支持在*平均* 时间复杂度 **O(1)** 下，执行以下操作的数据结构。

1. `insert(val)`：当元素 val 不存在时，向集合中插入该项。
2. `remove(val)`：元素 val 存在时，从集合中移除该项。
3. `getRandom`：随机返回现有集合中的一项。每个元素应该有**相同的概率**被返回。

**示例 :**

```
// 初始化一个空的集合。
RandomizedSet randomSet = new RandomizedSet();

// 向集合中插入 1 。返回 true 表示 1 被成功地插入。
randomSet.insert(1);

// 返回 false ，表示集合中不存在 2 。
randomSet.remove(2);

// 向集合中插入 2 。返回 true 。集合现在包含 [1,2] 。
randomSet.insert(2);

// getRandom 应随机返回 1 或 2 。
randomSet.getRandom();

// 从集合中移除 1 ，返回 true 。集合现在包含 [2] 。
randomSet.remove(1);

// 2 已在集合中，所以返回 false 。
randomSet.insert(2);

// 由于 2 是集合中唯一的数字，getRandom 总是返回 2 。
randomSet.getRandom();
```



## 2、解题思路

​	题目要求添加，删除，以及随机返回，都是$O(1)$的时间复杂度

​	如果直接使用数组，插入和随机返回都能够满足要求，但是删除不可以，会留有空位

​	如果直接用字典，插入和删除都能够满足要求，但是删除不可以，不能随机的找出想要的数字

随意，将数组和字典结合起来使用，我们使用数组保存元素，当我们想要删除的时候，就将当前元素与末尾元素调换，然后删除末尾元素即可，这样就得到了线性时间复杂度，并且，为了删除能快速定位这个元素，我们使用字典，保存元素所在的下标

```python
class RandomizedSet:
    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.nums = []
        self.nums_index = {}

    def insert(self, val):
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        :type val: int
        :rtype: bool
        """

        if val not in self.nums_index:
            self.nums.append(val)
            self.nums_index[val] = len(self.nums) - 1
            return True
        else:
            return False

    def remove(self, val):
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        :type val: int
        :rtype: bool
        """
        
        if val in self.nums_index:
            if len(self.nums) == 1:
                self.nums.pop()
                self.nums_index.pop(val)
            else:

                index = self.nums_index[val]
                self.nums[index], self.nums[-1] = self.nums[-1], self.nums[index]
                self.nums_index[self.nums[index]] = index
                self.nums.pop()
                self.nums_index.pop(val)
            return True
        else:
            return False

    def getRandom(self):
        """
        Get a random element from the set.
        :rtype: int
        """

        index = random.randint(0, len(self.nums) - 1)
        return self.nums[index]

# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```


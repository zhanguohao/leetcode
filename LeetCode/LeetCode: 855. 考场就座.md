# LeetCode: 855. 考场就座

[TOC]



## 1、题目描述

​	在考场里，一排有 `N` 个座位，分别编号为 `0, 1, 2, ..., N-1` 。

​	当学生进入考场后，他必须坐在能够使他与离他最近的人之间的距离达到最大化的座位上。如果有多个这样的座位，他会坐在编号最小的座位上。(另外，如果考场里没有人，那么学生就坐在 0 号座位上。)

​	返回 `ExamRoom(int N)` 类，它有两个公开的函数：其中，函数 `ExamRoom.seat()` 会返回一个 `int` （整型数据），代表学生坐的位置；函数 `ExamRoom.leave(int p)` 代表坐在座位 `p` 上的学生现在离开了考场。请确保每次调用 `ExamRoom.leave(p)` 时都有学生坐在座位 `p` 上。

**示例：**

```
输入：["ExamRoom","seat","seat","seat","seat","leave","seat"], [[10],[],[],[],[],[4],[]]
输出：[null,0,9,4,2,null,5]
解释：
ExamRoom(10) -> null
seat() -> 0，没有人在考场里，那么学生坐在 0 号座位上。
seat() -> 9，学生最后坐在 9 号座位上。
seat() -> 4，学生最后坐在 4 号座位上。
seat() -> 2，学生最后坐在 2 号座位上。
leave(4) -> null
seat() -> 5，学生最后坐在 5 号座位上。
```

 

**提示：**

1. `1 <= N <= 10^9`
2. 在所有的测试样例中 `ExamRoom.seat()` 和 `ExamRoom.leave()` 最多被调用 `10^4` 次。
3. 调用 `ExamRoom.leave(p)` 时需要确保当前有学生坐在座位 `p` 上。

## 2、解题思路

​	首先确定一点，我们肯定需要存储已经坐下的位置

​	然后每次想要新的座位的时候，就需要查找最大的空位，然后放入，

​	因此，使用list来存储已经坐下的位置，然后每一次需要查询一下，看看那个空位最大，然后返回

​	需要注意的是，第一个位置和最后一个位置，不需要除以2，中间位置的距离才需要除以2

​    ==寻找座位==

- 判断作为是不是空，如果是，座位是0

- 如果不为空，第一个距离就是座位表里面的第一个数，位置是0，

- 从第二个数开始，每一次减去前面的那个数，除以2表示距离

- 如果距离变大，就更新位置和距离

- 判断完毕以后，跳出来判断最后一个位置

- 然后将位置假如座位表，并返回

  ==离开座位==

- 需要注意的是，离开座位的时候，需要判断这个座位有没有人

```python
import bisect

class ExamRoom(object):
    def __init__(self, N):
        self.N = N
        self.seats = []

    def seat(self):
        if not self.seats:
            pos = 0
        else:
            dist, pos = self.seats[0], 0
            for i, s in enumerate(self.seats):
                if i:
                    prev = self.seats[i-1]
                    d = (s - prev) // 2
                    if d > dist:
                        dist, pos = d, prev + d

            # Considering the right-most seat.
            d = self.N - 1 - self.seats[-1]
            if d > dist:
                pos = self.N - 1
        bisect.insort(self.seats, pos)
        return pos

    def leave(self, p):
        if self.seats.count(p) != 0:
            self.seats.remove(p)

# Your ExamRoom object will be instantiated and called as such:
# obj = ExamRoom(N)
# param_1 = obj.seat()
# obj.leave(p)
```


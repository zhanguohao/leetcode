# LeetCode: 382. 链表随机节点

[TOC]

## 1、题目描述

给定一个单链表，随机选择链表的一个节点，并返回相应的节点值。保证每个节点**被选的概率一样**。

**进阶:**
如果链表十分大且长度未知，如何解决这个问题？你能否使用常数级空间复杂度实现？

**示例:**

```
// 初始化一个单链表 [1,2,3].
ListNode head = new ListNode(1);
head.next = new ListNode(2);
head.next.next = new ListNode(3);
Solution solution = new Solution(head);

// getRandom()方法应随机返回1,2,3中的一个，保证每个元素被返回的概率相等。
solution.getRandom();
```



## 2、解题思路

​	这道题目实际上是蓄水池抽样的例子，

​	假设有n条数据，可以是很大的文件很大，或者是流式数据，随机的选取k条数据，要保证取到每一个数据的概率是一样的，该如何来做呢？

来自《The Art of Computer Programming》里的伪代码：

```
Init : a reservoir with the size： k  
for i= k+1 to N  
    M=random(1, i);  
    if( M < k)  
     SWAP the Mth value and ith value  
end for   
```

​	

​	首先，我们进行分析，假设有N个元素，我们只需要选取一个，保证每一个元素被选中的概率是一样的，该如何做呢？

- N=1，选中这个元素的概率就是1，直接返回

- N=2，首先我们选中第1个元素，然后通过随机数生成，生成（1，2），如果生成的数字是1，返回1，如果是2，返回2

  或者反过来，如果是1，表示用当前数字替换1的位置

  其概率都是0.5

- N=3，同样是去取一个元素

  - 先选中第一个元素，让后读取第二个数，取随机数（1，2），如果取到1，将当前元素和第一个元素交换，每个元素被取到的概率都是0.5

  - 然后是第三个元素，随机的取（1，2，3），如果渠道的元素是1，就将当前元素和第一个元素交换，每一个元素渠道的概率：

    ==第一个元素==：$1* 1/2 * 2/3 = 1/3$

    ​	首先，取到第一个元素的概率是1，然后加入第二个元素，取到的概率就是$1/2$, 然后加入第三个元素，第一个元素被取到的概率是$1/3$, 为什么呢？因为加入第三个元素，1在首位的概率是$1/2$，然后如果取到的随机数（1，2，3），不是1，就表示1最终可能被取到的概率，那么取到随机数2，3的概率就是$2/3$

    ==第二个元素==：$1/2 * 2/3 = 1/3$

    ​	如果是2个元素，第二个元素被放到首位的概率是$1/2$，然后，加入第三个元素，被选中的概率是多少呢？

    是$2/3$，因为，不选中1的概率就是$2/3$, 于是，第二个元素被选中的概率是



```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    
    def __init__(self, head):
        """
        @param head The linked list's head.
        Note that the head is guaranteed to be not null, so it contains at least one node.
        :type head: ListNode
        """
        self.head = head
        self.value = head.val

    def getRandom(self):
        """
        Returns a random node's value.
        :rtype: int
        """

        temp = self.head
        count = 0
        while temp:
            count += 1
            if random.randint(1, count) == 1:
                self.value = temp.val
            temp = temp.next
        return self.value

# Your Solution object will be instantiated and called as such:
# obj = Solution(head)
# param_1 = obj.getRandom()
```

​	如果使用蓄水池抽样，执行效率并不是太高，

​	答案中有一些取巧的做法，使用的是将所有元素先放到数组中，然后使用随机的下标，当然，这样做，主要是在小数	据量的时候有用，在大数据处理的时候，会出现问题。

​	当然，这个也要考虑实际情况，如果说内存能够将所有的数据放进去，当然就能够直接用随机的下标取出来了，这样效率更高；

​	如果是流式数据的应用场景，那么使用蓄水池抽样就很合适了。



[蓄水池抽样]: http://blog.jobbole.com/42550/	"伯乐在线"



​	
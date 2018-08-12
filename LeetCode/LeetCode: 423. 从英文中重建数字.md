# LeetCode: 423. 从英文中重建数字

[TOC]

## 1、题目描述

给定一个**非空**字符串，其中包含字母顺序打乱的英文单词表示的数字`0-9`。按升序输出原始的数字。

**注意:**

1. 输入只包含小写英文字母。
2. 输入保证合法并可以转换为原始的数字，这意味着像 "abc" 或 "zerone" 的输入是不允许的。
3. 输入字符串的长度小于 50,000。

**示例 1:**

```
输入: "owoztneoer"

输出: "012" (zeroonetwo)
```

**示例 2:**

```
输入: "fviefuro"

输出: "45" (fourfive)
```

## 2、解题思路

​	首先来分析下每个数字的英文组成：

```
zero
one
two
three
four
five
six
seven
eight
nine
```

​	根据上面的字符，我们发现，一共出现了下面几个字符：

```
['e', 'f', 'g', 'h', 'i', 'n', 'o', 'r', 's', 't', 'u', 'v', 'w', 'x', 'z']
```

​	其中，仅仅在某一个数字中出现的有

```
z: zero
w: two
u: four
x: six
g: eight
```

​	因此，根据这几个字符的数量，我们就能够直接判断上面几个数字的数量

并且，有了上面几个数字以后，我们就可以判断机器的数字了，例如1

```
one 
在所有数字中，包含o的只有0，1，2，4，而我们已经得到0，2，4的数量，就可以直接算出1的数量了
```

```
three
r仅仅出现在zero，four，同样的也能够计算出3的数量了
```

```
five
f仅仅出现在four，five，也可以计算出5的数量了
```

```
seven
v仅仅出现在five，seven中，利用v直接计算7
```

```
nine
其他数字都已经计算出来了，剩下的9，利用n来计算也可以，i也可以，e也可以，用n的计算量比较小，因为n仅出现在one，seven，nine中
```

```python
class Solution:
    def originalDigits(self, s):
        """
        :type s: str
        :rtype: str
        """
        def zero():
            return 0

        buff = collections.defaultdict(zero)
        nums = [0] * 10

        for c in s:
            buff[c] += 1

        nums[0] = buff['z']
        nums[2] = buff['w']
        nums[4] = buff['u']
        nums[6] = buff['x']
        nums[8] = buff['g']
        nums[1] = buff['o'] - nums[0] - nums[2] -nums[4]
        nums[3] = buff['r'] - nums[0] - nums[4]
        nums[5] = buff['f'] - nums[4]
        nums[7] = buff['v'] - nums[5]
        nums[9] = buff['i'] - nums[5] - nums[6] - nums[8]

        ans = [str(i) * n for i, n in enumerate(nums)]
        return "".join(ans)
```


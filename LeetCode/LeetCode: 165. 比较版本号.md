# LeetCode: 165. 比较版本号

[TOC]



## 1、题目描述



比较两个版本号 *version1* 和 *version2*。
如果 `*version1* > *version2*` 返回 `1`，如果 `*version1* < *version2*` 返回 `-1`， 除此之外返回 `0`。

你可以假设版本字符串非空，并且只包含数字和 `.` 字符。

 `.` 字符不代表小数点，而是用于分隔数字序列。

例如，`2.5` 不是“两个半”，也不是“差一半到三”，而是第二版中的第五个小版本。

**示例 1:**

```
输入: version1 = "0.1", version2 = "1.1"
输出: -1
```

**示例 2:**

```
输入: version1 = "1.0.1", version2 = "1"
输出: 1
```

**示例 3:**

```
输入: version1 = "7.5.2.4", version2 = "7.5.3"
输出: -1
```



## 2、解题思路

​	

- 转换成数组
- 首先，判断两个数组中，小的那个数组的部分
- 判断完以后，如果是相等的，继续判断大的数组中，剩余部分是不是都是0，如果是，结果为0



```python
class Solution:
    def compareVersion(self, version1, version2):
        """
        :type version1: str
        :type version2: str
        :rtype: int
        """
        v1 = [int(v) for v in version1.split(".")]
        v2 = [int(v) for v in version2.split(".")]

        l1 = len(v1)
        l2 = len(v2)
        result = 0

        judge_times = min(l1, l2)

        for i in range(judge_times):
            if v1[i] == v2[i]:
                continue
            elif v1[i] > v2[i]:
                result = 1
                break
            elif v1[i] < v2[i]:
                result = -1
                break

                
        if result == 0:
            if l1 > l2:
                for i in range(l2, l1):
                    if v1[i] > 0:
                        result = 1
                        break

            elif l1 < l2:

                for i in range(l1, l2):
                    if v2[i] > 0:
                        result = -1
                        break
        return result
```


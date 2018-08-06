# LeetCode: 388. 文件的最长绝对路径

[TOC]

## 1、题目描述

假设我们以下述方式将我们的文件系统抽象成一个字符串:

字符串 `"dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext"` 表示:

```
dir
    subdir1
    subdir2
        file.ext
```

目录 `dir` 包含一个空的子目录 `subdir1` 和一个包含一个文件 `file.ext` 的子目录 `subdir2` 。

字符串 `"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"` 表示:

```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```

目录 `dir` 包含两个子目录 `subdir1` 和 `subdir2`。 `subdir1` 包含一个文件 `file1.ext` 和一个空的二级子目录 `subsubdir1`。`subdir2` 包含一个二级子目录 `subsubdir2` ，其中包含一个文件 `file2.ext`。

我们致力于寻找我们文件系统中文件的最长 (按字符的数量统计) 绝对路径。例如，在上述的第二个例子中，最长路径为 `"dir/subdir2/subsubdir2/file2.ext"`，其长度为 `32` (不包含双引号)。

给定一个以上述格式表示文件系统的字符串，返回文件系统中文件的最长绝对路径的长度。 如果系统中没有文件，返回 `0`。

**说明:**

- 文件名至少存在一个 `.` 和一个扩展名。
- 目录或者子目录的名字不能包含 `.`。

要求时间复杂度为 `O(n)` ，其中 `n` 是输入字符串的大小。

请注意，如果存在路径 `aaaaaaaaaaaaaaaaaaaaa/sth.png` 的话，那么  `a/aa/aaa/file1.txt` 就不是一个最长的路径。



## 2、解题思路

- 首先，我们可以将字符串按照'\n'进行分割
- 然后每一个目录或者是文件，前面有'\t'，个数就表示其深度
- 将目录入栈，并同时保存他的深度，遇到目录就入栈，遇到文件就将更新最长路径值

举个例子

```
"dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext"
首先分割字符串
['dir', '\tsubdir1', '\t\tfile1.ext', '\t\tsubsubdir1', '\tsubdir2', '\t\tsubsubdir2', '\t\t\tfile2.ext']
然后对每一个，计算其深度，并且去掉'\t'
['dir', '\tsubdir1', '\t\tfile1.ext', '\t\tsubsubdir1', '\tsubdir2', '\t\tsubsubdir2', '\t\t\tfile2.ext']
深度：
[0, 1, 2, 2, 1, 2, 3]
然后将每一个字符串去掉'\t'
['dir', 'subdir1', 'file1.ext', 'subsubdir1', 'subdir2', 'subsubdir2', 'file2.ext']

有了字符串，还有深度，就可以开始判断了，首先是判断当前字符串是不是路径，如果是，就判断当前的深度是不是小于栈中最后一个目录深度，如果是，就出栈，直到大于或者栈为空结束，然后将目录入栈
如果是文件，不需要入栈，需要判断栈中的目录深度是不是小于当前文件，如果是，表示这个就是父目录，更新最长路径值
```



```python
class Solution:
    def lengthLongestPath(self, input):
        """
        :type input: str
        :rtype: int
        """
        
        stack = []
        input_split = input.split('\n')
        deepth = [x.count('\t') for x in input_split]
        dir_files = [x.replace('\t', '') for x in input_split]

        res = 0
        count = 0
        index = 0
        while index < len(dir_files):
            if '.' in dir_files[index]:
                if not stack or stack[-1][1] < deepth[index]:
                    res = max(res, count + len(dir_files[index]) + len(stack))
                else:
                    while stack and stack[-1][1] >= deepth[index]:
                        count -= stack[-1][0]
                        stack.pop()
                    index -= 1

            else:
                if not stack or stack[-1][1] < deepth[index]:
                    count += len(dir_files[index])
                    stack.append((len(dir_files[index]), deepth[index]))
                else:
                    while stack and stack[-1][1] >= deepth[index]:
                        count -= stack[-1][0]
                        stack.pop()
                    index -= 1

            index += 1

        return res
```


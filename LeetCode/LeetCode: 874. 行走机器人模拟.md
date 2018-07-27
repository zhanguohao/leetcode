# LeetCode: 874. 行走机器人模拟

[TOC]

## 1、题目描述



无限网格上的机器人从点 (0, 0) 处开始出发，面向北方。该机器人可以接收以下三种类型的命令：

- `-2`：向左转 90 度
- `-1`：向右转 90 度
- `1 <= x <= 9`：向前移动 `x` 个单位长度

有一些网格方块被视作障碍物。 

第 `i` 个障碍物位于网格点  `(obstacles[i][0], obstacles[i][1])`

如果机器人试图走到障碍物上方，那么它将停留在障碍物的前一个网格方块上，但仍然可以继续该路线的其余部分。

返回从原点到机器人的最大欧式距离的**平方**。

 

**示例 1：**

```
输入: commands = [4,-1,3], obstacles = []
输出: 25
解释: 机器人将会到达 (3, 4)
```

**示例 2：**

```
输入: commands = [4,-1,4,-2,4], obstacles = [[2,4]]
输出: 65
解释: 机器人在左转走到 (1, 8) 之前将被困在 (1, 4) 处
```

 

**提示：**

1. `0 <= commands.length <= 10000`
2. `0 <= obstacles.length <= 10000`
3. `-30000 <= obstacle[i][0] <= 30000`
4. `-30000 <= obstacle[i][1] <= 30000`
5. 答案保证小于 `2 ^ 31`



## 2、解题思路

​	首先，我们使用下面的变量保存方向

```
direct :
	0	东
	1	南
	2	西
	3	北
	
(direct + 1 + 4 )% 4 ： 表示右转
(direct - 1 + 4 )% 4 ： 表示左转
```

然后就需要根据方向，还有步伐，加减，只要前方没有障碍物，继续走

最后返回坐标平方和

​	因为障碍物的list不好查询，我们转换成dict

​	要注意一点，返回的是最大欧式距离平方，也就是距离原点最远距离的位置，

```python
class Solution:
    def robotSim(self, commands, obstacles):
        """
        :type commands: List[int]
        :type obstacles: List[List[int]]
        :rtype: int
        """
        
        obstacles_dict = {tuple(v): i for i, v in enumerate(obstacles)}

        start_x = 0
        start_y = 0
        direct = 3

        result = 0

        for i in commands:
            if i == -1:
                direct = (direct + 1 + 4) % 4
            elif i == -2:
                direct = (direct - 1 + 4) % 4
            else:
                temp = i
                while temp > 0:
                    if direct == 0:
                        if (start_x + 1, start_y) not in obstacles_dict:
                            start_x += 1
                        else:
                            break
                    elif direct == 1:
                        if (start_x, start_y - 1) not in obstacles_dict:
                            start_y -= 1
                        else:
                            break
                    elif direct == 2:
                        if (start_x - 1, start_y) not in obstacles_dict:
                            start_x -= 1
                        else:
                            break
                    else:
                        if (start_x, start_y + 1) not in obstacles_dict:
                            start_y += 1
                        else:
                            break
                    temp -= 1
            result = max(result, start_x ** 2 + start_y ** 2)

        return result


```


# LeetCode: 626. 换座位

[TOC]

## 1、题目描述



小美是一所中学的信息科技老师，她有一张 `seat` 座位表，平时用来储存学生名字和与他们相对应的座位 id。

其中纵列的 **id** 是连续递增的

小美想改变相邻俩学生的座位。

你能不能帮她写一个 SQL query 来输出小美想要的结果呢？

 

**示例：**

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Abbot   |
|    2    | Doris   |
|    3    | Emerson |
|    4    | Green   |
|    5    | Jeames  |
+---------+---------+
```

假如数据输入的是上表，则输出结果如下：

```
+---------+---------+
|    id   | student |
+---------+---------+
|    1    | Doris   |
|    2    | Abbot   |
|    3    | Green   |
|    4    | Emerson |
|    5    | Jeames  |
+---------+---------+
```

**注意：**

如果学生人数是奇数，则不需要改变最后一个同学的座位。



## 2、解题思路

​	

​	首先通过判断当前id是不是奇数，如果是，当前id+1是不是大于最大id，如果大于，使用当前id，

​	如果不大于，是奇数，就用id+1，是偶数，就直接使用id-1，最后在使用一下升序排列





```sql
# Write your MySQL query statement below

select if(if(id%2,id+1,id-1)> (select max(id) from seat), id, if (id%2,id+1,id-1) ) as id ,student from seat order by id asc
```


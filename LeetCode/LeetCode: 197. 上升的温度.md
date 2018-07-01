# LeetCode: 197. 上升的温度

[TOC]

## 1、题目描述



给定一个 `Weather` 表，编写一个 SQL 查询，来查找与之前（昨天的）日期相比温度更高的所有日期的 Id。

```
+---------+------------------+------------------+
| Id(INT) | RecordDate(DATE) | Temperature(INT) |
+---------+------------------+------------------+
|       1 |       2015-01-01 |               10 |
|       2 |       2015-01-02 |               25 |
|       3 |       2015-01-03 |               20 |
|       4 |       2015-01-04 |               30 |
+---------+------------------+------------------+
```

例如，根据上述给定的 `Weather` 表格，返回如下 Id:

```
+----+
| Id |
+----+
|  2 |
|  4 |
+----+
```



## 2、解题思路

​	这里有一点需要注意，就是每一次比较，是今天的时间和昨天的时间进行比较

​	所以将日期转换为天数，得到差值为1的记录，并且过滤掉第一个，前面没有温度的



```sql
# Write your MySQL query statement below
select w1.Id from Weather w1 left join Weather w2 on to_days(w1.RecordDate) - to_days(w2.RecordDate) =1 and w1.Temperature > w2.Temperature where w2.Id is not null;
```


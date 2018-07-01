# LeetCode: 196. 删除重复的电子邮箱

[TOC]

## 1、题目描述

编写一个 SQL 查询，来删除 `Person` 表中所有重复的电子邮箱，重复的邮箱里只保留 **Id** *最小* 的那个。

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
| 3  | john@example.com |
+----+------------------+
Id 是这个表的主键。
```

例如，在运行你的查询语句之后，上面的 `Person` 表应返回以下几行:

```
+----+------------------+
| Id | Email            |
+----+------------------+
| 1  | john@example.com |
| 2  | bob@example.com  |
+----+------------------+
```





## 2、解题思路

```sql
# Write your MySQL query statement below
delete p1 from Person p1, Person p2 where p1.Email = p2.Email and p1.Id > p2.Id
```



```sql
# Write your MySQL query statement below
delete from Person where Id not in (select Id from (select min(Id) Id from Person group by Email) p)
```


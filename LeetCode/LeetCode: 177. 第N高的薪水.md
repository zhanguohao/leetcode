# LeetCode: 177. 第N高的薪水

[TOC]

## 1、题目描述

编写一个 SQL 查询，获取 `Employee` 表中第 *n* 高的薪水（Salary）。

```
+----+--------+
| Id | Salary |
+----+--------+
| 1  | 100    |
| 2  | 200    |
| 3  | 300    |
+----+--------+
```

例如上述 `Employee` 表，*n = 2* 时，应返回第二高的薪水 `200`。如果不存在第 *n* 高的薪水，那么查询应返回 `null`。

```
+------------------------+
| getNthHighestSalary(2) |
+------------------------+
| 200                    |
+------------------------+
```



## 2、解题思路



```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT

BEGIN

declare M INT;
set M = N-1;

  RETURN (
      # Write your MySQL query statement below.
      select distinct Salary  from Employee order by Salary DESC limit M,1 
      
  );
END
```


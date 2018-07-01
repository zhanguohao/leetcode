# LeetCode: 185. 部门工资前三高的员工

[TOC]



## 1、题目描述



`Employee` 表包含所有员工信息，每个员工有其对应的 Id, salary 和 department Id 。

```
+----+-------+--------+--------------+
| Id | Name  | Salary | DepartmentId |
+----+-------+--------+--------------+
| 1  | Joe   | 70000  | 1            |
| 2  | Henry | 80000  | 2            |
| 3  | Sam   | 60000  | 2            |
| 4  | Max   | 90000  | 1            |
| 5  | Janet | 69000  | 1            |
| 6  | Randy | 85000  | 1            |
+----+-------+--------+--------------+
```

`Department` 表包含公司所有部门的信息。

```
+----+----------+
| Id | Name     |
+----+----------+
| 1  | IT       |
| 2  | Sales    |
+----+----------+
```

编写一个 SQL 查询，找出每个部门工资前三高的员工。例如，根据上述给定的表格，查询结果应返回：

```
+------------+----------+--------+
| Department | Employee | Salary |
+------------+----------+--------+
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |
+------------+----------+--------+
```



## 2、解题思路

​	之前，我们找出了部门第N高的薪水，这里使用的思路差不多，如果在部门里面，大于当前工资的数量少于3个，就表示当前工资是前3高的



```sql
# Write your MySQL query statement below
select d.Name as Department , e.Name as Employee , e.Salary from Employee e , Department d where e.DepartmentId = d.Id and (select count(distinct Salary) from Employee where DepartmentId = d.id and  Salary > e.Salary ) < 3  order by Department
```


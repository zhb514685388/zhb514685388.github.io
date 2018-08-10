---
title: MYSQL数据库笔记_5之子查询
date: 2018-08-10 16:18:38
tags:
---

子查询

1、什么是子查询？
	- select 嵌套 select语句

2、子查询可以出现在哪？
	select ..(select)
	from ..(select)
	where.. (select)

<!--more-->
where 后面使用子查询 
3、例：找出薪水比公司平均薪水高的员工，要求显示员工名和薪水
第一步：求平均薪水
mysql> select avg(sal) asal from emp;
+-------------+
| asal        |
+-------------+
| 2073.214286 |
+-------------+

第二步:将上面的表当作临时表
mysql> select ename,sal from emp where e.sal > (select avg(sal) from emp);
+-------+---------+
| ename | sal     |
+-------+---------+
| JONES | 2975.00 |
| BLAKE | 2850.00 |
| CLARK | 2450.00 |
| SCOTT | 3000.00 |
| KING  | 5000.00 |
| FORD  | 3000.00 |
+-------+---------+


4、from面使用子查询 例：
找出每个部门的平均薪水，并且要求显示平均薪水的薪水等级

第一步：求部门平均薪水
mysql> select deptno,avg(sal) asal from emp group by deptno;
+--------+-------------+
| deptno | asal        |
+--------+-------------+
|     10 | 2916.666667 |
|     20 | 2175.000000 |
|     30 | 1566.666667 |
+--------+-------------+

第二步：将上面的表当作临时表
mysql> select t.deptno,t.asal,s.grade from (select deptno,avg(sal) asal from emp group by deptno) t join salgrade s on t.asal between s.losal and s.hisal;
+--------+-------------+-------+
| deptno | asal        | grade |
+--------+-------------+-------+
|     10 | 2916.666667 |     4 |
|     20 | 2175.000000 |     4 |
|     30 | 1566.666667 |     3 |
+--------+-------------+-------+


5、select后面使用子查询[了解]



6、union 将两条语句联合 [了解]
select ename,job from emp where job='manger'
union
select ename,job from emp where job='salesman'

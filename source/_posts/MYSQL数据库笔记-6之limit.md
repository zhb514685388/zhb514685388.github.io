---
title: MYSQL数据库笔记_6之limit
date: 2018-08-10 16:19:42
tags:
---

limit

1、limit用来获取一张表中的某部分数据

2、limit只有在MYSQL中存在，不通用。
<!--more-->
3、例：找出员工表中前5条记录
mysql> select ename from emp limit 5;
+--------+
| ename  |
+--------+
| SIMITH |
| ALLEN  |
| WARD   |
| JONES  |
| MARTIN |
+--------+

以上SQL语句的‘limit 5’中的表示从表中记录下标0开始，取5条
等同于下条语句：
select ename from emp limit 0,5;

limit语法格式 ：
	limit 起始下标，长度
	起始下标没有指定，默认从0开始，0表示表中第一条记录
	limit 出现在SQL语句最后面；

4、例：找出工资排名前5名的员工
mysql> select ename,sal from emp order by sal desc limit 5;
+-------+---------+
| ename | sal     |
+-------+---------+
| KING  | 5000.00 |
| FORD  | 3000.00 |
| SCOTT | 3000.00 |
| JONES | 2975.00 |
| BLAKE | 2850.00 |
+-------+---------+

5、例：找出工资排名在 (3-9)名的员工：
mysql> select ename,sal from emp order by sal desc limit 2,7;
+--------+---------+
| ename  | sal     |
+--------+---------+
| SCOTT  | 3000.00 |
| JONES  | 2975.00 |
| BLAKE  | 2850.00 |
| CLARK  | 2450.00 |
| ALLEN  | 1600.00 |
| TURNER | 1500.00 |
| MILLER | 1300.00 |
+--------+---------+

6、MYSQL中分页语句：

每页显示3条记录：
第1页：0,3
第2页：3,3
第3页：6,3
第4页：9,3

每页显示pageSize条记录
第pageNo页：(pageNo - 1) × pageSize，pageSize

select 
	a
from
	b
order by
	* desc
limit
	(pageNo - 1) × pageSize，pageSize;

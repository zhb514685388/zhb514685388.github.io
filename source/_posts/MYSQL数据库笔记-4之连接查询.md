---
title: MYSQL数据库笔记_4之连接查询
date: 2018-08-10 16:17:36
tags:
---

连接查询

1、什么是连接查询？
	- 查询的时候只从一张表检索数据，被称为单表查询
	
	- 在实际的开发中，数据并不是存储在一张表中的，是同时存储在多张表中，这些表和表之间存在关系，我们在检索的时候通常是需要将多张表联合起来取得有效数据，这种多表查询被称为连接查询或跨表查询。

2、根据年代分类：
	- SQL92
	- SQL99：更新语法，主要掌握
	SQL92需要能看懂
<!--more-->
3、连接查询根据连接方式分类：
	- 内连接：
		*等值连接
		*非等值连接
		*自连接

	- 外连接
		*左外连接(左连接)
		*右外连接(右连接)

	- 全连接(暂时不掌握，很少使用)

4、当多第表进行连接查询，若没有任何条件进行限制，会发生什么现象？
	例：查询每一个员工所在的部门名称，要求最终显示员工名和对应部门名。

mysql> select deptno,ename from emp; 员工表
+--------+--------+
| deptno | ename  |
+--------+--------+
|     20 | SIMITH |
|     30 | ALLEN  |
|     30 | WARD   |
|     20 | JONES  |
|     30 | MARTIN |
|     30 | BLAKE  |
|     10 | CLARK  |
|     20 | SCOTT  |
|     10 | KING   |
|     30 | TURNER |
|     20 | ADAMS  |
|     30 | JAMES  |
|     20 | FORD   |
|     10 | MILLER |
+--------+--------+

mysql> select deptno,dname from dept; 部门表
+--------+-------------+
| deptno | dname       |
+--------+-------------+
|     10 | ACCOUNTING  |
|     20 | RESEARCHING |
|     30 | SALES       |
|     40 | OPERATONS   |
+--------+-------------+

主要分析：多张表连接查询，若没有任何条件限制，会发生什么？

小知识： 在进行多表连接查询时，尽量给表起别名，这样效率高，可读性高。
mysql> select e.ename,d.dname from emp e,dept d;
+--------+-------------+
| ename  | dname       |
+--------+-------------+
| SIMITH | ACCOUNTING  |
| SIMITH | RESEARCHING |
| SIMITH | SALES       |
| SIMITH | OPERATONS   |
| ALLEN  | ACCOUNTING  |
| ALLEN  | RESEARCHING |
| ALLEN  | SALES       |
| ALLEN  | OPERATONS   |
| WARD   | ACCOUNTING  |
| WARD   | RESEARCHING |
| WARD   | SALES       |
| WARD   | OPERATONS   |
| JONES  | ACCOUNTING  |
| JONES  | RESEARCHING |
| JONES  | SALES       |
| JONES  | OPERATONS   |
|..................... |
+--------+-------------+
56 rows in set

结论：若两张表进行连接查询的时候没有任何条件限制，最终的查询结果总数是两张表记录条数乘积，需要添加限制条件。


例：查询每一个员工所在的部门名称，要求最终显示员工名和对应部门名。

mysql> select deptno,ename from emp; 员工表
+--------+--------+
| deptno | ename  |
+--------+--------+
|     20 | SIMITH |
|     30 | ALLEN  |
|     30 | WARD   |
|     20 | JONES  |
|     30 | MARTIN |
|     30 | BLAKE  |
|     10 | CLARK  |
|     20 | SCOTT  |
|     10 | KING   |
|     30 | TURNER |
|     20 | ADAMS  |
|     30 | JAMES  |
|     20 | FORD   |
|     10 | MILLER |
+--------+--------+

mysql> select deptno,dname from dept; 部门表
+--------+-------------+
| deptno | dname       |
+--------+-------------+
|     10 | ACCOUNTING  |
|     20 | RESEARCHING |
|     30 | SALES       |
|     40 | OPERATONS   |
+--------+-------------+

注：在连接查询的时候虽然使用了限制条件，但是匹配的次数没有减少，还是56次，只不过这一次的结果都是有效记录。

SQL92语法：内连接中的等值连接
mysql> select e.ename,d.dname from emp e,dept d where e.deptno=d.deptno;

SQL99语法：内连接中的等值连接
mysql> select e.ename,d.dname from emp e join dept d on e.deptno=d.deptno;
或者
mysql> select e.ename,d.dname from emp e inner join dept d on e.deptno=d.deptno; //inner可以省略

SQL99语法：表连接独立出来了，结构更清晰。对表连接不满意的话，还要以再追加where进行过滤。
+--------+-------------+
| ename  | dname       |
+--------+-------------+
| SIMITH | RESEARCHING |
| ALLEN  | SALES       |
| WARD   | SALES       |
| JONES  | RESEARCHING |
| MARTIN | SALES       |
| BLAKE  | SALES       |
| CLARK  | ACCOUNTING  |
| SCOTT  | RESEARCHING |
| KING   | ACCOUNTING  |
| TURNER | SALES       |
| ADAMS  | RESEARCHING |
| JAMES  | SALES       |
| FORD   | RESEARCHING |
| MILLER | ACCOUNTING  |
+--------+-------------+


5、例：找出每一个员工对应的工资等级，要求显示员工名，工资，工资等级

SQL99语法：内连接中的非等值连接
mysql> select e.ename,e.sal,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+--------+---------+-------+
| ename  | sal     | grade |
+--------+---------+-------+
| SIMITH |  800.00 |     1 |
| ALLEN  | 1600.00 |     3 |
| WARD   | 1250.00 |     2 |
| JONES  | 2975.00 |     4 |
| MARTIN | 1250.00 |     2 |
| BLAKE  | 2850.00 |     4 |
| CLARK  | 2450.00 |     4 |
| SCOTT  | 3000.00 |     4 |
| KING   | 5000.00 |     5 |
| TURNER | 1500.00 |     3 |
| ADAMS  | 1100.00 |     1 |
| JAMES  |  950.00 |     1 |
| FORD   | 3000.00 |     4 |
| MILLER | 1300.00 |     2 |
+--------+---------+-------+


6、找出每一个员工的上级领导，要求显示员工名以及对应的上级领导名称
emp a<员工表>
emp b<领导表>
条件：a.mgr=b.empno

SQL99语法：内连接中的自连接
mysql> select a.ename empname,b.ename leadername from emp a join emp b on a.mgr=b.empno;
+---------+------------+
| empname | leadername |
+---------+------------+
| SIMITH  | FORD       |
| ALLEN   | BLAKE      |
| WARD    | BLAKE      |
| JONES   | KING       |
| MARTIN  | BLAKE      |
| BLAKE   | KING       |
| CLARK   | KING       |
| SCOTT   | JONES      |
| TURNER  | BLAKE      |
| ADAMS   | SCOTT      |
| JAMES   | BLAKE      |
| FORD    | JONES      |
| MILLER  | CLARK      |
+---------+------------+


7、例：找出每一个员工对应的部门名称,要求显示全部部门

内连接：A表和B能够完全匹配的记录查询出来，称为内连接

外连接：A表和B表能够完全匹配的记录查询出来之外，将其中一张表的记录无条件的完全查询出来，对方表没有匹配的记录，会自动模拟出NULL与之匹配，称为外连接。

外连接的查询结果条数 >= 内连接的查询结果条数


SQL99语法：外连接中的右外连接
mysql> select e.ename,d.dname from emp e right outter join dept d on e.deptno=d.deptno;//outer可以省略
mysql> select e.ename,d.dname from emp e right join dept d on e.deptno=d.deptno;

SQL99语法：外连接中的左外连接
mysql> select e.ename,d.dname from dept d left outter join emp e on e.deptno=d.deptno;//outer可以省略
mysql> select e.ename,d.dname from dept d left join emp e on e.deptno=d.deptno; 

注：任何一个右外连接都可以写左外连接，反之也可以

+--------+-------------+
| ename  | dname       |
+--------+-------------+
| SIMITH | RESEARCHING |
| ALLEN  | SALES       |
| WARD   | SALES       |
| JONES  | RESEARCHING |
| MARTIN | SALES       |
| BLAKE  | SALES       |
| CLARK  | ACCOUNTING  |
| SCOTT  | RESEARCHING |
| KING   | ACCOUNTING  |
| TURNER | SALES       |
| ADAMS  | RESEARCHING |
| JAMES  | SALES       |
| FORD   | RESEARCHING |
| MILLER | ACCOUNTING  |
| NULL   | OPERATONS   |
+--------+-------------+

为什么inner和outter可以省略，加上去有什么好处？
	- 可以省略，国为区分内连接和外连接依靠的不是这些关键字，而是看SQL语句中是否存在left/right，若存在，表示一定是一个外连接，其他都是内连接。

	- 加上去的好处是增强可读性


8、例：找出每一个员工对应领导名，要求显示所有的员工
mysql> select a.ename empname,b.ename leadername from emp a left join emp b on a.mgr=b.empno;
+---------+------------+
| empname | leadername |
+---------+------------+
| SIMITH  | FORD       |
| ALLEN   | BLAKE      |
| WARD    | BLAKE      |
| JONES   | KING       |
| MARTIN  | BLAKE      |
| BLAKE   | KING       |
| CLARK   | KING       |
| SCOTT   | JONES      |
| KING    | NULL       |
| TURNER  | BLAKE      |
| ADAMS   | SCOTT      |
| JAMES   | BLAKE      |
| FORD    | JONES      |
| MILLER  | CLARK      |
+---------+------------+


9、例：找出每一个员工对应的部门名称，以及该员工对应的工资等级。要求显示员工名、部门名、工资等级。
mysql> select e.ename,d.dname,s.grade from emp e  join dept d on e.deptno=d.deptno join salgrade s on e.sal between s.losal and s.hisal;
+--------+-------------+-------+
| ename  | dname       | grade |
+--------+-------------+-------+
| SIMITH | RESEARCHING |     1 |
| ALLEN  | SALES       |     3 |
| WARD   | SALES       |     2 |
| JONES  | RESEARCHING |     4 |
| MARTIN | SALES       |     2 |
| BLAKE  | SALES       |     4 |
| CLARK  | ACCOUNTING  |     4 |
| SCOTT  | RESEARCHING |     4 |
| KING   | ACCOUNTING  |     5 |
| TURNER | SALES       |     3 |
| ADAMS  | RESEARCHING |     1 |
| JAMES  | SALES       |     1 |
| FORD   | RESEARCHING |     4 |
| MILLER | ACCOUNTING  |     2 |
+--------+-------------+-------+

多张表进行表连接的语法格式：
select 
	...
from
	a
join
	b
on
	条件
join
	c
on
	条件

原理：a和b先进行表连接，接着a和c再进行表连接。

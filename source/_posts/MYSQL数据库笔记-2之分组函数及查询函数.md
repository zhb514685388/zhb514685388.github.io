---
title: MYSQL数据库笔记_2之分组函数及查询函数
date: 2018-08-10 12:47:47
tags:
---

分组函数及分组查询
1、分组函数：
	分组函数/聚合函数/多行处理函数：
		sum 求和
		count 记数
			count(comm)表示这个字段里面不为空的数量
			count(*) 满足条件的所有记录总数
		avg 求平均值
		max 取最大值
		min 取最小值
<!--more-->
		注：分组函数自动忽略空值，不需要手动的加where排除
		分组函数不能直接使用在where关键字后面

	distinct:将查询重复的结果去除
		select distinct job from emp;
	注：distinct函数前面不能出现字段


2、分组查询涉及到的两个句子：
		- group by
		- having

3、group by
	- 通过哪些字段进行分组

	3.1 例：找每个工作岗位的最高薪水
		先按job分组，再求每一组的最高薪水 
		select job,max(sal) msal from emp group by job;


		select ename，job,max(sal) msal from emp group by job;
		这条语句是错误的，select后面不能跟ename

		注：若一条DQL语句当中有 group by子句，那么select关键字后面只能跟参与分组的字段和分组函数。

	3.2 例：找出每个部门的平均薪水
		mysql> select deptno,avg(sal) from emp group by deptno;
		+--------+-------------+
		| deptno | avg(sal)    |
		+--------+-------------+
		|     10 | 2916.666667 |
		|     20 | 2175.000000 |
		|     30 | 1566.666667 |
		+--------+-------------+

	3.3 例：计算不同部门不同工作岗位的最高薪水
		mysql> select deptno,job,max(sal) from emp group by job,deptno;
		+--------+-----------+----------+
		| deptno | job       | max(sal) |
		+--------+-----------+----------+
		|     20 | ANALYST   |  3000.00 |
		|     10 | CLERK     |  1300.00 |
		|     20 | CLERK     |  1100.00 |
		|     30 | CLERK     |   950.00 |
		|     10 | MANAGER   |  2450.00 |
		|     20 | MANAGER   |  2975.00 |
		|     30 | MANAGER   |  2850.00 |
		|     10 | PRESIDENT |  5000.00 |
		|     30 | SALESMAN  |  1600.00 |
		+--------+-----------+----------+

	3.4 找出每个工作岗位的最高薪水，除MANAGER之外。
		select job,max(sal) as msal from emp where job!='manager' group by job;
		or
		select job,max(sal) as msal from emp group by job having job!='manager' ;
		+-----------+---------+
		| job       | msal    |
		+-----------+---------+
		| ANALYST   | 3000.00 |
		| CLERK     | 1300.00 |
		| PRESIDENT | 5000.00 |
		| SALESMAN  | 1600.00 |
		+-----------+---------+

	3.5 找出每个工作岗位的平均薪水，要求显示平均薪水大于1500.
		mysql> select job,avg(sal) from emp where avg(sal) > 1500 group by job ;
		这是错误的！
		where后面不能直接使用分组函数。
		分组函数必须在分组完成后执行，而分组需要gruop by，而gruop by在where后面执行。


		

3、having
	having和where都是为了完成数据过滤。
	where和having后面都是添加条件。
	where在group by之前完成过滤。
	having在group by 之后完成过滤。

	例：找出每个工作岗位的平均薪水，要求显示平均薪水大于1500.
		mysql> select job,avg(sal) from emp group by job having avg(sal) > 1500;
		+-----------+-------------+
		| job       | avg(sal)    |
		+-----------+-------------+
		| ANALYST   | 3000.000000 |
		| MANAGER   | 2758.333333 |
		| PRESIDENT | 5000.000000 |
		+-----------+-------------+

		原则：尽量在where中过滤，无法过滤的数据，通常都是需要先分组之后再过滤的，这个时候可以选择使用having。效率问题

4、一个完整的DQL语句总结：
	select 
		....
	from
		....
	where
		....
	group by
		....
	having
		....
	order by
		....

	以上关键字顺序不能变，严格遵守
	执行顺序：
		1.from          从某张表中检索数据
		2.where 		经过某条件进行过滤
		3.group by 		分组
		4.having 		分组之后不满足条件再筛选
		5.select		查询出来
		6.order by 		排序输出

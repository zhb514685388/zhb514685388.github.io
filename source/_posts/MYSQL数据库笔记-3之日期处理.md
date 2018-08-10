---
title: MYSQL数据库笔记_3之日期处理
date: 2018-08-10 16:16:07
tags:
---

1、关于MYSQL中的日期处理
	1.1 第一个数据库处理日期的时候，采用的机制是不同的，日期处理都有自己的一套机制。
	所以在实际开发中，表中的字段定义为DATA类型，这种情况很少。因为一旦使用日期类型，
	那么其他程序将不能够通用。实际开发中，一般会使用‘日期字符串‘来表示日期。

2、日期处理函数：
	- str_to_date
	- date_formate

3、str_to_date
	3.1 该函数作用：将’日期字符串‘转换成’日期类型‘数据。[varchar-->date]
	3.2 该函数的执行结果是DATE类型
	3.3 格式：
		str_to_date('日期字符串','日期格式')
<!--more-->
	3.4 MYSQL日期格式：
		%Y	年
		%M  月
		%d  日
		%H  时
		%i  分
		%s  秒

	3.5 例：查询出1980-12-17入职的员工

		mysql> select ename,hiredate from emp where hiredate='1980-12-17';
		+--------+------------+
		| ename  | hiredate   |
		+--------+------------+
		| SIMITH | 1980-12-17 |
		+--------+------------+
		MYSQL默认的日期格式%Y-%m-%d,以上的日期字符串’1980-12-17‘正好和默认的日期格式一样，存在了自动类型转换，自动将日期字符串转换成了日期类型，所以以上查询可以查询出结果。

		mysql> select ename,hiredate from emp where hiredate='12-17-1980';
		Empty set, 2 warnings (0.00 sec)
		'12-17-1980' 日期字符串和MYSQL默认的日期格式不同
		hiredate是DATE类型，'12-17-1980'是一个字符串varchar类型，类型不匹配。所以无法查询结果，会有错误发生。

		纠正以上错误：
		mysql> select ename,hiredate from emp where hiredate=str_to_date('12-17-1980','%m-%d-%Y');
		+--------+------------+
		| ename  | hiredate   |
		+--------+------------+
		| SIMITH | 1980-12-17 |
		+--------+------------+

	3.6例：
	创建学生表：
	create table t_student(
		id int(10),
		name varchar(32),
		birth date
	);

	插入数据：
	insert into t_student(id,name,birth) values(1,'jack','1980-10-11');
	以上可以执行，因为格式和默认日期格式相同，所以自动转换类型。

	insert into t_student(id,name,birth) values(2,'road','10-15-1982');
	ERROR 1292 (22007): Incorrect date value: '10-15-1982' for column 'birth' at row 1
	以上发生错误：
		1、和默认格式不同
		2、'10-15-1982'是varchar类型，和DATE类型不同

	纠正：
	insert into t_student(id,name,birth) values(2,'road',str_to_date('10-15-1982','%m-%d-%Y'));


4、date_format
	4.1 作用：将日期类型DATE转换成具有特定格式的日期字符串varchar[date-->varchar]

	4.2 运算结果：varchar类型

	4.3 语法格式：
		date_format(日期类型数据，'日期格式')

	4.4 例：查询员工的入职日期，以‘%m-%d-%Y’的格式显示到窗口中
	mysql> select ename,date_format(hiredate,'%m-%d-%Y') hiredate from emp;
	+--------+------------+
	| ename  | hiredate   |
	+--------+------------+
	| SIMITH | 12-17-1980 |
	| ALLEN  | 02-20-1981 |
	| WARD   | 02-22-1981 |
	| JONES  | 04-02-1981 |
	| MARTIN | 09-28-1981 |
	| BLAKE  | 05-01-1981 |
	| CLARK  | 06-09-1981 |
	| SCOTT  | 04-19-1987 |
	| KING   | 11-17-1981 |
	| TURNER | 09-08-1981 |
	| ADAMS  | 05-23-1987 |
	| JAMES  | 12-03-1981 |
	| FORD   | 12-03-1981 |
	| MILLER | 01-23-1982 |
	+--------+------------+


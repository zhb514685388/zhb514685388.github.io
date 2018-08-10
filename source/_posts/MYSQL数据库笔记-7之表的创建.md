---
title: MYSQL数据库笔记_7之表的创建
date: 2018-08-10 16:20:24
tags:
---
表

1、表格，用来存储数据，表格是一种结构化文件

2、表格行被 称为记录（表中的数据），表格列被称为字段。

3、字段的属性：字段名称、字段数据类型、字段长度、字段约束

<!--more-->
4、创建表的语法：
	create table tablename(
		colunmnamedatatype(length),
		colunmnamedatatype(length),
		colunmnamedatatype(length),
		colunmnamedatatype(length),
		colunmnamedatatype(length)
	);

5、关于MYSQL数据库中的类型：
	- varchar
		可变长度字符串
		varchar（3）表示存储字符串长度不能超过3
	- char
		定长字符串
		char（3）表示存储字符串长度不能超过3
	
	varchar和char对比：
			- 都是字符串类型
			- varchar比较智能，可能根据实际的数据长度分配空间，比较节省空间，但是在分配的时候需要执行相关的判断程序，效率较低。
			- char不需要动态分配空间，所以执行效率很高，但是可能会导致空间浪费。
			- 若字段中的数据不具备伸缩性，建议采用char类型存储。
			- 若字段中的数据具备很强伸缩性，建议采用varchar类型存储。

	- int
		整型
		int（3）表示最大可以存储999
	- bigint
		长整型
	- float
		浮点型单精度
		float（7,2）7表示7个有效数字，2表示小数位
	- double
		浮点型双精度
	- date
		日期类型
		在实际开发中为了通用，所以日期类型一般不用，采用字符器串代替日期比较多。
	- blob
		binary large object 二进制大对象
		专门存储图片声音视频等数据
		数据库中存储一个图片是很见的，但是存储一个较大的视频是很少的，一般都 是提供一个视频的的链接地址。
	- clob
		charactor large object 字符大对象
		可以存储一个比较大的文本，4G +的字符串
	- 其它。。。


6、创建表格（学生表）
	建立学生信息表，字段包括：学号、姓名、性别、出生日期、eamil

create table t_student(
	id int(10),
	name varchar(32),
	sex char(1),
	birth date,
	email varchar(128)
);

	注：表格的名字是好以t_或tbl开头，增强可读性
		varchar长度最好是2的倍数，方便存储中文

删除表格：
	drop table t_student;这种删除方式，若数据库没有这样的表格，报错
	drop table if exists t_student;最好采用这种方式，只有MYSQL才能用

7、向t_student表格中插入数据
	7.1语法格式：
		insert into tablename(columnname1,columnname2..) values(value1,value2...)
		字段和值必须一一对应，个数必须相同，数据类型必须一致
	7.2 向t_student插入数据
		insert into t_student(id,name,sex,birth,email) values(1,'zhangsan','m','1987-09-17','zhangsan@qq.com');

		insert into t_student(id,name,sex,birth,email) values(2,'lisi','f','1987-09-27','lisi@qq.com');

		mysql> select * from t_student;
		+------+----------+------+------------+-----------------+
		| id   | name     | sex  | birth      | email           |
		+------+----------+------+------------+-----------------+
		|    1 | zhangsan | m    | 1987-09-17 | zhangsan@qq.com |
		|    2 | lisi     | f    | 1987-09-27 | lisi@qq.com     |
		+------+----------+------+------------+-----------------+

		insert into t_student(id,name) values(3,'wangwu');
		mysql> select * from t_student;
		+------+----------+------+------------+-----------------+
		| id   | name     | sex  | birth      | email           |
		+------+----------+------+------------+-----------------+
		|    1 | zhangsan | m    | 1987-09-17 | zhangsan@qq.com |
		|    2 | lisi     | f    | 1987-09-27 | lisi@qq.com     |
		|    3 | wangwu   | NULL | NULL       | NULL            |
		+------+----------+------+------------+-----------------+
		
		当一张表没有指定约束的话，可以为null值

		可以向sex、birth、eamil插入值吗？
		update语句进行更新数据操作
		update t_student set sex='m',birth='1999-02-10',email='wangwu@qq.com' where id=3;

		mysql> select * from t_student;
		+------+----------+------+------------+-----------------+
		| id   | name     | sex  | birth      | email           |
		+------+----------+------+------------+-----------------+
		|    1 | zhangsan | m    | 1987-09-17 | zhangsan@qq.com |
		|    2 | lisi     | f    | 1987-09-27 | lisi@qq.com     |
		|    3 | wangwu   | m    | 1999-02-10 | wangwu@qq.com   |
		+------+----------+------+------------+-----------------+

关于SQL脚本：
	你是怎么看SQL脚本的？
	-该文件是一个普通的文本文件，后缀名.sql,称为SQL脚本
	- 在SQL脚本中有大量的SQL语句，想指执行SQL语句，可以将这些SQL语句写入SQL脚本中，直接使用source执行脚本。
	source 脚本文件路径

9、获取系统当前时间：
	now()获取当前时间
	mysql> select now();
	+---------------------+
	| now()               |
	+---------------------+
	| 2018-08-10 11:45:09 |
	+---------------------+

	创建表，机构表：
create table t_organization(
	code char(10),
	name varchar(32),
	createtime date
);

insert into t_organization(code,name,createtime) values('111111','国家外汇局',now());
insert into t_organization(code,name,createtime) values('111112','北京市外汇局',now());
insert into t_organization(code,name,createtime) values('111113','海南省外汇局',now());
insert into t_organization(code,name,createtime) values('111114','山东省外汇局',now());
insert into t_organization(code,name,createtime) values('111115','福建省外汇局',now());


10、表的复制[快速创建表格]
语法：create table tablename1 as select columnname1,columnname2,...from tablename2;
	
mysql> create table emp1 as select ename,job,sal from emp;
+--------+-----------+---------+
| ename  | job       | sal     |
+--------+-----------+---------+
| SIMITH | CLERK     |  800.00 |
| ALLEN  | SALESMAN  | 1600.00 |
| WARD   | SALESMAN  | 1250.00 |
| JONES  | MANAGER   | 2975.00 |
| MARTIN | SALESMAN  | 1250.00 |
| BLAKE  | MANAGER   | 2850.00 |
| CLARK  | MANAGER   | 2450.00 |
| SCOTT  | ANALYST   | 3000.00 |
| KING   | PRESIDENT | 5000.00 |
| TURNER | SALESMAN  | 1500.00 |
| ADAMS  | CLERK     | 1100.00 |
| JAMES  | CLERK     |  950.00 |
| FORD   | ANALYST   | 3000.00 |
| MILLER | CLERK     | 1300.00 |
+--------+-----------+---------+

11、将查询结果插入某张表中
mysql> insert into emp1 select * from emp1 where sal=3000;

mysql> select * from emp1;
+--------+-----------+---------+
| ename  | job       | sal     |
+--------+-----------+---------+
| SIMITH | CLERK     |  800.00 |
| ALLEN  | SALESMAN  | 1600.00 |
| WARD   | SALESMAN  | 1250.00 |
| JONES  | MANAGER   | 2975.00 |
| MARTIN | SALESMAN  | 1250.00 |
| BLAKE  | MANAGER   | 2850.00 |
| CLARK  | MANAGER   | 2450.00 |
| SCOTT  | ANALYST   | 3000.00 |
| KING   | PRESIDENT | 5000.00 |
| TURNER | SALESMAN  | 1500.00 |
| ADAMS  | CLERK     | 1100.00 |
| JAMES  | CLERK     |  950.00 |
| FORD   | ANALYST   | 3000.00 |
| MILLER | CLERK     | 1300.00 |
| SCOTT  | ANALYST   | 3000.00 |
| FORD   | ANALYST   | 3000.00 |
+--------+-----------+---------+


12、增/删/改 表结构

drop table if exists t_student;
create table t_student(
	id int(10),
	name varchar(32)
);

mysql> desc t_student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(10)     | YES  |     | NULL    |       |
| name  | varchar(32) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+

*给t_student表格添加一个联系电话字段
alter table t_student add phone char(11) not null default '';

mysql> desc t_student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(10)     | YES  |     | NULL    |       |
| name  | varchar(32) | YES  |     | NULL    |       |
| phone | char(11)    | NO   |     |         |       |
+-------+-------------+------+-----+---------+-------+


*将t_student字段的类型和长度修改下

语法：alter table 表名 modify 列名 新的类型名 新列参数;

alter table t_student modify phone varchar(20);
mysql> desc t_student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(10)     | YES  |     | NULL    |       |
| name  | varchar(32) | YES  |     | NULL    |       |
| phone | varchar(20) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+


*将t_student字段的名称、类型和长度修改下

语法：alter table 表名 change 旧列名 新列名 新类型 新参数;

alter table t_student change phone telphone varchar(26);
mysql> desc t_student;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| id       | int(10)     | YES  |     | NULL    |       |
| name     | varchar(32) | YES  |     | NULL    |       |
| telphone | varchar(26) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+


*将t_student表格中的telphone字段删除
alter table t_student drop telphone;
mysql> desc t_student;
+-------+-------------+------+-----+---------+-------+
| Field | Type        | Null | Key | Default | Extra |
+-------+-------------+------+-----+---------+-------+
| id    | int(10)     | YES  |     | NULL    |       |
| name  | varchar(32) | YES  |     | NULL    |       |
+-------+-------------+------+-----+---------+-------+

13、增/删/改表结构[insert,delete,,update]

	13.1 update 语法格式：
		update tablename set 字段名1=字段值，字段名2=字段值2 ..where 条件;

		注：update语句没有条件限制，会将一张表中全部数据更新
		mysql> select * from t_student;
		+------+------+
		| id   | name |
		+------+------+
		|    1 | NULL |
		|    2 | NULL |
		+------+------+

		将id=2的name改为lisi
		update t_student set name='lisi' where id=2;
		mysql> select * from t_student;
		+------+------+
		| id   | name |
		+------+------+
		|    1 | NULL |
		|    2 | lisi |
		+------+------+

	13.2 delete
	格式：delete from tablename 条件
	注：若没条件限制，会将表中记录全部删除

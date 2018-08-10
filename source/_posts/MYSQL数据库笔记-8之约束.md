---
title: MYSQL数据库笔记_8之约束
date: 2018-08-10 16:21:05
tags:
---


约束

1.什么是约束，为什么要使用约束？
	- 约束对应的英语单词：constraint
	- 约束实际上就是表中数据的限制条件
	- 表在设计的时候加入约束的目的就是为了保证表中的记录完整有效。

<!--more-->
2、约束包括哪些？
	- 非空约束         not null
	- 唯一性约束		  unique
	- 主键约束		  primary key  PK
	- 外键约束		  foreign key  FK
	- 检查约束  mysql不支持，oracle支持

3、非空约束
	- not null约束的字段，不能为Null，必须给定具体数据

	 创建表，给字段添加非空约束（用户名不能为空）
	drop table if exists t_user;
	create table t_user(
		id int(10),
		name varchar(32) not null,
		email varchar(128)
	);

4、唯一性约束
	- unique约束的字段具有唯一性，不可重复
	
	//列级约束  
	创建用户，保证邮箱地址唯一
	drop table if exists t_user;
	create table t_user(
		id int(10),
		name varchar(32),
		email varchar(128) unique
	);

	unique约束的字段不能重复，但可以为Null


	drop table if exists t_user;
	//表级约束
	create table t_user(
		id int(10),
		name varchar(32) not null,
		email varchar(128), 
		unique(email)
	);

	使用表级约束给多个字段联合添加约束(以下表示name和eamil两个字段联合唯一)
drop table if exists t_user;

create table t_user(
	id int(10),
	name varchar(32) not null,
	email varchar(128),
	unique(name,email)
);

insert into t_user(id,name,email) values(1,'abc','abc@qq.com');//OK
insert into t_user(id,name,email) values(2,'abc','def@qq.com');//OK
insert into t_user(id,name,email) values(3,'def','abc@qq.com');//OK
insert into t_user(id,name,email) values(4,'def','abc@qq.com');//FAIL 跟第三个重复

	- 表级约束还可以给约束起名字，
	- 为什么 要起名字？ 因为以后要通过名字来删除这个约束
drop table if exists t_user;

create table t_user(
	id int(10),
	name varchar(32) not null,
	email varchar(128),
	constraint t_user_email_name_unique unique(name,email)
);
//查询名字
mysql> use information_schema;

Database changed
mysql> show tables;
+---------------------------------------+
| Tables_in_information_schema          |
+---------------------------------------+
| CHARACTER_SETS                        |
| COLLATIONS                            |
| COLLATION_CHARACTER_SET_APPLICABILITY |
| COLUMNS                               |
| COLUMN_PRIVILEGES                     |
| ENGINES                               |
| EVENTS                                |
| FILES                                 |
| GLOBAL_STATUS                         |
| GLOBAL_VARIABLES                      |
| KEY_COLUMN_USAGE                      |
| OPTIMIZER_TRACE                       |
| PARAMETERS                            |
| PARTITIONS                            |
| PLUGINS                               |
| PROCESSLIST                           |
| PROFILING                             |
| REFERENTIAL_CONSTRAINTS               |
| ROUTINES                              |
| SCHEMATA                              |
| SCHEMA_PRIVILEGES                     |
| SESSION_STATUS                        |
| SESSION_VARIABLES                     |
| STATISTICS                            |
| TABLES                                |
| TABLESPACES                           |
| TABLE_CONSTRAINTS                     |
| TABLE_PRIVILEGES                      |
| TRIGGERS                              |
| USER_PRIVILEGES                       |
| VIEWS                                 |
| INNODB_LOCKS                          |
| INNODB_TRX                            |
| INNODB_SYS_DATAFILES                  |
| INNODB_FT_CONFIG                      |
| INNODB_SYS_VIRTUAL                    |
| INNODB_CMP                            |
| INNODB_FT_BEING_DELETED               |
| INNODB_CMP_RESET                      |
| INNODB_CMP_PER_INDEX                  |
| INNODB_CMPMEM_RESET                   |
| INNODB_FT_DELETED                     |
| INNODB_BUFFER_PAGE_LRU                |
| INNODB_LOCK_WAITS                     |
| INNODB_TEMP_TABLE_INFO                |
| INNODB_SYS_INDEXES                    |
| INNODB_SYS_TABLES                     |
| INNODB_SYS_FIELDS                     |
| INNODB_CMP_PER_INDEX_RESET            |
| INNODB_BUFFER_PAGE                    |
| INNODB_FT_DEFAULT_STOPWORD            |
| INNODB_FT_INDEX_TABLE                 |
| INNODB_FT_INDEX_CACHE                 |
| INNODB_SYS_TABLESPACES                |
| INNODB_METRICS                        |
| INNODB_SYS_FOREIGN_COLS               |
| INNODB_CMPMEM                         |
| INNODB_BUFFER_POOL_STATS              |
| INNODB_SYS_COLUMNS                    |
| INNODB_SYS_FOREIGN                    |
| INNODB_SYS_TABLESTATS                 |
+---------------------------------------+

mysql> desc table_constraints;（该表格中专门存储约束信息）
+--------------------+--------------+------+-----+---------+-------+
| Field              | Type         | Null | Key | Default | Extra |
+--------------------+--------------+------+-----+---------+-------+
| CONSTRAINT_CATALOG | varchar(512) | NO   |     |         |       |
| CONSTRAINT_SCHEMA  | varchar(64)  | NO   |     |         |       |
| CONSTRAINT_NAME    | varchar(64)  | NO   |     |         |       |
| TABLE_SCHEMA       | varchar(64)  | NO   |     |         |       |
| TABLE_NAME         | varchar(64)  | NO   |     |         |       |
| CONSTRAINT_TYPE    | varchar(64)  | NO   |     |         |       |
+--------------------+--------------+------+-----+---------+-------+
6 rows in set (0.00 sec)

mysql> select constraint_name from table_constraints where table_name='t_user';
+--------------------------+
| constraint_name          |
+--------------------------+
| t_user_email_name_unique |
+--------------------------+

5、not null和unique可以联合使用吗？
	- 可以联合使用
	- 被not null unique约束的字段，既不能为空，也不能重复
	- 例：
drop table if exists t_user;
create table t_user(
	id int(10),
	name varchar(32) not null unique
);


6、主键约束-primary key 简称PK
	6.1主键涉及到的术语：
		-主键约束
		- 主键字段
		- 主键值
	6.2 以上的关系：
		表中的某个字段添加主键约束之后，该字段称为主键字段，主键字段中出现的每个值都被称为主键值。

	6.3 给某个字段添加主键约束之后，该字段不能重复，也不能为空。效果和“not null unique"约束相同，但本质不同，主键字段还会默认添加”索引-index“

	6.4 和张表应该有主键字段，若没有，表示这张表是无效的。
	”主键值“是当前行数据的唯一标识。”主键值“是当前行数据的身份证号。
	即使表中现行记录相关的数据是相同的，但是由于主键值不同，我们认为这两行是完全不同的数据。

	6.5  给一个字段添加主键约束，称为单一主键。
	单一主键：
	//列级约束(这种方式用的多)
		drop table if exists t_user;
		create table t_user(
			id int(10) primary key,
			name varchar(32)
		);

	单一主键：
	//表级约束
		drop table if exists t_user;
		create table t_user(
			id int(10) ,
			name varchar(32)，
			primary key(id)
		);


	单一主键：
	//表级约束
	//起名
		drop table if exists t_user;
		create table t_user(
			id int(10) ,
			name varchar(32)，
			constraint t_user_id_pk primary key(id)
		);

	6.6 给多个字段添加一个主键约束，称为复合主键。
	复合主键
	//表级约束
	drop table if exists t_user;
		create table t_user(
			id int(10) ,
			name varchar(32)，
			email varchar(128),
			primary key(id，name)
		);


	复合主键
	//表级约束
	//起名
	drop table if exists t_user;
		create table t_user(
			id int(10) ,
			name varchar(32)，
			email varchar(128),
			constraint t_user_id_pk primary key(id，name)
		);

	6.7 无论是单一主键还是复合主键，一张表主键约束只能有一个。

	6.8 主键根据性质分类：
		- 自然主键
		 	* 主键值若是一个自然数，这自然数和当前表的业务没有任何关系，这种主键叫做自然主键。
		- 业务主键
			* 主键值若和当前表中业务紧密相关的，这种主键值称为业务主键，当前业务数据发生改变的时候，主键值通常会受到影响，所以业务主键使用较少，大部分都 是使用自然主键。

	6.9  在	MYSQL数据库管理系统中提供了一个自增的数字，专门用来自动生成主键值。
	主键值不需要用户维护，也不需要用户提供，自动生成。数字默认从1开始，以1递增
	1,2,3,4,5,6
	drop table if exists t_user;
	create table t_user(
		id int(10) primary key auto_increment,
		name varchar(32)，
	);

7、外键约束-foreign key 简称 FK
	
	7.1 外键涉及到的术语：
		-外键约束
		- 外键字段
		- 外键值

	7.2 以上的关系：
		表中的某个字段添加外键约束之后，该字段称为外键字段，外键字段中出现的每个值都被称为外键值。

	7.3 外键分为单一外键、复合外键

	7.4 一张表中可以有多个外键字段

	7.5 分析场景：
	请设计数据库表用来存储学生和班级信息，给出两种解决方案：
	学生信息和班级信息之间的关系，一个班级对应多个学生，这是典型的一对多的关系。

	第一种 ：将学生信息和班级信息存储到一张表中
	学生信息表t_student
	sno        sname      classno     cname
	-----------------------------------------
	1           jack         100      一班
	2           zhan         100      一班
	3           duck         200      二班
	4           fork         200      二班
	5           zhen         300      三班
	6           luck         400      三班
	以上的设计缺点：数据冗余

	第二种 ：学生信息和班级信息分开存储，学生表+班级表
	学生表t_student
	sno(pk)      sname      classno(fk)
	--------------------------
	1           a       	100
	2           b       	100
	3           c       	200
	4           d       	200
	5           e  			300
	6           f			300

	班级表t_class
	cno(pk)      cname
	--------------------
	100          一班       
	200          二班
	300          三班
	
	结论：为了保证t_student表中的classno字段中的	数据必须来自于t_class表中cno字段中的数据，有必要给t_student表中的classno字段添加外键约束，classno字段被称为外键约束，字段中的100 200 300称为外键值，classno这里是单一外键字段。

	注：外键值可以为NULL
	
	注：外键字段去引用一张表的某个字段的时候，被引用的字段必须具有uniuqe约束。
	
	注：有了外键引用之后，表分了父表和子表，以上父表是班级表，子表是学生表 
	创建时先创建父表，再创建子表。删除时先删除子表中的数据，再删除父表中的数据，插入时先插入父表中的数据，再插入子表中的数据。 

	-----------------------SQL文----------------------------------
drop table if exists t_student;
drop table if exists t_class;
create table t_class(
	cno int(3) primary key,
	cname varchar(32) not null
);
create table t_student(
	sno int(3) primary key,
	sname varchar(32) not null,
	classno int(3),
	constraint t_student_classno_fk foreign key(classno) references t_class(cno)
);
insert into t_class(cno,cname) values(100,'一班');
insert into t_class(cno,cname) values(200,'二班');
insert into t_class(cno,cname) values(300,'三班');
insert into t_student(sno,sname,classno) values(1,'a',100);
insert into t_student(sno,sname,classno) values(2,'b',100);
insert into t_student(sno,sname,classno) values(3,'c',200);
insert into t_student(sno,sname,classno) values(4,'d',200);
insert into t_student(sno,sname,classno) values(5,'e',300);
insert into t_student(sno,sname,classno) values(6,'f',300);
select * from t_class;
select * from t_student;

重点：典型的一对多的设计是在多的一方加外键


8、级联更新和级联删除
	- 添加级联更新和删除时候，需要在外键约束后面添加

	- 在删除父表中数据的时候，级联删除子表中的数据    on delete cascade
		- 删除外键约束
			alter table t_student drop foreign key t_student_classno_fk;
		- 添加外键约束
			alter table t_student add constraint t_student_classno_fk foreign key(classno) references t_class(cno) on delete cascade;
	
	- 在更新父表中数据的时候，级联更新子表中的数据	   on update cascade
		- 删除外键约束
			alter table t_student drop foreign key t_student_classno_fk;
		- 添加外键约束
			alter table t_student add constraint t_student_classno_fk foreign key(classno) references t_class(cno) on update cascade;
	
	- 以上的级联更新和级联删除，谨慎使用，因为级联操作会将数据改变或者删除

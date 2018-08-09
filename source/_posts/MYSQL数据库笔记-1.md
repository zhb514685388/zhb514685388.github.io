---
title: MYSQL数据库笔记(1)
date: 2018-08-09 14:29:40
tags:
---

mysql -uroot -p密码   登入数据库

use 数据库名

1.显示所有数据库：show databases;  
2.显示数据库下的表：show tables;
3.创建数据库：create database 库名 [charset utf-8]；
4.删除数据库：drop database 库名；
5.修改数据库名：表可以修改名，数据库不能修改名。
6、建表： create table stu(
          id int,
          sname,varchar(32
          );
7、删除表：drop table 表名；
8、修改表名：rename table stu to newstu;
9、表中插入数据：(最好用这种）
insert into newstu(id,sname) values(1,'张三');
insert into newstu(id,sname) values(2,'李四');
insert into newstu(id,sname) values(3,'王五');
或者
insert into newstu values(4,'赵六'),(5,'七');
10、清空表中数据：truncate 表名；

11、truncate和delete的区别：
truncate相当于删表再重建一张同样结构的表，操作后得到一张全新表。
而delete是从删除所有的层面来操作的，把数据删除

12、windows系统中若出现乱码：set names gbk(编码方式）

tee /home/zhb/mysql/a.sql(选择文件路径) ：用来记录用户操作记录的
#输入文字...

13.建表语句：
create table class(
id int primary key auto_increment(表示自增),
sname varchar(10) not null default '',
gender char(1) not null default '',
company varchar(20) not null default '',
salary decimal(6,2) not null default 0.00,
fanbu smallint not null default 0
);

===添加内容：
insert into class(id,sname,gender,company,salary,fanbu)values(1,'张三','男','百度',8888,234);

若没声明需要添加的字段，则默认添加所有。
insert into class
values(5,'五妹','女','新浪',9999,10);

部分声明字段
insert into class(sname,company,salary)
values
('刘备','皇室成员',14.28),
('孙策','江东集团',55.55),
('曹操','宦官后裔',33.88);


===修改表内容：
update class set gender='男',fanbu=99 where id=2;
update class set gender='男',fanbu=23 where id=3;
update class set gender='男',fanbu=43 where id=4;

#改性别为男，且工资大于8000的用户的fanbu
update class set fanbu=200 where salary>8000 and gender='男';

===删除表的内容：只能删除行，不能删除单个列的内容。
# 把工资大于8000的男的用户删除：
delete from class where salary>8000 and gender='男';

===添加列：
alter table class add score tinyint unsigned not null default 0;

 ====================================================
 


14.三大类型
	数值型
		14.1整型:
			*tinyint 
				空间：1字节；范围：-128～127,0～255 (2^8-1)
			*smallint
				空间：2字节；范围：-2^14~2^14-1,0~2^16-1
			mediumint
				空间：3字节；
			*int
				空间：4字节；
			bigint
				空间：8字节；
		注：
			不加特殊说明，默认是有符号，范围是-2^(n-1)-2^(n-1)-1,
			unsigned表示无符号，可以影响存储范围0-2^n-1
			zerofill：zero代表零，fill代表填充，表示0填充(默认unsigned类型);
			(M) M表示参数，要和zerofill结合才有意义：即不够位数，用0填充。
				例：添加一个学号，学号不能为负，学号位数相同。
				alter table class add snum smallint(5) zerofill not null default 0;
				insert into class(sname,snum) values('赵云',12);
				insert into class(sname,snum) values('典韦',14);

		14.2小数(浮点型/定点型)
			float(M,D)：浮点

			decimal(M,D)：定点,是把整数部分和小数部分，分开存储的，比float精确。
			
			M叫精度--->代表总位数，D叫标度--->代表小数位数 

			=======================================
			create table salary(
			sname varchar(20) not null default '',
			sal float(6,2) 
			);

			insert into salary values('张三',-9999.20);
			insert into salary values('李四',9999.99);

			alter table salary add bonus float(5,2) unsigned not null default 0.00; 

			insert into salary(sname,bonus) values('赵四',552);

	14.3 字符串类型
		*char:定长字符串  对于char(N),如果不够N个长度，则在末尾用空格补充，浪费空间。
		
		*varchar：变长字符串  varchar(n),存储0-n个字符，会有1-2个字节来标志该列的内容。
		
		text:文本类型，可以存较大的文本段(文章，新闻)，搜索速度稍慢，不用加默认值。
		
		blob：二进制类型，用来存储图，音频像等信息
			意义：二进制0～255,都有可能出现，防止因为字符集的问题，导致信息丢失。
			如：一张图片中有0XFF个字节，这个在ASCII字符集被认为非法，在入库时被过滤了

		char和varchar区别：
		=========================================
		create table test(
		ca char(6) not null default '',
		vca varchar(6) not null default ''
		);

		insert into test values('hello','hello');
		insert into test values('aa ','aa ');

		concat连接字符串用的 

		查看区别：
		mysql>select concat(ca,'!'),concat(vca,'!') from test;
		+----------------+-----------------+
		| concat(ca,'!') | concat(vca,'!') |
		+----------------+-----------------+
		| hello!         | hello!          |
		| aa!            | aa !            |
		+----------------+-----------------+
		char型，如果不够n个字符，内部用空格补齐，取出时再把右侧空格删掉。
		
		速度上：定长速度快些
		char(n)和varchar(n)限制的是字符，不是字节
		char(2)能存2个字符，如‘福建’，'ab'

	14.4日期/时间类型
		date 日期类型 3个字节
		time 时间类型 3个字节
		datetime 8个字节
		year 1字节
		timestamp 显示当前时间
		
		create table star(
	    sname varchar(32) not null default '',
	    brith date
	    );





15、建表过程
		-就是声明字段的过程
		-建立合理的类型，充分利用存储空间
		===============================
		ID(pk)   用户名   性别    体重   生日    工资    上次登入时间   个人简介
		================================================================

		让所有列都定长，可以极大提高查询速度
create table member(
id int(10) unsigned primary key auto_increment,
username char(20) not null default '',
gender char(1) not null default '',
weight float(4,1) unsigned not null default 0,
birth date not null,
salary float(8,2) not null default 0,
login int unsigned not null default 0(时间戳)或者可以timestamp
);
		username char(20)会造成空间的浪费，但是提高速度。
		intro char(1500)浪费太多空间，另一方面，人的简介修改频率不高，可以把它单独列出来放另一张表

		create table member(
		id int(10) unsigned primary key not null,
		username char(20),
		intro varchar(1500)
		);

		在开发中，会员的信息优化往往是把频繁用到的信息，优先考虑效率，存放到一张表中。
		不常用的信息和比较占据空间的信息，优先考虑空间战胜，存储到辅表中


16、修改表的语法
	增加列：
		*在表的最后面添加:
		alter table 表名 add 列名称 列类型 列参数;
		例：alter table member add username char(20) not null default '';
		
		*把新列指定加在某列后面:
		alter table 表名 add 列名称 列类型 列参数 after 某列名;
		例：alter table member add gender char(1) not null default '' after birth;

		*把列添加在最前面：
		alter table 表名 add 列名称 列类型 列参数 first;
		例：alter table member add id int(10) unsigned first;

	删除列：
		alter table 表名 drop 列名;
		例：alter table member drop id;

	修改列类型：
		alter table 表名 modify 列名 新的类型名 新列参数;
		alter table  member modify id  int(4) not null default '';

	修改列名及列类型：
		alter table 表名 change 旧列名 新列名 新类型 新参数;
		alter table member change id uid int(10) unsigned;

17、

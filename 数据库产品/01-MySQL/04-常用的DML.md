# 1 常用SQL

## 1.1 建表语句

### 1.1.1 MySQL建表

方式01

```sql
CREATE TABLE t_book (
	book_id VARCHAR ( 36 ) PRIMARY KEY NOT NULL,
	book_desc VARCHAR ( 255 ) DEFAULT NULL,
	book_name VARCHAR ( 255 ) DEFAULT NULL
) ENGINE = INNODB DEFAULT CHARSET = utf8mb4;
```

方式02

```sql
CREATE TABLE `t_student` (
	id BIGINT ( 20 ) PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '主键ID',
	create_time datetime DEFAULT NULL,
	name VARCHAR ( 255 ) DEFAULT NULL,
	update_time datetime DEFAULT NULL
) ENGINE = INNODB AUTO_INCREMENT = 1000001 DEFAULT CHARSET = utf8mb4;
```

### 1.1.2 Oracle建表

```properties

```



## 1.2 基本操作语句

### 4.2.1 MySQL操作语句

#### 4.2.1.1 DDL语法

```properties
创建数据库
create database dbName charset utf8;
进入你选择的库：
use databaseName;
设置编码：
set names gbk;
创建数据库表
create table student(id int,name varchar(20),age int);
create table guo(id int primary key,name varchar(20),age int);（注意添加主键）
create table guo(id int primary key not null AUTO_INCREMENT,name varchar(36),age INT);（主键非空自增）
create table guo(id int primary key not null AUTO_INCREMENT comment '字段注解',name varchar(36) comment '字段注解',age INT comment '字段注解') comment '表注解';（主键非空自增，添加注解）
删除数据库
Drop database dbName;
修改数据库：
Alter datebase db_name;
查看当前数据库服务下的库：
Show databases;
查看当前库中的表：
show tables;
删除表：
drop table table_name；
查看表的结构：
desc tableName;
添加或删除表的字段：
alter table table_name add column ages int;
alter table tb_user drop column ages
查看创建表的sql语句
show create table tableName;
清空表的数据
Truncate tableName;
更改表名
Rename table oldtable to newtable;
数据库版本

```

#### 4.2.1.2 DML语法

```properties
增：insert into users(userid, username, password) values(1, 'mysql', 'mysql');
删：delete from users where userid = 1;
改：update users set password = 'password' where userid = 1;
查：select userid, username, password from users;
过滤查：select distinct id from a
```

#### 4.2.1.3 查询模型

```properties
where：（in&not in、between and、like、is null）
group by：（max，min，sum，avg，count）
having：（磁盘原始数据使用where，内存中的结果集having；having和where的区别）
order by：（升序排列：asc默认的，降序排列：desc）
limit：（limit（偏移量，限制条目），当偏移量为0时，可以省略偏移量！）
例子：
select * from goods where shop_price in(100,12222);
select * from goods where shop_price between 12 and 66666;
select * from goods where goods_name like '诺基亚%';
select * from goods group by cat_id;
select (market_price-shop_price) as guo from goods having guo>100;
select * from goods order by shop_price limit 3;
select * from goods order by shop_price limit 0,3;
MySQL分页公式：select * from goods limit (pageNo-1)*pageRows,pageRows
注意：列是变量，可以放入函数中（大胆的把列看成变量，变量可以计算，可以放入函数中）；投影运算：本有三列，但是取出来两列；广义投影：通过投影的数据进行运算操作得到的列；As：将广义投影给一个新的变量。
```

#### 4.2.1.4 子查询

```properties
where子查询：（内层的查询结果作为外层sql的比较条件！）
from子查询：（把子查询的结果作为一个表，供你的外边的查询语句使用）
exists子查询：（子查询实际上不产生任何数据，它只返回 ture 或 false 值）
比较子查询：
Select子查询：
例子：
SELECT * FROM goods WHERE goods_id IN (SELECT max(goods_id) FROM goods GROUP BY cat_id);
select goods_name from goods where goods_id=(select max(goods_id) from goods);
select * from(select goods_name,goods_id,shop_price,cat_id from goods) as t group by cat_id;
select * from(select num from a) as t where t.num =(select max(num) from a)
select * from category where exists (select * from goods where goods.cat_id=category.cat_id);
select 学号,姓名,专业名,出生日期 from xs where 出生日期 < all(select 出生日期 from xs where 专业名='计算机');
```

#### 4.2.1.5 连接查询

```properties
语法：Select xxxx from Table1 连接查询的类型 table2 on table1xx=table2xx;
左连接：（以左边的数据为主进行查询，右表进行配对，没有的补null）永远以左表为准
右连接：（以右边的表为主要的查询，左边进行配对，没有的补null）
内连接：（以左右数据表查询，没有的不需要补null）
外连接：（MySQL不支持，oracle支持）
左外连接：（进行两张表中的人员配对？（boy表和gril表））
多表关联：
练习：
select boy.hid,boy.bname,girl.hid,girl.gname from boy right join girl on boy.hid=girl.hid;
select boy.hid,boy.bname,girl.hid,girl.gname from boy left join girl on boy.hid=girl.hid;
select boy.hid,boy.bname,girl.hid,girl.gname from boy inner join girl on boy.hid=girl.hid;
```

#### 4.2.1.6 Union查询

```properties
Union查询是：把两条或多条sql的查询结果合并成一个结果集。
select * from boy union select * from girl;
注意：
> 1.    满足的条件，各语句取出来的列要相同。
> 2.    使用union的，完全相同的列，将会被合并。Union all 可以不合并。
> 3.    在union的子句中不使用order by，sql合并后得到总的结果集排序。
```

#### 4.2.1.7 视图操作

```properties
视图的创建
第一类：create view v as select * from table;
第二类：create view v as select id,name,age from table;
第三类：create view v[vid,vname,vage] as select id,name,age from table;
查看视图信息：describe v
查看视图基本信息：show table status like 'v'
view的algorithm
视图DML操作
创建视图：create view v as select * from sun;
查看视图：select * from v;
更改视图：update student_view set name= "guo" where id = '1';
删除视图：drop view if exists student_view;
```

#### 4.2.1.8 索引操作

```properties
MVI：索引文件
索引是数据的目录，能快速定位数据的位置
Key：普通索引（提高查询速度）
primary key：主键索引
unique key：唯一索引（提高查询速度、约束作用）
fulltext：全文索引（中文环境下，全文索引无效，要分词+索引。
一般使用第三方解决方案：sphinx）
索引可以提高查询的速度，但是增删改查的速度变慢
一般在查询频繁的地方加索引
索引的长度：建索引的时候，
多列索引：把两列或多列看成一个整体，然后建索引
冗余索引：在某个列上存在多个索引
Explain 
操作索引：
查看：show index from tableName;
删除：alter table tableName drop index indexName;
增加（普通的）：alter table tableName add index indexName;
增加主键索引：alter table tableName add primary key（列名）；
删除主键索引：alter table tableName drop primary key；
```

#### 4.2.1.9 时间操作

```properties

```



#### 4.2.1.10 统计操作

1. 分组查询并根据某个字段计算和

```sql
SELECT
  a.BILL_TYPE,
  sum(a.PRICE_SUM) AS PRICE_SUM,
  sum(CASE WHEN a.BILL_TYPE = 1 THEN a.PRICE_SUM ELSE 0 END) AS PRICE_SUM,
  sum(CASE WHEN a.BILL_TYPE = 1 THEN a.TAX_SUM ELSE 0 END)  AS TAX_SUM
FROM tb_bill_info a
GROUP BY a.BILL_TYPE;
```

1. 天、周、月、年

创建表

```sql
CREATE TABLE tb_mysql_time (
	id BIGINT ( 20 ) PRIMARY KEY NOT NULL AUTO_INCREMENT COMMENT '主键ID',
	days datetime DEFAULT NULL,
	weeks datetime DEFAULT NULL,
	months datetime DEFAULT NULL,
    years datetime DEFAULT NULL 
);
```

天

```sql
SELECT
	DATE_FORMAT( days, '%Y-%m' ) as days,
	count( * ) count 
FROM
	tb_mysql_time 
GROUP BY
	days;
```

月

```sql
SELECT
	DATE_FORMAT( months, '%Y-%m' ) months,
	count( * ) count 
FROM
	tb_mysql_time 
GROUP BY
	months;
```

年

```sql

```



### 4.2.2 Oracle操作语句

#### 4..2.2.1 时间操作



#### 4.2.2.2 统计操作



### 4.2.1 MySQL&Oracle语法差异

#### 4.2.1.1 日期

```sql
to_char(a.create_time,'yyyy-mm-dd hh24:mi:ss') create_time
date_format(a.create_time,'%Y-%m-%d %H:%i:%s') create_time

trunc(a.create_time, 'dd') >=to_date(:startDate,'yyyy-mm-dd')
date_format(a.create_time, '%Y-%m-%d') >=:startDate
```



#### 4.2.1.2 左右连接

```sql
SELECT a.*, b.* from a(+) = b就是一个右连接，等同于select a.*, b.* from a right join b
SELECT a.*, b.* from a = b(+)就是一个左连接，等同于select a.*, b.* from a left join b
```



#### 4.2.1.3 分页查询

```sql

```




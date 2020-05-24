# 1 触发器

## 1.1 什么是触发器

进行数据库应用软件的开发时，我们有时会碰到表中的某些数据改变，希望同时引起其他相关数据改变的需求，
利用触发器就能满足这样的需求，它能在表中的某些特定数据变化时自动完成某些查询。运用触发器不仅可以简化程序，而且可以增加程序的灵活性。

## 1.2 触发器的应用场合

1. 当向一张表中添加或删除记录时，需要在相关表中进行同步操作。比如，当一个订单产生时，订单所购的商品的库存量相应减少。
2. 当表上某列数据的值与其他表中的数据有联系时比如，当某客户进行欠款消费，可以在生成订单时通过设计触发器判断该客户的累计欠款是否超出了最大限度。
3. 当需要对某张表进行跟踪时。比如，当有新订单产生时，需要及时通知相关人员进行处理，此时可以在订单表上设计添加触发器加以实现。

## 1.3 创建触发器四要素

监视地点（table）

监视事件（insert/delete/update）

触发时间（after/before）

触发事件（insert/delete/update）

## 1.4 触发器语法

修改语句的结束符：delimiter $

查看已有的触发器：show triggers

删除已有的触发器：drop trigger triggerName

```properties
create trigger triggerName      （触发器名称）
after/befor         （触发时间）
insert/delete/update（监视事件）
on 表名              （监视地址）
for each row
begin
	代码部分添加变量部分（非必须）
	sql1
	SqIN;
end
```

### 1.4.1 引用行变量

new、old

```properties

```

### 1.4.2 after/before

如何预防爆仓（库存出现负值），如何预防爆仓？如果购买的number1大于库存中的number2，把number1自动变成number2；

报如下错误
ERROR 1362（HY000）： Updating of NEW row is not allowed in after trigger
原因： insert之后，ne行已经插入到表中，成为事实，改new已经晚了

### 1.4.3 for each row

是声明行级处理器的，每一行都受影响。

在Oracle的触发器中，触发器分为语句触发器和行级触发器。在Oracle中是可以不写for each row的，默认是语句级的触发器。

## 1.5 触发器编程



# 2 存储过程

## 2.1 什么是存储过程

在一起语言中，如 pasca1，有一个概念叫"过程" procedure，和"函数" function。在PHP中，没有过程，只有函数。

存储过程：我们把若干条SQL封装起来，起个名字-过程，把此过程存储在数据库中---存储过程。
过程：封装了若干条语句，调用时，这些封装体执行，没有返回值的函数。
函数：是一个有返回值的过程。

## 2.2 创建存储过程

### 2.2.1 创建存储过程语法

```powershell
create procedure myproc()
begin   
    declare num int;
    set num=1;
    while num <= 10000000 do   
        insert into test_user(username,email,password) values(CONCAT('username_',num), CONCAT(num ,'@qq.com'), MD5(num));   
        set num=num+1;  
    end while;  
end
```

### 2.2.2 存储过程基础命令

```sql
查看存储过程：show procedure status \G;
删除存储过程：drop procedure procedureName;
```

### 2.2.3 变量运算和控制语句

声明变量：declare age int default 18;

赋值语法（set age := age+30;）\（select count(*) into cnt from goods;）

控制语句：顺序结构、选择结构、循环结构

### 2.2.4 存储过程传参

语法是：[in/out/inout] 参数名称 参数类型，理解下变量名和变量值！

## 2.3 调用存储过程

call procedureName(参数);

# 3 游标操作

## 3.1 什么是游标

cursor 游标 游标的标志
1条sq1，对应N条结果集的资源取出资源的接口/句柄，就是游标
沿看游标，可以一次取出1
declare声明； declare游标名 cursor for select_statement;
open打开；open游标名
fetch取值； fetch游标名 into var1，var2[]
close关闭； close游标名；

## 3.2 定义游标

```properties
create procedure p1
begin
	declare row_id int,
	declare getgoods cursor for select * from goods;
	open getgoods;
	fetch getgoods into row_id;
	close getgoods;
end;
```

## 3.3 游标循环

### 3.3.1 通过计数的方式



### 3.3.2 通过标记的方式

游标取值越界时，有没有标识？利用标识来结束
在 mysql cursor中，可以 declare continue handler来操作1个越界标识
declare continue handler for Not FOUND statement;会多取一行，同时报一个war ing。

declare exit handler for Not FOUND statement;

注意使用continue和exit的使用。	

除 continue，exit外，还有一种 undo handler，
continue是触发后，后面的语句继续执行
exit是触发后，后面的语句不再执行
undo是触发后，前面的语句撤消（但是，目前mysq还不支持und）

# 4 数据库权限

## 4.1 权限检查原理

[用户]==>[服务器]

权限的认证有两个阶段：你有没有权连接服务；你有没有执行操作的权限（curd）。

主要通过三张表控制权限：user、db、tables_priv

### 4.1.1 权限服务连接

你从哪里来？你是谁？你的密码是？

尽量避免问题的发生，而不是防止问题的发生更好。

用户的这三个信息，存储在mysql库中的user表下。

mysql -h127.0.0.1 -uroot -ppassw0rd

```shell
update user set host='127.0.0.1' where user='root';
update user set user='dev' where user='root';
update user set password=password('111111') where user='root';
flush privileges;
```

让局域网都可以连接

```shell
update user set host='192.168.100.%' where user='root';
```

### 4.1.2 操作的权限

grand [权限1,权限2,权限3...] on 哪个数据库.哪些表 to 哪个用户@'host' identified by '111111'

常用权限：all（所有权限）、create、drop、insert、delete、update、select

## 4.2 全局授权和回收

### 4.2.1 全局授权

例如：`grand all on *.* to guo@'192.168.100.%' identified '111111'`

### 4.2.2 收回权限

例如：`revoke all on *.* from guo@'192.168.100.%'` 

## 4.3 局部授权和回收

### 4.3.1 局部授权

例如：`grand all on test.* to guo@'192.168.100.%' identified '111111'`

### 4.3.2 局部回收

例如：`revoke all on test.* from guo@'192.168.100.%'` 

## 4.4 表授权和回收

### 4.4.1 表授权

例如：`grand instert,update,select on test.goods to guo@'192.168.100.%' `

### 4.4.2 表回收

例如：`revoke instert,update,select on test.goods from guo@'192.168.100.%'` 

# 5 数据库备份还原

## 5.1 mysqldump命令

### 5.1.1 备份

mysqldump -uroot -ppassw0rd dbName table01 table02 >D:/test.sql

mysqldump -uroot -ppassw0rd dbName >D:/test.sql

mysqldump -uroot -ppassw0rd --all-databases >D:/test.sql

### 5.1.2 还原

mysqldump -uroot -ppassw0rd dbName <D:/test.sql

## 5.2 直接复制数据库目录



## 5.3 mysqlhotcopy工具备份



# 6 主从数据库

## 6.1 主从库原理

主从分离，主服务上产生binlog文件，从服务器读取binlog文件为relog文件。

主服务器要配置binlog，从服务器要配置relog。

从服务是如何有权读取binlog的，master要授予slave账号。

从服务使用该账号连接master。

## 6.2 主从配置过程

### 6.2.1 配置主从服务

配置主MySQL的配置文件my.cnf

```properties
server-id=201         // 服务器特有的id
log-bin=mysql-bin     // 声明二进制日志文件为mysql-bin.xxx
binlog-format=mixed   // 二进制的格式：mixed(混合的)、row(磁盘变化)、statement(执行语句)
```

语句长而磁盘变化少，宜用row；语句短但影响上万行，磁盘变化大，宜用statement。

配置从MySQL的配置文件my.cnf

```properties
server-id=202         // 服务器特有的id
log-bin=mysql-bin     // 声明二进制日志文件为mysql-bin.xxx
binlog-format=mixed   // 二进制的格式：mixed(混合的)、row(磁盘变化)、statement(执行语句)
relay-log=mysql-relay // 
```

语句长而磁盘变化少，宜用row；语句短但影响上万行，磁盘变化大。

### 6.2.2 启动主从服务器

```properties
mysql server start
mysqld_safe --user=mysql &
```

## 6.3 启动主从

show master status;    // 是否具备主服务器的条件，就是产生binlog。

show slave status;       // 是否具备从服务器的条件。

```properties
// 低版本
grant replication client,replication slave on *.* to guodd@'192.168.10.%' identified by 'passw0rd';

// 高版本的MySQL数据库需要分开写
create user 'guodd'@'192.168.10.*' identified by 'passw0rd';
grant all privileges on *.* to 'guodd'@'192.168.10.*' with grant option;

flush privileges;
```





```properties
change master to 
master_host='192.168.10.50',
master_user='guodd',
master_password='passw0rd',
master_log_file='mysql-bin.000001',
master_log_pos=872;
```

查看从服务器状态

show slave status \G;

start slave;   // 启动从服务器的功能

stop slave;

**1. 先停止slave
**mysql>stop slave;

**2. 跳过slave上的1个错误
**mysql>set global sql_slave_skip_counter=1;

**3.在slave上手工插入一条数据
**mysql>insert into aa values(1,'Java');



# 7 宕机情况

## 7.1 主库宕机

(1)确保所有的relay log全部更新完毕，在每个从库上执行show processlist;

(2)更新完毕后，登录所有从库查看master.info文件，对比选择pos最大的作为新的主库，

(3)然后登录这个新的主库，执行stop slave；进入主目录，删除master.Info和relay-log.info配置my.cnf文件开启log-bin文件

(4)创建用于同步的用户并授权slave

(5)登录另外一台从库，执行stop slave停止同步

(6)执行start slave

(7)修改新的master数据，测试slave是否同步更新

## 7.2 从库宕机

(1)查看从库上mysql的错误日志，里面有记录主从挂掉时的binlog信息。

(2)有了binlog和postion信息后，只需要重新在从库上进行change master to配置即可。配置后开启slave状态，没有报错

(3)查看slave状态，发现slave已经正常了，开始进行延时数据恢复。

# 1 Windows环境

Stable Version：MySQL-8.0.19

## 1.1 安装版



## 1.2 绿色版

1、MySQL8.0.16解压

其中`dada`文件夹和`my.ini`配置文件是解压后手动加入的，如下图所示

2、新建配置文件`my.ini`放在`D:\Free\mysql-8.0.16-winx64`目录下

```properties
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
# 时区为东八区
default-time_zone='+8:00'
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=D:/dev/soft/SQL/mysql-8.0.19-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:/dev/soft/SQL/mysql-8.0.19-winx64/data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=20
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 忽略密码
# skip-grant-tables
```

3、初始化MYSQL配置

管理员身份打开Windows PowerShell，并进入`D:\Free\mysql-8.0.16-winx64\bin`目录，执行如下命令：

```
mysqld --initialize --console
```

4、安装MySQL服务，并启动服务

安装服务的命令为：mysqld --install 服务名，由于我电脑已配置安装了mysql服务，此处用mysql8作为服务名，如下所示

```
mysqld --install mysql8
```

启动服务命令为：net start 服务名

```
net start mysql8
```

5、登录MySQL并修改root密码

使用默认分配的密码（即diK3i1dH=k8b）进行登录

```
mysql -uroot -pdiK3i1dH=k8b
```

登录成功后，修改密码为password

```
alter user 'root'@'localhost' IDENTIFIED BY 'password';
```

刷新一下即可

```
flush privileges;
```

show variables like '%time_zone%';
set global time_zone = '+8:00'; 
set persist time_zone='+8:00';

# 2 Linux环境

## 2.1 安装配置

### 2.1.1 下载

[下载地址](www.mysql.com)

### 2.1.2 配置/etc/my.cnf

```properties
[client]
port=3306
socket=/tmp/mysql.sock
default-character-set = utf8

[mysqld]
default-time_zone='+8:00'
port=3306
user=mysql
default-character-set = utf8
socket=/tmp/mysql.sock
basedir=/opt/soft/mysql/mysql-8.0.15-el7
datadir=/opt/soft/mysql/mysql-8.0.15-el7/data
log-error=/opt/soft/mysql/mysql-8.0.15-el7/logs/error.log
# 表名的大小写
lower_case_table_names=1
# 允许最大连接数
max_connections=200
# 允许连接失败的次数
max_connect_errors=10
sql_mode='STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION'
```

特别注意：socket=/tmp/mysql.sock的位置，如果配置错误，MySQL服务将启动失败！但是要根据情况添加，有些服务是不能添加的，如果添加的话会报错！这个错误启动MySQL和连接MySQL都会出现的。

## 2.1.3. 初始化数据库

```properties
groupadd mysql
useradd -g mysql -r mysql
useradd -g mysql mysql -p passw0rd

chown -R mysql:mysql /tmp/mysql
chown -R mysql:mysql /opt/soft/mysql/mysql-8.0.15-el7

yum install -y libaio

./bin/mysqld --initialize --user=mysql --basedir=/opt/soft/mysql/mysql-8.0.15-el7 --datadir=/opt/soft/mysql/mysql-8.0.15-el7/data/
```

## 1.4. 查看初始化密码

```properties
cat /usr/local/soft/mysql/mysql-8.0.15/logs/error.log
```

## 1.5. 配置环境变量

```properties
vim /etc/profile // 打开profile文件。
export MYSQL_HOME
MYSQL_HOME=/opt/soft/mysql/mysql-8.0.15-el7
export PATH=$PATH:$MYSQL_HOME/lib:$MYSQL_HOME/bin 

source /etc/profile
```

## 1.6. 登录修改root密码

```properties
mysql -uroot -p
alter user 'root'@'localhost' identified by 'your_password';
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密码';
```

## 1.7. 开机启动

```properties
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql //添加可执行权限。
chkconfig --add mysql // 注册启动服务
service mysql start
```

# 2 配置远程登录

## 2.1. 改表法

```properties
use mysql;
update user set host = '%' where user = 'root';
```

## 2.2 配置账号授权

```properties
use mysql;
select host,user,plugin from user;
create user 'dev'@'%' identified by 'passw0rd';   // 创建账号
// MySQL 8中必须执行否则远程连接不上
ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; 
grant all privileges on *.* to 'username'@'%';  // 授权
grant all privileges on onedatabase.* to 'username'@'%';
```

## 2.3 远程连接

```properties
mysql -h192.168.100.11 -P3306 -uroot -ppassw0rd
```

# 4. 找回密码

## 4.1 步骤

```properties
1.vi /etc/my.cnf，在[mysqld]中添加
skip-grant-tables
例如：
[mysqld]
skip-grant-tables
datadir=/var/lib/mysql
socket=/var/lib/mysql/mysql.sock
2.重启mysql
service mysql restart
3.使用用户无密码登录
mysql -uroot -p (直接点击回车，密码为空)
4.选择数据库
use mysql;
5.修改root密码
update user set authentication_string='passw0rd' where user='root';
update user set authentication_string=password('123456') where user='root';
6.刷新权限
flush privileges;
7.退出
exit;
8.删除第1部增加的配置信息
skip-grant-tables
9.重启mysql
service mysql restart
```

# 5. 数据库版本

## 5.1 版本信息

```properties
select version();
```

## 5.2 编码规则

```properties
show variables where variable_name like 'character\_set\_%' or variable_name like 'collation%';
```

# 6. 磁盘情况

## 6.1 库所占磁盘大小

```sql
select 
TABLE_SCHEMA, 
concat(truncate(sum(data_length)/1024/1024,2),' MB') as data_size,
concat(truncate(sum(index_length)/1024/1024,2),'MB') as index_size
from information_schema.tables
group by TABLE_SCHEMA
ORDER BY data_size desc;
order by data_length desc;
```

## 6.2 表所占磁盘大小

```sql
select 
TABLE_NAME, 
concat(truncate(data_length/1024/1024,2),' MB') as data_size,
concat(truncate(index_length/1024/1024,2),' MB') as index_size
from information_schema.tables 
where TABLE_SCHEMA = 'mysql'
group by TABLE_NAME
order by data_length desc;
```

# 7. 错误信息

## 7.1 远程连接

报错信息：

```properties
ERROR 2059 (HY000): Plugin caching_sha2_password could not be loaded: 找不到指定的模块。
```

解决方法

MySQL 5.7

```properties
# {usernama} 是远程访问登录的用户名，不建议用 root;
# {password} 是远程访问的登录密码;
# '%'代表的是所有IP，如果可以尽量设置指定 IP 或 IP 段
GRANT ALL PRIVILEGES ON *.* TO 'username'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION;
# 刷新权限
FLUSH PRIVILEGES;
```

MySQL 8

```properties
# 创建一个账号，用来进行远程访问；
# {usernama} 是远程访问登录的用户名，不建议用 root;
# {password} 是远程访问的登录密码;
# '%'代表的是所有IP，如果可以尽量设置指定 IP 或 IP 段
CREATE USER 'username'@'%' IDENTIFIED BY 'password';

# 赋予所有权限给之前创建的账号
GRANT ALL ON *.* TO 'username'@'%';

# 确认使用这里的密码登录此账号
ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password';

# 刷新权限
FLUSH PRIVILEGES;
```



## 7.2 MySQL 8 

报错信息：

```properties
this is incompatible with sql_mode=only_full_group_by
```

> 原因以及解决：8.0以上已经取消了NO_AUTO_CREATE_USER这个关键字，删掉sql语句中的这个关键字即可。

解决问题：

```sql
select @@global.sql_mode;
SET GLOBAL sql_mode=(SELECT REPLACE(@@sql_mode,'ONLY_FULL_GROUP_BY',''));
```

## 7.3 时区错误

```properties
Server returns invalid timezone. Go to 'Advanced' tab and set 'serverTimezon'
The server time zone value 'EDT' is unrecognized or represents more than one time zone.
```

时区错误，MySQL默认的时区是UTC时区，比北京时间晚`8`个小时。

所以要修改mysql的时长，在mysql的命令模式下，输入：

```properties
show variables like '%time_zone%';
set time_zone=SYSTEM;
set global time_zone='+8:00';
flush privileges;
```

## 7.4 最大连接数

max_connections：mysql的最大连接数。

max_used_connections：针对某一个账号的所有客户端并行连接到MYSQL服务的最大并行连接数。

在 show global 里有个系统状态Max_used_connections,它是指从这次mysql服务启动到现在，同一时刻并行连接数的最大值。它不是指当前的连接情况，而是一个比较值。如果在过去某一个时刻，MYSQL服务同时有1000个请求连接过来，而之后再也没有出现这么大的并发请求时，则Max_used_connections=1000。

对于mysql服务器最大连接数值的设置范围比较理想的是：服务器响应的最大连接数值占服务器上限连接数值的比**例值在10%以上**，如果在10%以下，说明mysql服务器最大连接上限值设置过高。

**公式：Max_used_connections / max_connections * 100%**

计算得到，差不多在5.3%作用，所以最大连接数设置有些偏高。可以适当的降低

设置方法：vim /etc/my.cnf  然后添加一句 max_connections=80

这个时候设置的是最理想的。

当然在现实项目中，出现高并发的问题，不可能按着当前的服务器响应最大连接数去设置，应该将最大连接数设置到最大，等项目差不多稳定了，或者在日志中分析出，高并发的数量，从而去调整最大连接数。
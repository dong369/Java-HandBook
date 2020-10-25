# 1 Win环境

Stable Version：MySQL-8.0.21

## 1.1 安装版

安装正常软件的安装流程，进行下一步下一步操作。

## 1.2 绿色版

1、MySQL8.0.21解压，其中`dada`文件夹和`my.ini`配置文件是解压后手动加入的，如下图所示

2、新建配置文件`my.ini`放在`D:\dev\soft\SQL\mysql-8.0.21-winx64`目录下

```properties
[mysqld]
# 设置3306端口
port=3306
# 时区为东八区
default-time_zone='+8:00'
# 设置mysql的安装目录
basedir=D:\dev\soft\SQL\mysql-8.0.21-winx64
# 设置mysql数据库的数据的存放目录
datadir=D:\dev\soft\SQL\mysql-8.0.21-winx64\data
# 允许最大连接数
max_connections=200
# 允许连接失败的次数。
max_connect_errors=10
# 服务端使用的字符集默认为utf8mb4
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
#mysql_native_password
default_authentication_plugin=mysql_native_password

[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

3、初始化MYSQL配置

管理员身份打开，并进入`D:\dev\soft\SQL\mysql-8.0.21-winx64\bin`目录，执行如下命令：

```properties
mysqld --initialize --console
```

4、安装MySQL服务，并启动服务，安装服务的命令为：mysqld --install 服务名，由于我电脑已配置安装了mysql服务，此处用mysql8.0.21作为服务名。

```properties
mysqld --install mysql8.0.21
```

5、启动服务命令为：net start 服务名

```bash
net start mysql8.0.21
```

5、登录MySQL并修改root密码，使用默认分配的密码（即diK3i1dH=k8b）进行登录，第三步会打印密码

```sql
mysql -uroot -pdiK3i1dH=k8b
```

6、登录成功后，修改密码为password

```sql
mysql -uroot -pdiK3i1dH=k8b
alter user 'root'@'localhost' IDENTIFIED BY 'passw0rd';
```

7、刷新一下即可

```sql
show variables like '%time_zone%';
set global time_zone = '+8:00';   // Asia/Shanghai或者GMT+8
set persist time_zone='+8:00';
flush privileges;
```

## 1.3 脚本安装

```bash
@echo off
echo 初始化MySQL服务

set Pan=D:\
set PanDir=dev\soft\SQL\mysql-8.0.21-winx64

rd /s /q %Pan%%PanDir%\data

cd /d %Pan%%PanDir%/bin
mysqld --remove MySQL-8.0.21
mysqld --initialize-insecure --user=mysql
mysqld --install MySQL-8.0.21

pause
exit
```



```bash
@echo off
echo 启动MySQL服务...
set Pan=D:\
set PanDir=dev\soft\SQL\mysql-8.0.21-winx64

net start MySQL-8.0.21

cd /d %Pan%%PanDir%/bin
mysql -uroot -p

pause
exit

@echo off
echo 停止MySQL服务...

net stop MySQL-8.0.21

pause
exit

@echo off
echo 删除MySQL服务

set Pan=D:\
set PanDir=dev\soft\SQL\mysql-8.0.21-winx64\bin\

cd /d %Pan%%PanDir%
mysqld --remove MySQL-8.0.21

pause
exit
```



# 2 Linux环境

## 21 下载

[下载地址](www.mysql.com)

## 2.2 卸载

```properties
1、如果安装过MySQL（每个文件都要移除）
输入1（获取文件列表）：rpm -qa | grep mysql
输出1：文件名列表
输入2（删除文件，文件名列表中的都要删除）：yum remove 文件名
输出2：略
输入3（删除mysl配置文件）：rm -rf /var/lib/mysql、rm /etc/my.cnf、rm -rf /usr/share/mysql-8.0

2、查看自带数据库（输出与系统版本相关）
输入：rpm -qa | grep mariadb
输出：mariadb-libs-5.5.64-1.el7.x86_64
 
3、卸载自带数据库（数据库名以上条语句输出为准）
输入：rpm -e --nodeps mariadb-libs-5.5.64-1.el7.x86_64
输出：无
 
4、安装依赖
输入：yum install -y libaio perl net-tools
输出：略
```

## 2.3 my.cnf

配置/etc/my.cnf

```properties
[mysqld]
# 设置3306端口
port=3306
# 设置mysql的安装目录
basedir=/opt/soft/db/mysql
# 设置mysql数据库的数据的存放目录，指定自己的文件
datadir=/opt/soft/db/mysql/data
log-error=/opt/soft/db/mysql/logs/error.log
# 允许最大连接数
max_connections=10000
# 允许连接失败的次数。这是为了防止有人从该主机试图攻击数据库系统
max_connect_errors=10
# 服务端使用的字符集默认为UTF8
character-set-server=utf8mb4
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
# 默认使用“mysql_native_password”插件认证
default_authentication_plugin=mysql_native_password
[mysql]
# 设置mysql客户端默认字符集
default-character-set=utf8mb4
[client]
# 设置mysql客户端连接服务端时默认使用的端口
port=3306
default-character-set=utf8mb4
```

## 2.4 用户权限

```properties
groupadd mysql
useradd -g mysql mysql（useradd -g mysql -r mysql）
passwd mysql

chown -R mysql:mysql /tmp/mysql
chown -R mysql:mysql /opt/soft/mysql/mysql-8.0.15-el7
chmod -R 777 /opt/soft/db/mysql
```

## 2.5 配置环境变量

```properties
vim /etc/profile
export MYSQL_HOME=/opt/soft/db/mysql
export PATH=$PATH:$MYSQL_HOME/lib:$MYSQL_HOME/bin

source /etc/profile
```

## 2.6 初始化数据库

```properties
mysqld --initialize --console（./bin/mysqld --initialize --user=mysql --basedir=/opt/soft/db/mysql --datadir=/opt/soft/db/mysql/data/）一般使用简写方式
```

## 2.7 启动服务

```properties
cd support-files/
./mysql.server start
```

## 2.8 查看初始化密码

```properties
cat /usr/local/soft/mysql/mysql-8.0.15/logs/error.log
```

## 2.9 登录修改root密码

```properties
mysql -uroot -p
alter user 'root'@'localhost' identified by 'your_password';

ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '你的密码';
alter user  'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'passw0rd';
```

## 2.10 开机启动

```properties
方式1
cp /usr/local/db/mysql/support-files/mysql.server /etc/init.d/mysql
vim /usr/lib/systemd/system/mysqld.service
[Unit]
Description=mysql server
After=network.target remote-fs.target nss-lookup.target

[Service]
#User=mysql
Type=forking
ExecStart=/etc/init.d/mysqld start
ExecReload=/etc/init.d/mysqld restart
ExecStop=/etc/init.d/mysqld stop

[Install]
WantedBy=multi-user.target

systemctl daemon-reload

方式2：
chmod +x /etc/init.d/mysql //添加可执行权限。
chkconfig --add mysql      // 注册启动服务
service mysql start
```

# 3 配置远程登录

## 3.1 改表法

```properties
use mysql;
update user set host = '%' where user = 'root';
flush privileges;
```

## 3.2 配置账号授权

```properties
use mysql;
select host,user,plugin from user;
create user 'dev'@'%' identified by 'passw0rd';   // 创建账号

// MySQL 8中必须执行否则远程连接不上
ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; 
grant all privileges on *.* to 'username'@'%';  // 授权
grant all privileges on onedatabase.* to 'username'@'%';
```

## 3.3 远程连接

```properties
mysql -h 192.168.100.11 -P3306 -uroot -ppassw0rd
```

# 4 找回密码

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

# 5 数据库版本

## 5.1 版本信息

```properties
select version();
```

## 5.2 编码规则

```properties
show variables where variable_name like 'character\_set\_%' or variable_name like 'collation%';
```

# 6 磁盘情况

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

# 7 入坑

## 7.1 启动报错

情况一、Starting MySQL.. ERROR! The server quit without updating PID file (/usr/local/mysql/data/localhost.localdomain.pid).

情况二、特别注意：socket=/tmp/mysql.sock的位置，如果配置错误，MySQL服务将启动失败！但是要根据情况添加，有些服务是不能添加的，如果添加的话会报错！这个错误启动MySQL和连接MySQL都会出现的。

## 7.2 远程连接

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

## 7.3 MySQL 8 

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

## 7.4 时区错误

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

## 7.5 最大连接数

max_connections：mysql的最大连接数。

max_used_connections：针对某一个账号的所有客户端并行连接到MYSQL服务的最大并行连接数。

在 show global 里有个系统状态Max_used_connections,它是指从这次mysql服务启动到现在，同一时刻并行连接数的最大值。它不是指当前的连接情况，而是一个比较值。如果在过去某一个时刻，MYSQL服务同时有1000个请求连接过来，而之后再也没有出现这么大的并发请求时，则Max_used_connections=1000。

对于mysql服务器最大连接数值的设置范围比较理想的是：服务器响应的最大连接数值占服务器上限连接数值的比**例值在10%以上**，如果在10%以下，说明mysql服务器最大连接上限值设置过高。

**公式：Max_used_connections / max_connections * 100%**

计算得到，差不多在5.3%作用，所以最大连接数设置有些偏高。可以适当的降低

设置方法：vim /etc/my.cnf  然后添加一句 max_connections=80

这个时候设置的是最理想的。

当然在现实项目中，出现高并发的问题，不可能按着当前的服务器响应最大连接数去设置，应该将最大连接数设置到最大，等项目差不多稳定了，或者在日志中分析出，高并发的数量，从而去调整最大连接数。

## 7.6 版本差异



# 8 存储对比

## 8.1 MySQL



## 8.2 Polardb



## 8.3 Greenplum



## 8.4 PostgreSQL



## 8.5 Tidb



## 8.6 Mariadb



## 8.7 达梦




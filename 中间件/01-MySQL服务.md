## 1. 安装配置

### 1.1. 下载

[下载地址](www.mysql.com)

### 1.2. 配置/etc/my.cnf

```properties
[client]
port=3306
socket=/tmp/mysql.sock

[mysqld]
port=3306
user=mysql
socket=/tmp/mysql.sock
basedir=/opt/soft/mysql/mysql-8.0.15-el7
datadir=/opt/soft/mysql/mysql-8.0.15-el7/data
log-error=/opt/soft/mysql/mysql-8.0.15-el7/logs/error.log
lower_case_table_names=1
```

特别注意：socket=/tmp/mysql.sock的位置，如果配置错误，MySQL服务将启动失败！

### 1.3. 初始化数据库

```properties
groupadd mysql
useradd -g mysql -r mysql
useradd -g mysql mysql -p 123456

chown -R mysql:mysql /tmp/mysql
chown -R mysql:mysql /opt/soft/mysql/mysql-8.0.15-el7

./mysqld --initialize --user=mysql --basedir=/opt/soft/mysql/mysql-8.0.15-el7 --datadir=/opt/soft/mysql/mysql-8.0.15-el7/data/
```

### 1.4. 查看初始化密码

```properties
cat /usr/local/soft/mysql/mysql-8.0.15/logs/error.log
```

### 1.5. 配置环境变量

```properties
vim /etc/profile // 打开profile文件。
export MYSQL_HOME
MYSQL_HOME=/opt/soft/mysql/mysql-8.0.15-el7
export PATH=$PATH:$MYSQL_HOME/lib:$MYSQL_HOME/bin 
```

### 1.6. 修改root密码

```properties
alter user 'root'@'localhost' identified by 'your_password';
```

### 1.7. 开机启动

```properties
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql //添加可执行权限。
chkconfig --add mysql // 注册启动服务
service mysql start
```

## 2. 配置远程登录账号

### 2.1. 改表法

```properties
update user set host = '%' where user = 'root';
```



```properties
mysql -h 192.168.100.11 -P3306 -uroot -ppassw0rd
use mysql;
select user,host,plugin from user;
CREATE USER 'new_user'@'%' IDENTIFIED BY 'passwd';
GRANT ALL ON *.* TO 'new_user'@'%';
flush privileges;
SHOW VARIABLES WHERE Variable_name = 'version';
```

## 3. 配置账号授权

```properties
select host,user,plugin from user;
create user 'username'@'%' identified by 'password';
ALTER USER 'username'@'%' IDENTIFIED WITH mysql_native_password BY 'password'; 
grant all privileges on *.* to 'username'@'%';
grant all privileges on onedatabase.* to 'username'@'%';
```

## 4. 找回密码

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

## 5. 数据库版本


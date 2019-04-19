第一步：vim /etc/my.cnf
[client]
port=3306
socket=/tmp/mysql/mysql.sock

[mysqld]
port=3306
user=mysql
socket=/tmp/mysql/mysql.sock
basedir=/usr/local/soft/mysql/mysql-8.0.15
datadir=/usr/local/soft/mysql/mysql-8.0.15/data
log-error=/usr/local/soft/mysql/mysql-8.0.15/logs/error.log
lower_case_table_names=1

第二步：初始化
groupadd mysql
useradd -g mysql mysql

mkdir -p /tmp/mysql
chown -R mysql:mysql /tmp/mysql
chown -R mysql:mysql ./

./mysqld --initialize --user=mysql --basedir=/usr/local/soft/mysql/mysql-8.0.15 --datadir=/usr/local/soft/mysql/mysql-8.0.15/data/

第三步：查看初始化密码
cat /usr/local/soft/mysql/mysql-8.0.15/logs/error.log

第四步：修改root密码
alter user 'root'@'localhost' identified by 'your_password';

第五步：开机启动
cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysql
chmod +x /etc/init.d/mysql //添加可执行权限。
chkconfig --add mysql // 注册启动服务

第六步：配置环境变量
vim /etc/profile // 打开profile文件。
export MYSQL_HOME
MYSQL_HOME=/usr/local/mysql
export PATH=$PATH:$MYSQL_HOME/lib:$MYSQL_HOME/bin 

第七步：配置远程登录账号
mysql -u root -p
use mysql;
select user,host,plugin from user;
CREATE USER 'new_user'@'%' IDENTIFIED BY 'passwd';
GRANT ALL ON *.* TO 'new_user'@'%';
flush privileges;
SHOW VARIABLES WHERE Variable_name = 'version';
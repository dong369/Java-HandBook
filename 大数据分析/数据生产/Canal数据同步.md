# 1 下载

stable version：[canal.deployer-1.1.4.tar.gz](https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.deployer-1.1.4.tar.gz)

## 1.1 工作原理

- canal 模拟 MySQL slave 的交互协议，伪装自己为 MySQL slave ，向MySQL master发送dump协议
- MySQL master收到dump 请求，开始推送 binary log 给slave (即canal )
- canal 解析 binary log 对象(原始为 byte 流)

# 2 配置

## 2.1 基础环境

JDK、Zookeeper、Kafka、MySQL8、主服务器开启主从配置

## 2.2 MySQL主从配置

### 2.2.1 配置主从服务

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

### 2.2.2 启动主从服务器

```properties
mysql server start
mysqld_safe --user=mysql &
```

### 2.2.3 启动主从

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

## 2.2 Canal配置文件

### 2.2.1 canal.properties

```properties
# ZK配置
canal.zkServers = 127.0.0.1:2181
# 默认值tcp，这里改为投递到Kafka
canal.serverMode = kafka
# Kafka bootstrap.servers，可以不用写上全部的brokers
canal.mq.servers = 127.0.0.1:9092
```

### 2.2.2 instance.properties

```properties
# MySQL中会配置
canal.instance.mysql.slaveId=50
# MySQL数据库地址
canal.instance.master.address=192.168.10.50:3306
canal.instance.dbUsername=guodd
canal.instance.dbPassword=passw0rd
canal.instance.filter.regex=.*\\..*
canal.mq.topic=test
```

# 3 启动服务

## 3.1 基础服务

zookeeper、kafka、MySQL8

## 3.2 Canal服务

```properties
./bin/startup.sh
./bin/stop.sh
```
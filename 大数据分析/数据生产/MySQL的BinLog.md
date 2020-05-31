# 1 下载

stable version：[canal.deployer-1.1.4.tar.gz](https://github.com/alibaba/canal/releases/download/canal-1.1.4/canal.deployer-1.1.4.tar.gz)

## 1.1 工作原理

- canal 模拟 MySQL slave 的交互协议，伪装自己为 MySQL slave ，向MySQL master发送dump协议
- MySQL master收到dump 请求，开始推送 binary log 给slave (即canal )
- canal 解析 binary log 对象(原始为 byte 流)

# 2 配置

## 2.1 基础环境

JDK、Zookeeper、Kafka、MySQL8

## 2.2 配置文件

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

zookeeper、kafka、MySQL8（配置参考MySQL高级）

## 3.2 Canal服务

```properties
./bin/startup.sh
./bin/stop.sh
```
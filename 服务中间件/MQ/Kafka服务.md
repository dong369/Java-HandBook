## 1. Kafka介绍

### 1.1 官网下载

[Kafka下载地址](https://redis.io/download)

下面教程使用的版本是：kafka_2.11-2.1.1.tgz

### 1.2 安装配置

#### 1.2.1 基础环境

> Java开发环境和zookeeper服务。

#### 1.2.2 进行安装

> 01、上传kafka_2.11-2.1.1.tgz
>
> 02、解压到指定的文件夹中（本人解压到/usr/local/soft下）
>
> 03、创建kafka的日志目录

```properties
tar -zxvf kafka_2.11-2.1.1.tgz
cd /usr/local/kafka_2.11
make kafka-logs
```

#### 1.2.3 server.properties

> 01修改daemonize yes，使用yes启用守护进程
>
> 02修改log.dirs=/usr/local/kafka/kafka-logs
>
> 03修改zookeeper.connect=localhost:2181

### 1.3 启动使用

#### 1.3.1 kafka启动

> 直接运行bin/redis-server将以前端模式启动，前端模式启动的缺点是ssh命令窗口关闭则redis-server程序结束，不推荐使用此方法。

```properties
启动Zookeeper： 
bin/zookeeper-server-start.sh -daemon config/zookeeper.properties

启动Kafka
bin/kafka-server-start.sh -daemon config/server.properties &

创建TOPIC
bin/kafka-topics.sh --create --zookeeper 192.168.100.15:2181 -replication-factor 1 --partitions 1 --topic mytopic 

创建生产者：
bin/kafka-console-producer.sh --broker-list 192.168.100.16:9092 --topic mytopic

创建生产者：
bin/kafka-console-consumer.sh --bootstrap-server 192.168.100.16:9092 --topic mytopic 
bin/kafka-console-consumer.sh --bootstrap-server 192.168.100.16:9092 --topic test --from-beginning

查看topic列表：
bin/kafka-topics.sh --list --zookeeper 192.168.100.15:2181

查看是否启动（两种方式）
jps
ps -ef|grep kafka
```

## 2. 客户端连接

### 2.1 官网下载客户端

[Kafka Tool客户端](http://www.kafkatool.com/download.html)

### 2.2 安装配置



### 2.3 连接Kafka服务



## 3. Kafka高可用

[TOC]

## 1. Zookeeper介绍

### 1.1 官网下载

[Zookeeper下载地址](https://www.apache.org/dyn/closer.cgi/zookeeper/)

下面教程使用的版本是：zookeeper-3.4.13.tar.gz

### 1.2 安装配置

#### 1.2.1 基础环境

> Java环境，需要配置全局Java环境变量。

#### 1.2.2 进行安装

> 01、上传zookeeper-3.3.6.tar.gz
>
> 02、解压到指定的文件夹中（本人解压到/usr/local/soft下）
>
> 03、创建数据文件夹和日志文件夹
>
> 04、安装目录下conf/zoo_sample.cfg重命名zoo.cfg，注意养成备份的习惯
>

```properties
tar -zxvf zookeeper-3.4.13.tar.gz
cd /usr/local/soft/zookeeper-3.4.13
make data log
mv config/zoo_sample.cfg zoo.cfg
```

#### 1.2.3 zoo.cfg

> 01、修改dataDir=/usr/local/soft/zookeeper-3.3.6/data
>
> 02、修改dataLogDir=/usr/local/soft/zookeeper-3.3.6/log
>
> 03、修改clientPort 2181，监听端口

```properties
# The number of milliseconds of each tick
tickTime=2000
# The number of ticks that the initial
# synchronization phase can take
initLimit=10
# The number of ticks that can pass between
# sending a request and getting an acknowledgement
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just
# example sakes.
dataDir=/usr/local/soft/zookeeper-3.4.13/data
dataLogDir=/usr/local/soft/zookeeper-3.4.13/log
# the port at which the clients will connect
clientPort=2181
# the maximum number of client connections.
# increase this if you need to handle more clients
#maxClientCnxns=60
#
# Be sure to read the maintenance section of the
# administrator guide before turning on autopurge.
#
# http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_maintenance
#
# The number of snapshots to retain in dataDir
#autopurge.snapRetainCount=3
# Purge task interval in hours
# Set to "0" to disable auto purge feature
#autopurge.purgeInterval=1
```



### 1.3 启动使用

#### 1.3.1 启动命令

> 直接运行

```properties
启动ZK服务: sh bin/zkServer.sh start
停止ZK服务: sh bin/zkServer.sh stop
重启ZK服务: sh bin/zkServer.sh restart
查看ZK服务状态: sh bin/zkServer.sh status
```

## 2. Zookeeper高可用


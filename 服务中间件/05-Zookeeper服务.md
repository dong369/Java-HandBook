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
tar -zxvf zookeeper-3.3.6.tar.gz
cd /usr/local/soft/zookeeper-3.3.6
make data log
mv config/zoo_sample.cfg zoo.cfg
```

#### 1.2.3 zoo.cfg

> 01、修改dataDir=/usr/local/soft/zookeeper-3.3.6/data
>
> 02、修改dataLogDir=/usr/local/soft/zookeeper-3.3.6/log
>
> 03、修改clientPort 8181，监听端口

### 1.3 启动使用

#### 1.3.1 启动命令

> 直接运行bin/redis-server将以前端模式启动，前端模式启动的缺点是ssh命令窗口关闭则redis-server程序结束，不推荐使用此方法。

```properties
启动ZK服务: sh bin/zkServer.sh start
停止ZK服务: sh bin/zkServer.sh stop
重启ZK服务: sh bin/zkServer.sh restart
查看ZK服务状态: sh bin/zkServer.sh status
```

## 2. Zookeeper高可用


[TOC]

## 1. Redis介绍

### 1.1 官网下载

[Redis下载地址](https://redis.io/download)

下面教程使用的是：redis-5.0.3.tar.gz

### 1.2 安装配置

#### 1.2.1 基础环境

> redis是C语言开发，建议在linux上运行，本教程使用Centos6.4作为安装环境。安装redis需要先将官网下载的源码进行编译，编译依赖gcc环境。

```properties
yum install gcc-c++
```

#### 1.2.2 进行安装

> 01解压源码
>
> 02进入解压后的目录进行编译
>
> 03安装到指定目录
>
> 04拷贝配置文件到安装目录下

```properties
tar -zxvf redis-5.0.3.tar.gz
cd /usr/local/redis-5.0.3.tar.gz
make PREFIX=/usr/local/redis install
cd /usr/local/redis
mkdir conf
cp /usr/local/redis-5.0.3/redis.conf  /usr/local/redis/bin
```

#### 1.2.3 redis.conf

> 01修改daemonize yes，使用yes启用守护进程
>
> #### 02修改bind 192.168.101.3，绑定的主机地址，提倡修改成：0.0.0.0
>
> 03修改port 6379，监听端口



### 1.3 启动使用

#### 1.3.1 前端模式

> 直接运行bin/redis-server将以前端模式启动，前端模式启动的缺点是ssh命令窗口关闭则redis-server程序结束，不推荐使用此方法。

```properties
bin/redis-server
```



#### 1.3.2 后端模式

> 前提修改redis.conf配置文件，daemonize yes 以后端模式启动。

执行如下命令启动redis：

```properties
cd /usr/local/redis
./bin/redis-server ./redis.conf
ps -ef|grep redis
```

#### 1.3.3 脚本模式

1. vim /etc/init.d/redis

```properties
#!/bin/sh
#
# Redis       Redis
#
# chkconfig: 2345 20 10
# description: Redis is a persistent key-value database \
#              start at boot time.
#

# 端口号
REDISPORT=6379

# 执行脚本的地址
EXEC=/usr/local/redis/bin/redis-server

# 客户端执行脚本的地址
REDIS_CLI=/usr/local/redis/bin/redis-cli

# 进程id文件地址
PIDFILE=/var/run/redis_${REDISPORT}.pid

# 配置文件地址
CONF="/usr/local/redis/bin/redis.conf"

case "$1" in
    start)
        if [ -f $PIDFILE ]
        then
                echo "$PIDFILE exists, process is already running or crashed"
        else
                echo "Starting Redis server..."
                $EXEC $CONF
        fi
        if [ "$?"="0" ]
        then
              echo "Redis is running..."
        fi
        ;;
    stop)
        if [ ! -f $PIDFILE ]
        then
                echo "$PIDFILE does not exist, process is not running"
        else
                PID=$(cat $PIDFILE)
                echo "Stopping ..."
                $REDIS_CLI -p $REDISPORT SHUTDOWN
                while [ -x ${PIDFILE} ]
               do
                    echo "Waiting for Redis to shutdown ..."
                    sleep 1
                done
                echo "Redis stopped"
        fi
        ;;
   restart|force-reload)
        ${0} stop
        ${0} start
        ;;
  *)
    echo "Usage: /etc/init.d/service-redis {start|stop|restart|force-reload}" >&2
        exit 1
esac
```

2. 设置权限

```properties
chmod 755 /etc/init.d/redis
```

3. 启动测试

```properties
/etc/init.d/redis start
```

4. 设置开机自启动

```properties
chkconfig --add /etc/init.d/redis
chkconfig redis on
```

5. 创建Redis服务

创建文件：vim /usr/lib/systemd/system/redis.service

```properties
[Unit]
Description=Redis Server
After=network.target

[Service]
ExecStart=/usr/local/redis/bin/redis-server  /usr/local/redis/bin/redis.conf  --daemonize no
ExecStop=/usr/local/redis/bin/redis-cli -p 6379 shutdown
Restart=always

[Install]
WantedBy=multi-user.target
```

开机启动命令

```properties
systemctl enable redis   #设置Redis服务开机启动命令
systemctl start redis    #立即启动Redis服务命令
pstree
```



## 2. Redis高可用

### 2.1  Redis集群原理

#### 2.1.1 redis-cluster架构图



#### 2.1.2 redis-cluster投票:容错





### 2.2 Ruby环境

> redis集群管理工具redis-trib.rb依赖ruby环境，首先需要安装ruby环境。

```properties
安装ruby
yum -y install ruby
yum -y install rubygems

gem install redis  # 安装ruby和redis的接口程序
```

> CentOS7 yum库中ruby的版本支持到 2.0.0，可gem 安装redis需要最低是2.2.2，采用rvm来更新ruby

[参考资料](https://www.cnblogs.com/PatrickLiu/p/8454579.html)

```properties
gem install redis
    ERROR:  Error installing redis:
            redis requires Ruby version >= 2.2.2.
```



### 2.3 创建集群

#### 2.3.1 基础配置

> 将redis安装目录bin下的文件拷贝到每个redis700X目录内，同时将redis源码目录bin（src）下的./redis-cli（redis-trib.rb）拷贝到redis-cluster目录下。
>
> 特别注意：redis5.0使用redis-cli作为创建集群的命令，使用c语言实现，不再使用ruby语言。



修改每个700X目录下的redis.conf配置文件：

```properties
protected-mode no
port 8000
cluster-enabled yes                     # 开启集群
cluster-config-file nodes-8000.conf     # 集群的配置文件 nodes_{port}.conf的形式
cluster-node-timeout 5000               # 超时时间 5s够了
daemonize yes                           # redis后台运行
pidfile /var/run/redis_8000.pid         # 需要修改为 reids_{port}.pid 的形式
logfile "8000.log"
dir /redis/data
bind 127.0.0.1
appendonly  yes                         #开启AOF日志
```



#### 2.3.2 脚本启动并创建集群

```properties
#!/bin/bash

# 集群文件位置
REDIS_PATH="/usr/local/redis-cluster/"

# 集群信息文件
REDIS_DATA="/redis/data/"

# 集群各种操作
case "$1" in
    start)
    cd ${REDIS_PATH}
    mkdir ${REDIS_DATA}
    # 启动Redis服务
    redis_arr=(8001 8002 8003 8004 8005 8006)
    for num in ${redis_arr[@]}
    do
        echo "start ${num}nth redis server..."
        cd redis${num}/
        ./redis-server ./redis.conf
        cd ..
    done
    # 创建Redis服务集群
    ./redis-cli --cluster create   127.0.0.1:8001 127.0.0.1:8002 127.0.0.1:8003 127.0.0.1:8004 127.0.0.1:8005 127.0.0.1:8006  --cluster-replicas 1
    ;;
    stop)
    echo 'stop redis cluster option.....'
    # 清除数据（细节）
    rm -rf ${REDIS_DATA}appendonly.aof dump.rdb nodes-800*
    rm -rf ${REDIS_PATH}redis*/dump.rdb
    DATA_NUM=`find ${REDIS_PATH} -type f | grep -E "dump.rdb|nodes*" | wc -l`
    if [[ "${DATA_NUM}" -le 0 ]]
        then
            echo -e "=====> Success: Has remove all dump.rdb and nodes configure file."
        else
            echo -e "<===== Fail: There still are files is exist,Use command: \n\tfind ${redis_path} -type f | grep -E \"dump.rbd|nodes*\" \nto search, and then remove them manually.\n"
        exit 1
    fi
    # 关闭进程
    ps aux | grep redis | grep cluster | grep -v grep | awk '{print $2}' | xargs kill -9
    echo '进程杀死'
    # 进程数量
    CLUSTER_NUM=`ps aux | grep redis | grep cluster | wc -l`
    if [[ "${CLUSTER_NUM}" -le 0 ]]
        then
            echo -e "=====> Success: Has killed all cluster progress."
        else
            echo -e "<===== Fail: There still are ${cluster_num} is alive.\n"
        exit 1
    fi
        ;;
   restart|force-reload)
        ;;
  *)
    echo "Usage: /usr/local/redis-cluster/start.sh {start|stop|restart|reload}" >&2
        exit 1
esac
```

#### 2.3.3 集群常用命令

```properties
flushdb
cluster reset
./redis-cli -c -h 127.0.0.1 -p 8001  # 客户端以集群方式登陆
./redis-cli --cluster check 127.0.0.1:8001   # 查看节点8001端口号的集群信息
```

### 2.4 查询集群信息



### 2.5 添加主节点



### 2.6 添加从节点



### 2.7 删除节点





## 参考资料

[CentOS7下Redis的安装和设置开机启动](https://blog.csdn.net/Jeffid/article/details/84642388)
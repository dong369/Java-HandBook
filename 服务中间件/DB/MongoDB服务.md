## 1. MongoDB介绍

### 1.1 官网下载

[MongoDB下载地址](https://redis.io/download)

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
> 02修改bind 192.168.101.3，绑定的主机地址，提倡修改成：0.0.0.0
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

1. 设置权限

```properties
chmod 755 /etc/init.d/redis
```

1. 启动测试

```properties
/etc/init.d/redis start
```

1. 设置开机自启动

```properties
chkconfig --add /etc/init.d/redis
chkconfig redis on
```

1. 创建Redis服务

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





## 参考资料

[CentOS7下Redis的安装和设置开机启动](https://blog.csdn.net/Jeffid/article/details/84642388)
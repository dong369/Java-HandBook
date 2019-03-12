[TOC]

## 1. JDK配置

### 1.1 官网下载

[163镜像中心下载地址](http://mirrors.163.com/)

演示使用版本：CentOS-7-x86_64-Minimal-1810.iso

### 1.2 官方JDK安装

第一步：打开vi /etc/profile

```properties
JAVA_HOME=/usr/local/soft/java/jdk1.8
JRE_HOME=/usr/local/soft/java/jdk1.8/jre
CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export PATH JAVA_HOME CLASSPATH
```

第二步：使环境变量生效

```properties
source /etc/profile
```

第三步：看看自己的配置是否都正确

```properties
echo $JAVA_HOME
```

### 1.3 OpenJDK 安装

1. 安装

```properties
yum search java|grep jdk
yum install java-1.8.0-openjdk.x86_64
ls /usr/lib/jvm
vi /etc/profile
	#set java environment
	JAVA_HOME=/usr/lib/jvm/jdk1.8.0_202
	JRE_HOME=$JAVA_HOME/jre
	CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
	PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
	export JAVA_HOME JRE_HOME CLASS_PATH PATH
source /etc/profile
```

2. 卸载

```properties
yum list installed | grep java
yum -y remove java-1.8.0-openjdk*
yum -y remove tzdata-java.noarch
java -version
```

## 2. 防火墙配置

#### 2.1 V-7之前

> + 重启后生效
>   开启： chkconfig iptables on
>   关闭： chkconfig iptables off
> + 即时生效，重启后失效
>   开启： service iptables start
>   关闭： service iptables stop
> + 推荐使用
>   在/etc/sysconfig/iptables文件中加入如下端口访问规则

注意更改后重启防火墙服务：service iptables restart

#### 2.2 V-7之后

1. firewalld的基本使用

```properties
启动： systemctl start firewalld
关闭： systemctl stop firewalld
查看状态： systemctl status firewalld
开机禁用  ： systemctl disable firewalld
开机启用  ： systemctl enable firewalld
```

2. systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

```properties
启动一个服务：systemctl start firewalld.service
关闭一个服务：systemctl stop firewalld.service
重启一个服务：systemctl restart firewalld.service
显示一个服务的状态：systemctl status firewalld.service
在开机时启用一个服务：systemctl enable firewalld.service
在开机时禁用一个服务：systemctl disable firewalld.service
查看服务是否开机启动：systemctl is-enabled firewalld.service
查看已启动的服务列表：systemctl list-unit-files|grep enabled
查看启动失败的服务列表：systemctl --failed
```

3. 配置firewalld-cmd

```properties
查看版本： firewall-cmd --version
查看帮助： firewall-cmd --help
显示状态： firewall-cmd --state
查看所有打开的端口： firewall-cmd --zone=public --list-ports
更新防火墙规则： firewall-cmd --reload
查看区域信息:  firewall-cmd --get-active-zones
查看指定接口所属区域： firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态： firewall-cmd --panic-off
查看是否拒绝： firewall-cmd --query-panic
```

4. CentOS 7怎么开启一个端口

> 22、80、8080、3306、6379、

```properties
添加
firewall-cmd --zone=public --add-port=80/tcp --permanent    （--permanent永久生效，没有此参数重启后失效）
查看
firewall-cmd --zone=public --query-port=80/tcp
删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent
重新载入
firewall-cmd --reload
```

## 3. 文件传输

```properties
scp root@10.248.2.10:/usr/local/service/jdk/jdk-8u201-linux-x64.tar.gz /usr/local/soft/
```
# 1 JDK配置

## 1.1 官网下载

[163镜像中心下载地址](http://mirrors.163.com/)

演示使用版本：CentOS-7-x86_64-Minimal-1810.iso

## 1.2 官方JDK安装

第一步：打开	vi /etc/profile

```properties
JAVA_HOME=/opt/soft/java/jdk1.8

PATH=$PATH:$JAVA_HOME/bin

CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export JAVA_HOME PATH CLASSPATH
```

第二步：使环境变量生效

```properties
source /etc/profile
```

第三步：看看自己的配置是否都正确

```properties
echo $JAVA_HOME
```

## 1.3 OpenJDK 安装

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

## 1.4 环境变量

```properties
1、/etc/profile:在登录时,操作系统定制用户环境时使用的第一个文件,此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。
2、/etc/environment:在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,设置环境文件的环境变量。
3、~/.bash_profile:在登录时用到的第三个文件是.profile文 件,每个用户都可使用该文件输入专用于自己使用的shell信息,当用 户登录时,该 文件仅仅执行一次!默认情况下,他设置一些环境变游戏量,执 行用户的.bashrc文件。/etc/bashrc:为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该 文件被读取.
4、~/.bashrc:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。
```

优先级：1>2>3>4

# 2 防火墙配置

## 2.1 V-7之前

> + 重启后生效
>   开启： chkconfig iptables on
>   关闭： chkconfig iptables off
> + 即时生效，重启后失效
>   开启： service iptables start
>   关闭： service iptables stop
> + 推荐使用
>   在/etc/sysconfig/iptables文件中加入如下端口访问规则

注意更改后重启防火墙服务：service iptables restart

## 2.2 V-7之后

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

> 22、80、8080、3306、6379

```properties
添加（--permanent永久生效，没有此参数重启后失效）
firewall-cmd --zone=public --add-port=80/tcp --permanent
查看
firewall-cmd --zone=public --query-port=80/tcp
删除
firewall-cmd --zone=public --remove-port=80/tcp --permanent
重新载入
firewall-cmd --reload
```

# 3 文件传输

## 3.1 SCP

```properties
scp root@10.248.2.10:/usr/local/service/jdk/jdk-8u201-linux-x64.tar.gz /usr/local/soft/
scp /usr/local/soft/ root@10.248.2.10:/usr/local/service/jdk/jdk-8u201-linux-x64.tar.gz
```

## 3.2 Rsync

# 4 文件共享

## 4.1 安装samba

```properties
yum -y install samba
```

## 4.2 配置共享文件夹

> samba配置文件在`/etc/samba/smb.conf`，修改这个配置文件在最后加入下面的配置

```properties
[linux_share]
path=/home/share
writable = yes
public=yes
```

## 4.3 配置免密码模式

> samba配置文件在`/etc/samba/smb.conf`，修改这个配置文件在最后加入下面的配置

```properties
[global]
	workgroup = SAMBA
	security = user
	map to guest = Bad User
```

## 4.4 启动samba

```properties
systemctl start smb nmb
systemctl restart smb nmb
ps -ef | grep -E 'smb|nmb'
netstat -tunlp | grep -E 'smbd|nmbd'
```

## 4.5 关闭SElinux

```properties
vi /etc/selinux/config（vi /etc/sysconfig/selinux）
找到#SELINUX=enforcing，改成SELINUX=disabled，如上图，保存。这个操作需要重启
才能永久生效，所以可以临时关闭一下。
setenforce 0
查看生效的命令是
sestatus
reboot # 进行重启操作
getenforce  # 检查是否生效
```

## 4.6 访问

```properties
\\192.168.100.12
file://192.168.100.12/share/
file:///192.168.100.12
```

## 4.7 开机启动

```properties

```



# 5 图形界面

## 5.1 yum方式

```properties
yum -y groupinstall "GNOME Desktop"

startx
```

# 6 磁盘操作

## 6.1 分区

```properties
# 磁盘分区
fdisk -l
fdisk  /dev/vdb（输入 n， p， 1， 回车，回车， wq）
mkfs.ext3 /dev/vdb1（格式化磁盘）
mkdir /data0
mount /dev/vdb1 /data0（umount /dev/vdb1）
echo '/dev/vdb1 /data0 ext3 defaults 0 0' >> /etc/fstab
```

## 6.2 转换

```properties
# ext3转ext4
umount /dev/vdb1
lsblk -f
mkfs.ext4 /dev/vdb1
mount -t ext4 /dev/vdb1 /data/
```

# 7 第三方库

## 7.1 Git

```properties
yum install -y git
```


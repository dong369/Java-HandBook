# 1 Java环境

## 1 JDK配置

### 1.1 官网下载

[163镜像中心下载地址](http://mirrors.163.com/)

演示使用版本：CentOS-7-x86_64-Minimal-1810.iso

### 1.2 官方JDK安装

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

### 1.3 OpenJDK安装

1、安装

```properties
yum search java|grep jdk
yum install java-1.8.0-openjdk.x86_64
ls /usr/lib/jvm

vi /etc/profile
# set java environment
JAVA_HOME=/usr/lib/jvm/jdk1.8.0_202
JRE_HOME=$JAVA_HOME/jre
CLASS_PATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar:$JRE_HOME/lib
PATH=$PATH:$JAVA_HOME/bin:$JRE_HOME/bin
export JAVA_HOME JRE_HOME CLASS_PATH PATH

source /etc/profile
```

2、卸载

```properties
yum list installed | grep java
yum -y remove java-1.8.0-openjdk*
yum -y remove tzdata-java.noarch
java -version
```

### 1.4 环境变量

```properties
1、/etc/profile:在登录时,操作系统定制用户环境时使用的第一个文件,此文件为系统的每个用户设置环境信息,当用户第一次登录时,该文件被执行。
2、/etc/environment:在登录时操作系统使用的第二个文件,系统在读取你自己的profile前,设置环境文件的环境变量。
3、~/.bash_profile:在登录时用到的第三个文件是.profile文件,每个用户都可使用该文件输入专用于自己使用的shell信息,当用户登录时,该文件仅仅执行一次!默认情况下,他设置一些环境变游戏量,执行用户的.bashrc文件。
4、/etc/bashrc:为每一个运行bash shell的用户执行此文件.当bash shell被打开时,该文件被读取。
5、~/.bashrc:该文件包含专用于你的bash shell的bash信息,当登录时以及每次打开新的shell时,该该文件被读取。
```

优先级：1>2>3>4>5

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

1、firewalld的基本使用

```properties
启动：systemctl start firewalld
关闭：systemctl stop firewalld
查看状态：systemctl status firewalld
开机禁用：systemctl disable firewalld
开机启用：systemctl enable firewalld
```

2、systemctl是CentOS7的服务管理工具中主要的工具，它融合之前service和chkconfig的功能于一体。

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

3、配置firewalld-cmd

```properties
查看版本：firewall-cmd --version
查看帮助：firewall-cmd --help
显示状态：firewall-cmd --state
查看所有打开的端口：firewall-cmd --zone=public --list-ports
更新防火墙规则：firewall-cmd --reload
查看区域信息：firewall-cmd --get-active-zones
查看指定接口所属区域：firewall-cmd --get-zone-of-interface=eth0
拒绝所有包：firewall-cmd --panic-on
取消拒绝状态：firewall-cmd --panic-off
查看是否拒绝：firewall-cmd --query-panic
```

4、CentOS 7怎么开启一个端口

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

# 3 传输服务

## 3.1 Scp

安全远程文件复制程序,基于ssh；不支持符号连接；支持两个远程主机之间的复制。

```shell
$> scp -r ~/xxx.txt ubuntu@s101:/home/ubuntu/		//递归复制
$> scp root@10.248.2.10:/usr/local/service/jdk/jdk-8u201-linux-x64.tar.gz /usr/local/soft/
$> scp /usr/local/soft/ root@10.248.2.10:/usr/local/service/jdk/jdk-8u201-linux-x64.tar.gz
```

## 3.2 Rsync

远程同步工具，主要用于备份和镜像；支持符号链接、设备等等；速度快，避免复制相同内容的文件数据；不支持两个远程主机之间的复制。

```shell
$> rsync -rvl ~/hello.txt root@s101:/home/ubuntu/
```

# 4 共享服务

## 4.1 Samba服务

### 4.1.1 服务端

4.1 安装Samba

```properties
yum -y install samba
```

4.2 配置共享文件夹

> samba配置文件在`/etc/samba/smb.conf`，修改这个配置文件在最后加入下面的配置

```properties
[linux_share]
path=/home/share
writable = yes
public=yes
```

4.3 配置免密码模式

> samba配置文件在`/etc/samba/smb.conf`，修改这个配置文件在最后加入下面的配置

```properties
[global]
	workgroup = SAMBA
	security = user
	map to guest = Bad User
```

4.4 启动samba

```properties
systemctl start smb nmb
systemctl restart smb nmb
ps -ef | grep -E 'smb|nmb'
netstat -tunlp | grep -E 'smbd|nmbd'
```

4.5 关闭SElinux

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

4.6 访问

```properties
\\192.168.100.12
file://192.168.100.12/share/
file:///192.168.100.12
```

4.7 开机启动

```properties

```

## 4.2 NFS服务

### 4.2.1 服务端

1、安装服务
yum install -y nfs-utils

2、vi /etc/exports //NFS配置文件

/home/nfs *(rw,sync,no_root_squash)

3、启动服务

systemctl start rpcbind

systemctl start nfs

netstat -anptu | grep rpcbind

rpm -ql nfs-utils | grep show

4、查看发布的NFS共享目录

showmount -e 172.16.7.121

### 4.2.2 客户端

1、安装

yum install nfs-utils

2、挂载

mount -t nfs 172.16.7.121:/home/nfs /data/soft/br_data_bak

3、设置自动挂载

## 4.3 挂载Win10共享

### 4.3.1 服务

文件共享部分：
1、选好需要共享的文件夹D:\share

2、对share文件夹右击——>共享——>特定用户

3、下拉菜单中——>选择everyone ——>点击添加

4、确定共享
网络部分：
1、右击网络选属性——>更改高级共享设置

2、在所有网络中拉到最下面——>密码保护的共享

3、选择关闭密码保护共享，WIN+R输入\192.168.80.10 (此处为vnet1虚拟网卡的地址）

1、安装共享必备工具

```
yum install samba-client cifs-utils -y //安装samba客户软件和文件系统管理工具
```

2、查询共享文件夹

`smbclient -L //192.168.80.10/`

诉查询命令对于查询win7共享目录是可以实现的，这是因为win7和samba客户端CentOS7都是使用的SMB1的协议，而win10已经使用SMB2协议了，因此不能正常访问。
**解决办法如下：**
1）smbclient -L //192.168.80.10/ -m SMB2 //加入-m参数指定协议类型

2）-m参数只是临时的，因此可以通过在主配置文件/etc/samba/smb.conf中加入client max protocol = SMB2

3、samba客户端挂载

```
mkdir /abc   //新建挂载目录
mount.cifs //192.168.80.10/share /abc    // 挂载
```

同样该命令对于win7可以使用，但是win8和win10，对于挂载共享目录来说需要用以下标准语法：mount -t cifs //IP地址/共享名称 挂载点 -o username=用户名,password=密码,其他选项
**解决办法如下：**

mount -t cifs //192.168.80.10/share /abc -o username=root,password=root,vers=2.0
**验证挂载成功**

`df -hT`

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
mkfs.ext4 /dev/vdb1
mount -t ext4 /dev/vdb1 /data/
# 查看格式
lsblk -f
df -hT（df -hl）
```

# 7 第三方库

## 7.1 Git

```properties
yum -y install git
```


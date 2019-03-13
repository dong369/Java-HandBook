[TOC]

## 1. Linux的版本

### 1.1 版本

> 1.	[CentOS](https://www.centos.org/)
> 2.	[Redhat](https://www.ubuntu.com/index_kylin)
> 3.	Ubuntu 12.04 64位
> 4.	Suse
> 5.	Debian
> 6.	Mandriva
> 7.	红旗

### 1.2 查看帮助

```properties
man grep
info grep
ls --help
```

## 2. 版本下载

[阿里巴巴开源镜像站](https://opsx.alibaba.com/)

[网易开源镜像站](https://link.jianshu.com/?t=http://mirrors.163.com/)

[清华大学 TUNA 协会](https://tuna.moe/)

### 2.1 CentOS系统
```properties
CentOS-6.4-x86_64-netinstall.iso    网络安装镜像
CentOS-6.4-x86_64-minimal.iso       最小化安装，只安装必须的软件，类似于精简版，无桌面界面
CentOS-6.4-x86_64-bin-DVD1.iso      完整版的安装盘 
CentOS-6.4-x86_64-bin-DVD2.iso      对完整版安装盘的软件进行补充和升级。
```
### 2.2 Ubuntu系统
```properties
Ubuntu-17.04-desktop-amd64.iso  桌面版系统
Ubuntu-17.04-server-amd64.iso   服务版系统
```

## 3. Linux配置信息

### 3.1 CentOS系统信息



> 输入uname -a ，可显示电脑以及操作系统的相关信息。 
> 输入cat /proc/version，说明正在运行的内核版本。
> 输入cat /etc/redhat-release，Linux查看版本当前操作系统发行版信息
> 输入cat /etc/issue，显示的是发行版本信息
> lsb_release -a，适用于所有的linux，包括Redhat、SuSE、Debian等发行版，但是在debian下要安装lsb



> cat /proc/cpuinfo，显示系统配置信息

### 3.1 Ubunto系统版本



## 4. Linux基础配置

### 4.1 CentOS基础软件

CentOS的软件安装工具不是apt-get 是yum，安装基础环境和rz上传。

```properties
yum install -y gcc gcc-c++ pcre pcre-devel zlib zlib-devel openssl openssl-devel
yum -y install libstdc++-devel
yum –y install lrzsz
yum -y install wget
yum -y install psmisc  #pstree以树结构显示进程
yum install curl
yum -y install net-tools
```

### 4.2 Ubunto基础软件

Ubunto的软件安装工具apt-get ，安装基础环境和rz上传。

```properties
sudo apt-get install lrzsz
sudo apt-get install openssh-server openssh-client
sudo /etc/init.d/ssh restart
```

### 4.2 重启命令

> 1、reboot
> 2、shutdown -r now 立刻重启(root用户使用)
> 3、shutdown -r 10 过10分钟自动重启(root用户使用)
> 4、shutdown -r 20:35 在时间为20:35时候重启(root用户使用)

### 4.3 关机命令

> 1、halt   立刻关机
> 2、poweroff  立刻关机
> 3、shutdown -h now 立刻关机(root用户使用)
> 4、shutdown -h 10 10分钟后自动关机

### 4.4 用户切换命令：su

> 1、使用su命令：只是切换了root身份，但Shell环境仍然是普通用户的Shell
> 2、使用su -命令：连用户和Shell环境一起切换成root身份

```properties
sudo：表示获取临时的root权限执行命令。
su - root：以root身份登录。
su root/其他命令：与root建立一个连接，通过root执行命令。
```

## 5. 配置阿里YUM源

### 5.1 备份

备份原始YUM文件
`mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.backup`

### 5.2 下载新的YUM文件

`curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo`

### 5.3 更新缓存

`yum makecache`

## 6. Linux其它配置

### 6.1 CentOS其它配置

#### 6.1.1 服务名称管理

更改hostname

```properties
vi /etc/hostname
```

配置hosts

```properties
vi /etc/hosts
```

#### 6.1.2 计划任务

1. 服务安装

```properties
systemctl status crond.service #检查服务是否在运行

yum install vixie-cron
yum install crontabs

systemctl restart crond.service #启动服务
systemctl reload crond.service  #重新载入配置
systemctl status crond.service  #查看crontab服务状态
systemctl enable crond.service  #开机自启动

crontab -e #进入定时任务编辑
```

1. 相关参数

```properties
基本格式 :　　
* * 　 *　 *　 *　　command　　
分　 时　 日　 月　 周　 命令　　
第1列表示分钟1～59 每分钟用*或者 */1表示　　
第2列表示小时1～23（0表示0点）　　
第3列表示日期1～31　　
第4列表示月份1～12　　
第5列标识号星期0～6（0表示星期天）　　
第6列要运行的命令
```

1. 例子

```properties
*/2 * * * * curl -o /home/index.html www.baidu.com
每隔两分钟使用curl 访问www.baidu.com 并将结果写入/home/index.html 文件
```

#### 6.1.3 IP配置

1. 动态IP

```properties
DEVICE=eth0
HWADDR=00:0C:29:02:BD:F3
TYPE=Ethernet
UUID=d65d5473-4b67-4eab-817c-7dfba15d9203
ONBOOT=yes  #默认是no，需要改成yes。指明在系统启动时是否激活网卡
NM_CONTROLLED=yes
BOOTPROTO=dhcp
```

2. 静态IP

```properties
DEVICE=eth0 #描述网卡对应的设备别名
HWADDR=00:0C:29:02:BD:F3    
TYPE=Ethernet   #设置网卡类型
UUID=d65d5473-4b67-4eab-817c-7dfba15d9203
ONBOOT=yes  #默认是no，需要改成yes。指明在系统启动时是否激活网卡
NM_CONTROLLED=yes   #network manger的参数，实时生效，修改后无需要重启网卡立即生效,建议网卡配置中设置为no
BOOTPROTO=static    #设置网卡获得ip地址的方式，可能的选项为static，dhcp或bootp
IPADDR=192.168.100.100  #此字段就指定了网卡对应的ip地址
GATEWAY=192.168.100.2   #默认网关的IP地址
systemctl restart network（CentOS7以前使用命令：service restart network）
```

#### 6.1.4 更新系统和补丁

> 虚拟机-管理-克隆-创建完整克隆

1. 网络设置

```properties
yum -y update
```

#### 6.1.5 域名解析DNS

> DNS（Domain Name System，[域名系统](https://baike.baidu.com/item/%E5%9F%9F%E5%90%8D%E7%B3%BB%E7%BB%9F/2251573)），万维网上作为域名和[IP地址](https://baike.baidu.com/item/IP%E5%9C%B0%E5%9D%80)相互映射的一个[分布式数据库](https://baike.baidu.com/item/%E5%88%86%E5%B8%83%E5%BC%8F%E6%95%B0%E6%8D%AE%E5%BA%93/1238109)，能够使用户更方便的访问[互联网](https://baike.baidu.com/item/%E4%BA%92%E8%81%94%E7%BD%91)，而不用去记住能够被机器直接读取的IP数串。

互联网上的服务器，编辑文件 "/etc/resolv.conf"，根据情况修改文件内容。

```properties
nameserver 223.5.5.5
nameserver 223.6.6.6
```

#### 6.1.6 网络授时NTF

> NTP是网络时间协议(Network Time Protocol)，它是用来同步网络中各个计算机的时间的协议。

互联网上的服务器，编辑文件 "/etc/ntp.conf"，根据情况修改文件内容为：

```properties
driftfile  /var/lib/ntp/drift
pidfile   /var/run/ntpd.pid
logfile /var/log/ntp.log
restrict    default kod nomodify notrap nopeer noquery
restrict -6 default kod nomodify notrap nopeer noquery
restrict 127.0.0.1
server 127.127.1.0
fudge  127.127.1.0 stratum 10
server ntp.aliyun.com iburst minpoll 4 maxpoll 10
restrict ntp.aliyun.com nomodify notrap nopeer noquery
```

#### 6.1.7 修改ssh远程登陆的默认端口

```properties
vi /etc/ssh/sshd_config
找到#Port 23，Port前面默认是有#的，去掉之后把23改成非常规端口，然后保存。
systemctl restart sshd
```

#### 6.1.8 禁止默认root账号登陆

```properties
vi /etc/ssh/sshd_config
找到＃PermitRootLogin no 这行,把前面的#去掉，修改no为yes，保存即可。
然后不要着急重启服务，先要创建账号。
useradd admin
useradd是创建命令，admin是你要创建账号的名称，然后要为新账号设置密码。
passwd admin
systemctl restart sshd
```

#### 6.1.9 关闭SElinux

```properties
vi /etc/selinux/config
找到#SELINUX=enforcing，改成SELINUX=disabled，如上图，保存。这个操作需要重启
才能永久生效，所以可以临时关闭一下。
setenforce 0
查看生效的命令是
sestatus
```

### 6.2 Ubunto其它配置

#### 6.2.1 服务名称管理

更改hostname

```properties
vi /etc/hostname
```

配置hosts

```properties
vi /etc/hosts
```

#### 6.2.2 计划任务



#### 6.2.3 IP配置

1. 动态IP

```properties
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
source /etc/network/interfaces.d/*
# The loopback network interface
auto lo
iface lo inet loopback
# The primary network interface
auto ens33
iface ens33 inet dhcp
```

2. 静态IP

```properties
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
source /etc/network/interfaces.d/*
# The loopback network interface
auto lo
iface lo inet loopback
# The primary network interface
iface ens33 inet static
address 192.168.100.200
gateway 192.168.100.2
netmask 255.255.255.0
dns-nameservers 192.168.100.2
auto ens33
```



#### 6.2.4 更新系统和补丁

```properties
sudo apt-get update
```

## 7. 软件安装

### 7.1 yum方式

#### 7.1.1 基础命令

```properties
yum list | grep nginx  # 查询所有可用软件包列表
yum search 关键字       # 搜索服务器上所有和关键字相关的包
yum install rpmname --downloadonly --downloaddir=/rpmpath # 下载但不安装
yum -y install 包名     # 安装命令
yum -y update 包名      # 更新命令
yum -y remove 包名      # 卸载命令
```

#### 7.1.2 添加nginx源

1. vim /etc/yum.repos.d/nginx.repo

```properties
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/rhel/7/$basearch/
gpgcheck=0
enabled=1
```

2. 生成缓存

```properties
yum makecache
yum search nginx
yum -y install nginx.x86_64
```

### 7.2 Rpm包方式

#### 7.2.1 基础命令

```properties
rpm -ivh 包全名		# rpm手工安装
rpm -Uvh 包全名		# rpm包升级
rpm -e 包名            # rpm包卸载
rpm -q 包名            # 查询包是否安装
rpm -qa               # 查询所有已安装的rpm包
rpm -qa | grep 包名    # 可联合grep命令查找
```

## 8. 解压/压缩

### 8.1 tar

```properties
tar -cvf jpg.tar file1 file2 ....fileN
tar -xvf my.tar.gz
tar -xvf my.tar.gz -C /usr/local/soft
// 大文件后台压缩
nohup tar -cvf jpg.tar file1 file2 ....fileN &
```

### 8.2 tar.gz

```properties
tar -zcvf my.tar.gz file1 file2 ....fileN
tar -zxvf my.tar.gz
tar -zxvf my.tar.gz -C /usr/local/soft
// 大文件后台解压
nohup tar -zcvf my.tar.gz file1 file2 ....fileN &
```

## 9. 开机启动

```properties
chkconfig --list
chkconfig --add service
chkconfig --del service
systemctl enable docker.service
/usr/lib/systemd/system/docker.service
```


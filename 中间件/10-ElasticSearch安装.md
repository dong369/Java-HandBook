# ES的概述，下载，安装

## ES概述

## ES下载
下载地址：`https://www.elastic.co/cn/downloads/elasticsearch`
找到`MACOS/LINUX`，点击下载

## ES安装

### 0.安装JDK
具体参见以前写的安装JDK文件

### 1.建立目录
`mkdir -p /opt/soft`

### 2.上传文件
把刚刚下载的`elasticsearch-6.5.1.tar.gz`上传至到`/opt/soft`目录下

### 3.解压
`tar -zxvf elasticsearch-6.5.1.tar.gz`

### 4.进入目录 
`cd elasticsearch-6.5.1`

### 5.建立es用户和用户组

```shell
groupadd esgroup
useradd esuser -g esgroup -p 123456
```
### 6.更改es文件夹及内部文件的所属用户组

更改elasticsearch文件夹及内部文件的所属用户及组：
```shell
cd /opt

chown -R esuser:esgroup elasticsearch-6.2.4
```

### 7.切换用户并运行

```shell
su esuser

./bin/elasticsearch
```

### 8.后台运行
刚刚启动后，你会发现一按`ctrl + c` 就关闭服务了，这时候需要加参数进行后台启动

```shell
su esuser
./bin/elasticsearch -d
ps -ef | grep elastic
```

### 9.测试连接
再复制一个ssh端口，之后走`curl 127.0.0.1:9200`

### 10.打开远程连接
这个时候想从浏览器打开，发现打不开，此时要修改配置文件`config/elasticsearch.yml`
```shell
network.host: 10.237.16.13
```
### 11.修改配置文件后报错处理
再次启动，会有一个error
``` shell
[2018-11-30T15:10:04,815][DEBUG][o.e.a.ActionModule       ] [4NdIzs7] Using REST wrapper from plugin org.elasticsearch.xpack.security.Security
[2018-11-30T15:10:05,016][INFO ][o.e.d.DiscoveryModule    ] [4NdIzs7] using discovery type [zen] and host providers [settings]
[2018-11-30T15:10:05,794][INFO ][o.e.n.Node               ] [4NdIzs7] initialized
[2018-11-30T15:10:05,794][INFO ][o.e.n.Node               ] [4NdIzs7] starting ...
[2018-11-30T15:10:05,931][INFO ][o.e.t.TransportService   ] [4NdIzs7] publish_address {10.237.16.13:9300}, bound_addresses {10.237.16.13:9300}
[2018-11-30T15:10:05,949][INFO ][o.e.b.BootstrapChecks    ] [4NdIzs7] bound or publishing to a non-loopback address, enforcing bootstrap checks
[2018-11-30T15:10:05,988][ERROR][o.e.b.Bootstrap          ] [4NdIzs7] node validation exception
[2] bootstrap checks failed
[1]: max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]
[2]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
[2018-11-30T15:10:05,989][INFO ][o.e.n.Node               ] [4NdIzs7] stopping ...
[2018-11-30T15:10:06,006][INFO ][o.e.n.Node               ] [4NdIzs7] stopped
[2018-11-30T15:10:06,006][INFO ][o.e.n.Node               ] [4NdIzs7] closing ...
[2018-11-30T15:10:06,023][INFO ][o.e.n.Node               ] [4NdIzs7] closed
[2018-11-30T15:10:06,025][INFO ][o.e.x.m.j.p.NativeController] [4NdIzs7] Native controller process has stopped - no new native processes can be started

```

修改第一个错误
```
vi /etc/security/limits.conf       //文件最后加入

esuser soft nofile 65536

esuser hard nofile 65536

esuser soft nproc 4096

esuser hard nproc 4096
```
处理第二个错误：
```

进入limits.d目录下修改配置文件。

vi /etc/security/limits.d/20-nproc.conf
```
添加 `esuser soft nproc 4096`

处理第三个错误：
​    
`vi /etc/sysctl.conf`
```
vm.max_map_count=655360
```

执行以下命令生效：
`sysctl -p`

关闭防火墙：`systemctl stop firewalld.service`


### 12.每次SSH连接都提示找不到JAVA_HOME
`vi ~/.bashrc`在末尾加上`source /etc/profile`



### 1.8安装Kibana
Kibana是一个针对Elasticsearch的开源分析及可视化平台，使用Kibana可以查询、查看并与存储在ES索引的数据进行交互操作，使用Kibana能执行高级的数据分析，并能以图表、表格和地图的形式查看数据

(1)下载Kibana
https://www.elastic.co/downloads/kibana

(2)把下载好的压缩包拷贝到/soft目录下

(3)解压缩，并把解压后的目录移动到/user/local/kibana

(4)编辑kibana配置文件

[root@localhost /]# vi /usr/local/kibana/config/kibana.yml

![image](https://images2017.cnblogs.com/blog/210978/201708/210978-20170805113725272-708617928.png)

将server.host,elasticsearch.url修改成所在服务器的ip地址

(5)开启5601端口

Kibana的默认端口是5601

开启防火墙:systemctl start firewalld.service

开启5601端口:firewall-cmd --permanent --zone=public --add-port=5601/tcp

重启防火墙：firewall-cmd –reload

(6)启动Kibana

[root@localhost /]# /usr/local/kibana/bin/kibana

浏览器访问：http://192.168.25.131:5601



vi /etc/systemd/system/elasticsearch.service

vi /usr/lib/systemd/system

[Unit]
Description=elasticsearch
[Service]
User=es
LimitNOFILE=100000
LimitNPROC=100000
ExecStart=/usr/local/server/elasticsearch/bin/elasticsearch
[Install]

WantedBy=multi-user.target
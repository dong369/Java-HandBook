## 1. Elk的下载

### 1.1 Elk下载
[下载地址](https://www.elastic.co/cn/downloads/past-releases)

本教程使用的elasticsearch版本：elasticsearch-6.8.0.tar.gz

本教程使用的kibana版本：kibana-6.8.0-linux-x86_64.tar.gz

本教程使用的logstash版本：logstash-6.8.0.tar.gz

## 2. 安装Elasticsearch

### 2.1 安装JDK
具体参见以前写的安装JDK文件

### 2.2 建立目录
```properties
mkdir -p /opt/soft/{elasticsearch,kiban}
```

### 2.3 上传文件
把刚下载的elasticsearch-7.0.0-linux-x86_64.tar.gz上传至到/usr/local/soft/es目录下

### 2.4 解压
```properties
tar -zxvf elasticsearch-6.8.0.tar.gz
```

### 2.5 进入目录 
```properties
cd /usr/local/soft/es/elasticsearch-6.8.0
```

### 2.6 建立ES用户和用户组

```properties
groupadd esgroup
useradd esuser -g esgroup -p 123456
```
### 2.7 更改ES文件的所属用户组

更改elasticsearch文件夹及内部文件的所属用户及组：
```properties
cd /usr/local/soft/es/
chown -R esuser:esgroup elasticsearch-6.8.0
```

### 2.8 切换ES用户并运行

```properties
su esuser
./bin/elasticsearch
```

### 2.9 ES后台运行
刚刚启动后，你会发现一按`ctrl + c` 就关闭服务了，这时候需要加参数进行后台启动

```properties
su esuser
./bin/elasticsearch -d
ps -ef|grep elasticsearch
```

### 2.10 ES测试连接
再复制一个ssh端口，之后走`curl 127.0.0.1:9200`

### 2.11 打开远程连接
这个时候想从浏览器打开，发现打不开，此时要修改配置文件`config/elasticsearch.yml`
```shell
network.host: 10.237.16.13
#cluster.initial_master_nodes: ["node-1", "node-2"] 修改为 cluster.initial_master_nodes: ["node-1"]，记得保存。ES7必须修改。
```
### 2.12 修改配置文件后报错处理
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
```properties
vi /etc/security/limits.conf       //文件最后加入
esuser soft nofile 65536
esuser hard nofile 65536
esuser soft nproc 4096
esuser hard nproc 4096
```
处理第二个错误：
```properties
进入limits.d目录下修改配置文件。
vi /etc/security/limits.d/20-nproc.conf
esuser soft nproc 4096
```
处理第三个错误：

```properties
vi /etc/sysctl.conf
vm.max_map_count=655360
```

执行以下命令生效：
`sysctl -p`

关闭防火墙：`systemctl stop firewalld.service`


### 2.13 SSH连接JAVA_HOME
`vi ~/.bashrc`在末尾加上`source /etc/profile`

## 3. 安装Kibana

### 3.1 Kibana关联ES

1. Kibana是一个针对Elasticsearch的开源分析及可视化平台，使用Kibana可以查询、查看并与存储在ES索引的数据进行交互操作，使用Kibana能执行高级的数据分析，并能以图表、表格和地图的形式查看数据

2. 解压缩，并把解压后的目录移动到/usr/local/soft/kibana

3. 编辑kibana配置文件

```properties
vim /usr/local/soft/kibana/kibana-7.0.0-linux-x86_64/config/kibana.yml
```

4. 将server.host，elasticsearch.url修改成所在服务器的ip地址

5. 开启5601端口

6. 开启防火墙:systemctl start firewalld.service

7. 开启5601端口:firewall-cmd --permanent --zone=public --add-port=5601/tcp

8. 重启防火墙：firewall-cmd –reload

### 3.2 Kibana汉化

```properties
vim /usr/local/soft/kibana/kibana-7.0.0-linux-x86_64/config/kibana.yml
i18n.locale: "zh_CN"
```

### 3.3 Kibana启动

1. 启动Kibana

```properties
/usr/local/soft/kibana/kibana-7.0.0-linux-x86_64/bin/kibana
nohup /usr/local/soft/kibana/kibana-7.0.0-linux-x86_64/bin/kibana &
```

2. 浏览器访问：[http://192.168.100.16:5601](http://192.168.100.16:5601/)

## 4. 安装Logstash





## 5. 项目实战


## 1. Hadoop介绍

### 1.1 官网下载

[Hadoop下载地址](http://hadoop.apache.org/)

### 1.2 版本选择

| 软件   | 版本       | 注意事项 |
| ------ | ---------- | -------- |
| JDK    | 1.8        |          |
| Hadoop | 3.2.0      |          |
| MySQL  | 8.0.15-el7 |          |
| Hive   | 3.1.1      |          |
| Hbase  |            |          |



## 2. JDK配置

参考文件！

## 3. Hadoop环境搭建

### 3.1 安装包

#### 3.1.1 上传解压

```properties
rz
tar -zxvf hadoop-3.2.0.tar.gz -C /opt/soft/hadoop/
cd /opt/soft/hadoop/hadoop-3.2.0
```

### 3.2 伪分布式

#### 3.2.1 hadoop-env.sh

vim etc/hadoop/hadoop-env.sh

```properties
export JAVA_HOME=/usr/local/soft/java/jdk1.8
```

#### 3.2.2 core-site.xml

vim etc/hadoop/core-site.xml

```properties
<configuration>
	<property>
		<name>fs.defaultFS</name>
		<value>hdfs://localhost:9000</value>
		<description>HDFS的URI，文件系统://namenode标识:端口号</description>
	</property>
	<property>
		<name>hadoop.tmp.dir</name>
		<value>/opt/soft/hadoop/hadoop-3.2.0/tmp</value>
		<description>namenode上本地的hadoop临时文件夹</description>
	</property>
	<property>
		<name>hadoop.proxyuser.root.hosts</name>
		<value>*</value>
	</property>
	<property>
		<name>hadoop.proxyuser.root.groups</name>
		<value>*</value>
	</property>
</configuration>
```

#### 3.2.3 hdfs-site.xml

vim etc/hadoop/hdfs-site.xml

```properties
<configuration>
	<property>
		<name>dfs.replication</name>
		<value>1</value>
		<description>副本个数，配置默认是3,应小于datanode机器数量</description>
	</property>
</configuration>
```

#### 3.2.4 mapred-site.xml

vim etc/hadoop/mapred-site.xml

```properties
<configuration>
	<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
	</property>
</configuration>
```

#### 3.2.5 yarn-site.xml

vim etc/hadoop/yarn-site.xml

```properties
<configuration>
	<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
	</property>
</configuration>
```

#### 3.2.6 用户配置

start-dfs.sh、stop-dfs.sh

```properties
HDFS_DATANODE_USER=root  
HDFS_DATANODE_SECURE_USER=hdfs  
HDFS_NAMENODE_USER=root  
HDFS_SECONDARYNAMENODE_USER=root
```

start-yarn.sh、stop-yarn.sh

```properties
YARN_RESOURCEMANAGER_USER=root
HADOOP_SECURE_DN_USER=yarn
YARN_NODEMANAGER_USER=root
```

#### 3.2.7 启动/访问

1. 启动服务

```properties
./sbin/start-all.sh
./sbin/stop-all.sh
jps
```

2. 服务访问

```properties
Namenode information: http://hadoop1:9870
All Applications: http://hadoop1:8088
HDFS NameNode web interface: http://hadoop1:8042
```

#### 3.2.8 报错问题





### 3.3 完全分布式



## 4. Hive

### 4.1 安装包

#### 4.1.1 上传解压

```properties
rz
tar -zxvf apache-hive-3.1.1-bin.tar.gz -C /opt/soft/hive
cd /opt/soft/hive/opt/soft/hive/apache-hive-3.1.1-bin
```

### 4.2 配置

#### 3.2.1 hive-env.sh

vim conf/hive-env.sh

```properties
# hadoop根目录
HADOOP_HOME=/opt/soft/hadoop/hadoop-3.2.0
# hive配置文件目录  
export HIVE_CONF_DIR=/opt/soft/hive/apache-hive-3.1.1-bin/conf
# hive依赖目录
export HIVE_AUX_JARS_PATH=/opt/soft/hive/apache-hive-3.1.1-bin/lib
```

#### 3.2.2 vim conf/hive-site.xml

```properties
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
	<!-- MySQL数据库基础连接配置 -->
	<property>
		<name>javax.jdo.option.ConnectionURL</name>
		<value>jdbc:mysql://211.144.5.80:30112/hive?serverTimezone=GMT</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionDriverName</name>
		<value>com.mysql.jdbc.Driver</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionUserName</name>
		<value>root</value>
	</property>
	<property>
		<name>javax.jdo.option.ConnectionPassword</name>
		<value>passw0rd</value>
	</property>
	<!-- 这是metastore -->
	<property>
		<name>hive.metastore.schema.verification</name>
		<value>false</value>
	</property>
	<property>
		<name>hive.cli.print.current.db</name>
		<value>true</value>
	</property>
	<property>
		<name>hive.cli.print.header</name>
		<value>true</value>
	</property>
	<!-- 这是hiveserver2 -->
	<property>
		<name>hive.server2.thrift.port</name>
		<value>10000</value>
	</property>
	<property>
		<name>hive.server2.thrift.bind.host</name>
		<value>211.144.5.80</value>
	</property>
</configuration>
```

#### 3.2.3 上传MySQL

```properties
cd /opt/soft/hive/apache-hive-3.1.1-bin/lib/
rz
mysql-connector-java-8.0.15.jar
```

#### 3.2.4 删除日志包

```properties
cd /opt/soft/hive/apache-hive-3.1.1-bin/lib/
rm -rf slf
```

#### 3.2.5 初始化

```properties
schematool -initSchema -dbType mysql
hive --version
hive
show databases;
```

#### 3.2.6 外部连接

```properties
nohup hive --service metastore &
nohup hive --service hiveserver2 &
netstat -nltpu |grep 10000
beeline
beeline> !connect jdbc:hive2://192.168.134.154:10000
Connecting to jdbc:hive2://192.168.134.154:10000
Enter username for jdbc:hive2://192.168.134.154:10000: root # 用户名root
Enter password for jdbc:hive2://192.168.134.154:10000: **** # 密码root
```


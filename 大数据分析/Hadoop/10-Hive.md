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


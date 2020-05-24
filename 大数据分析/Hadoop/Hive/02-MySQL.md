1 hive
==================
	数据仓库，OLAP，分析处理，存储和分析，延迟较高。
	数据库:  OLTP,在线事务处理，低延迟，事务支持。
	运行在hadoop，类SQL方法方式运行，SQL(HiveQL,HQL),MR运算。
	操纵的结构化数据。
	schema(模式，元信息存放到数据库中)，HDFS文件。derby，mysql。
	数据库和表都是路径。

hive
------------------
	类似mysql
	
	[配置hive]
		1.conf/hive-env.sh
			HADOOP_HOME=...						//不配也可以
		2.conf/hive-site.xml
			${system:java.io.tmpdir}			//配置本地临时目录
			
	$>schematool -initSchema --dbtype derby		//初始化模式
	$>hive										//初始化模式

常用命令
----------------------	
	$>!clear ;					//hive中执行shell命令
	$>!dfs -lsr / ;				//hive中执行hdfs命令
	$hive>create table hive1.t as select hive2.t2 ;	//复制表

# 2 外部MySQL

## 2.1 配置

将hive中的schema存放到外部的mysql中

	1.编写hive-site.xml，添加mysql连接信息
		[hive/conf/hive-site.xml]
		...
		<property>
			<name>javax.jdo.option.ConnectionDriverName</name>
			<value>com.mysql.jdbc.Driver</value>
		</property>
		<property>
			<name>javax.jdo.option.ConnectionURL</name>
			<value>jdbc:mysql://192.168.231.1:3306/myhive</value>
		</property>
		<property>
			<name>javax.jdo.option.ConnectionUserName</name>
			<value>root</value>
		</property>
		<property>
			<name>javax.jdo.option.ConnectionPassword</name>
			<value>root</value>
		</property>
	2.在mysql中创建myhive数据库
		$>create database myhive ;
	
	3.mysql驱动程序(jar)放置到hive classpath下。
		...
	4.重新初始化hive schema元数据库。
		$>hive/bin/shematool -initschema --dbtype mysql



启动hiveserver2服务,接收多个客户端连接请求,

使得client通过jdbc连接操纵hive数据仓库

	$>hive/bin/hive --service hiveserver2 start		//启动服务
	$>nohup hive --service hiveserver2 start &      // 后台启动
	$>netstat -ano | grep 10000						//查看端口

## 2.2  Java客户端

在eclipse中创建maven(暂时不用)项目

	1.创建java项目
	2.引入外部jar包
		181个
	3.修改hive-site.xml配置文件
		使用OS操作系统的认证方式。
		[hive-site.xml]
		hive.server2.enable.doAs=false
		hive.metastore.sasl.enabled=false
		hive.server2.authentication=NONE
	3'.重启hiveserver2服务器
		$>hive --service hiveserver2 stop
		$>hive --service hiveserver2 start &
		$>netstat -ano | grep 10000				//验证是否启动10000端口
	
	4.编写App程序
		public static void main(String[] args) throws Exception {
			Class.forName("org.apache.hive.jdbc.HiveDriver");
			Connection conn = DriverManager.getConnection("jdbc:hive2://192.168.231.100:10000/hive1","ubuntu","123456");
			PreparedStatement ppst = conn.prepareStatement("select * from t");
			ResultSet rs = ppst.executeQuery();
			while(rs.next()){
				int id = rs.getInt("id");
				String name = rs.getString("name");
				int age = rs.getInt("age");
				System.out.println(id + "," + name + "," + age);
			}
			rs.close();
			ppst.close();
			conn.close();
		}
	
	5.常用聚集函数
		count()
		sum()
		avg()
		max()
		min()


	6.解决beeline命令行终端的上下键导航历史命令的bug
		[bin/beeline]
		修改行
		if [[ ! $(ps -o stat= -p $$) =~ + ]]; then
		为
		if [[ ! $(ps -o stat= -p $$) =~ "+" ]]; then

# 3 Hive操作

3.1 基础命令
-------------

	$hive>dfs -lsr /							//执行dfs命令
	$hive>!clear ;								//执行shell脚本
	$>hive -e "select * from test"				//-e execute
	$>hive -S -e "select * from test"			//-S 静默,不输出OK,...
	$>hive -f /x/x/x/a.sql						//-f 执行一个文件,通常用于批处理
	$hive>tab tab								//显示所有命令
	$hive>-- this is a comment !				//显示所有命令
	$hive>set hive.cli.print.header=true;		//现在字段名称(头)
	$hive>create database if not exists xxx	//
	$hive>create database hive3 with dbproperties('author'='xupc','createtime'='today')
	$hive>alter database hivee3 set dbproperties('author'='you')
	
	[创建表语法]
	CREATE [TEMPORARY] [EXTERNAL] TABLE [IF NOT EXISTS] [db_name.] table_name
		[(col_name data_type [COMMENT col_comment], ...)]
		[COMMENT table_comment]
		[ROW FORMAT row_format]
		[STORED AS file_format]
	[创建表例子]
	CREATE TABLE IF NOT EXISTS employee( eid int, name String, salary String, destination String)
			COMMENT 'Employee details'						//注释
			ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'	//字段结束符
			LINES TERMINATED BY '\n'						//行结束符
			STORED AS TEXTFILE;								//存储成何种文件
	[创建表带分区]
	create table hive1.test5(id int ,name  string ,age int ) partitioned by (province string , city string) row format DELIMITED FIELDS TERMINATED BY '\t' LINES TERMINATED BY '\n' STORED AS TEXTFILE;
	
	[加载数据===insert]
	LOAD DATA [LOCAL] INPATH 'filepath' [OVERWRITE] INTO TABLE tablename [PARTITION (partcol1=val1, partcol2=val2 ...)]
	LOAD DATA LOCAL INPATH '/home/user/sample.txt' OVERWRITE INTO TABLE employee;



	[创建分区表]
	create table test5(id int,name string,age int) partitioned by (province string,city string);		//按照省份和城市分区
	[加载数据到指定分区]
	load data local inpath '/home/ubuntu/employees.txt' into table hive1.test5 partition(province='hebei',citry='baoding');
	[查看hdfs]
	/user/hive/warehouse/hive1.db/test5/province=hebei/city=baoding/employees.txt						//
	[查询分区表]
	$hive>select * from hive1.test5 where province = 'hebei' and city = 'baoding';
	[分区表的查询模式:strict / nostrict]
	$hive>set hive.mapred.mode=strict													//严格模式,默认是nostrict
	
	[查看分区表有哪些分区]
	$hive>show partitions hive1.test5 ;
	$hive>show partitions hive1.test5 partition(province='hebei') ;						//查看具体分区的细节信息
	$hive>desc extended hive1.test5 ;													//查看扩展表信息
	[手动增加分区]
	$hive>alter table hive1.test5 add partition(province='hanan',city='pingdingshan')				//
	$hive>alter table hive1.test5 add partition(area='huabei',province='hanan',city='pingdingshan')	//增加不存在的分区列，是非法的。
	
	[修改表]
	$hive>alter table hive1.test5 rename to hive1.test6 ;											//重命名
	$hive>alter table hive1.test5 add partion(province='hebei',city='zhangjiakou') location 'xxx'	//添加多个分区
									  partion(province='hebei',city='zhangjiakou')
									  partion(province='hebei',city='zhangjiakou')
									  partion(province='hebei',city='zhangjiakou')
	$hive>alter table hive1.test5 partition(province=..,city=..) set location 'xxxx'				//移动分区
	
	$hive>alter table hive1.test5 change name string												//??????
	$hive>alter table hive1.test5 add columns(birth string , fire string);							//增加列
	$hive>alter table hive1.test5 replace columns(birth string , fire string);						//增加列
	
	[修改表属性]
	$hive>alter table hive1.test5 set tblproperties('a'='x',...)									//修改表属性
	[启用归档]
	$hive>set hive.archive.enabled=true																//设置可以归档,默认false????
	
	[复制数据到分区表]
	$>insert into hive1.test5 partition(province='hebei',city='baoding') select * from hive1.test2 ;//
	insert into hive1.test6 partition(province='hebei',city='baoding') select * from hive1.test6	//字段个数相同
			where province='hebei' and city='shijiazhuang' and id > 5 ;								//查询时，分区通过where子句指定，
																									//插入时，分区用partition(..)指定
	[动态分区]
	$hive>insert overwrite table hive1.test6 partition(province,city) select id,...,province ,city	//动态分区.
																	from table2 ; 
	
	$hive>create table hive1.test1(id int) tblproperties('author'='xupc');
												//创建表，指定属性
	$hive>create table hive1.test1(id int) LOCATION '/user/ubuntu/test1'


	$hive>create external table hive1.test3 like hive1.test1 ;					//创建外部表external,只复制表结构，没有数据。
	$hive>create external table hive1.test4 as select * from hive1.test1;		//创建外部表external,只复制表结构，有数据。


	$hive>desc extended hive1.test1;					//显示扩展信息
	$hive>desc formatted hive1.test1;					//显示格式化的信息
	$hive>create table hive2.test4 like hive1.test1;	//复制表
	$hive>show tables in hive1;							//显示指定数据库的表集合,默认是当前库.


	$>hive>drop database if exists xxx			//存在即删除
	$>hive>drop database if exists xxx			//存在即删除
	$>hive>drop database if exists xxx cascde	//级联删除
	$>hive>create database hive2 location '/user/ubuntu/';
	$>hive>desc[ribe] database hive2			//显示db信息,不包含扩展信息
	$>hive>desc[ribe] database extended hive3	//显示db信息,不包含扩展信息
	$>hive>use hive3;							//使用哪个库

分区表
-----------------


托管表
----------------
	hive默认表都是托管表。hive控制其数据的生命周期。删除托管表时，元数据和数据都被删除。

外部表
---------------
	hive控制元数据。删除托管表时，数据不被删除。
	create external table hive1.test3 like hive1.test1 ;



## Beeline客户端

使用beeline客户端可以实现远程jdbc连接

	1.连接
		$>hive --service beeline -u jdbc:hive2://s100:10000/hive1
		$>beeline -u jdbc:hive2://s100:10000/hive1
		$beenline>!sh clear ;					//执行shell脚本
		$beenline>show databases;				//查看库
		$beenline>!help;						//帮助
		$beenline>!dbinfo						//帮助

配置hive的仓库位置
---------------------
	[hive-site.xml]
	hive.metastore.warehouse.dir=/user/hive/warehouse/


hive数据类型
----------------------
	类型			Size					案例
TINYINT		1 byte signed integer.				20				//byte
SMALLINT	2 byte signed integer.				20				//short
INT			4 byte signed integer.				20				//int
BIGINT		8 byte signed integer.				20				//long
BOOLEAN		Boolean true or false.				TRUE			//boolean
FLOAT		Single precision floating point.	3.14159			//float
DOUBLE		Double precision floating point.	3.14159			//double

STRING		'Now is the time', "for all	good men"				//字符串'' / ""
TIMESTAMP
BINARY		字节数组

[集合类型]
STRUCT		struct('John', 'Doe')
MAP			map('first', 'John','last', 'Doe')
ARRAY		array('John', 'Doe')

Hive所谓的读模式
-----------------------
	hive在写操作是不校验，读时校验。



```properties
show databases ;

drop database aa;
create database aa ;

use default;

show tables;

select * from mm;

drop table t;

create table t(id int ,name string,date_time timestamp);

desc t;

insert into t values (2,'java');

select * from t;

select count(*) from t;

show create table t;

alter table t set location 'hdfs://s10/user/hive/warehouse/bb.db/t';

select * from t where id = 2;

delete from t where id=2;

update t set name = 'c' where id = 1;
```


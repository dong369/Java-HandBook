flume
--------------
	agent	:
	source	:收集数据,event	
	channel	:临时存储event，buffer
	sink	:writer，destination(hdfs).

Source
-------------
	netcat
	http
	avro
	exec
	tcp
	udp
	spooldir

HDFS-Sink
------------------
	a1.sources = r1
	a1.channels = c1
	a1.sinks=k1
	
	#source
	a1.sources.r1.type = netcat
	a1.sources.r1.bind = localhost
	a1.sources.r1.port = 8888
	
	#sink
	a1.sinks = k1
	a1.sinks.k1.type = hdfs
	a1.sinks.k1.channel = c1
	a1.sinks.k1.hdfs.path = /home/ubuntu/a/%y-%m-%d/%H%M/%S
	a1.sinks.k1.hdfs.filePrefix = events-
	a1.sinks.k1.hdfs.round = true
	a1.sinks.k1.hdfs.roundValue = 10
	a1.sinks.k1.hdfs.roundUnit = minute
	a1.sinks.k1.hdfs.useLocalTimeStamp=true
	
	#channel
	a1.channels.c1.type=memory
	
	#bind
	a1.sources.r1.channels = c1
	a1.sinks.k1.channel=c1

hive sink
----------------
	1.创建表
	create table weblogs ( id int , msg string ) partitioned by (continent string, country string, time string) clustered by (id) into 5 buckets stored as orc;
	
	com.it18zhang.myflume160.source.MySource


avro sink
----------------
	1.创建avro-r.conf
		#comonents
		a1.sources = r1
		a1.channels = c1
		a1.sinks=s1
	
		#source
		a1.sources.r1.type = avro
		a1.sources.r1.bind=0.0.0.0
		a1.sources.r1.port = 9999
	
		#sink
		a1.sinks.s1.type=logger
	
		#channel
		a1.channels.c1.type=memory
	
		#bind
		a1.sources.r1.channels = c1
		a1.sinks.s1.channel=c1
	2.创建avro-s.conf
		#comonents
		a1.sources = r1
		a1.channels = c1
		a1.sinks=s1
	
		#source
		a1.sources.r1.type = netcat
		a1.sources.r1.bind=0.0.0.0
		a1.sources.r1.port = 8888
	
		#sink
		a1.sinks.s1.type=avro
		a1.sinks.s1.hostname=localhost
		a1.sinks.s1.port=9999
	
		#channel
		a1.channels.c1.type=memory
	
		#bind
		a1.sources.r1.channels = c1
		a1.sinks.s1.channel=c1
	
	3.先启动avro-r.conf
		$>flume-ng agent --conf-file avro-r.conf -n a1 -Dflume.root.logger=INFO,console
	
	4.启动avro-s.conf
		$>flume-ng agent --conf-file avro-s.conf -n a1
	
	5.启动netcat
		$>nc localhost 8888
			hello worl
file roll sink
--------------------
	本地文件系统滚动存储日志.
	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = file_roll
	a1.sinks.k1.channel = c1
	a1.sinks.k1.sink.directory = /var/log/flume

hbase sink
--------------------
	a1.channels = c1
	a1.sinks = k1
	a1.sinks.k1.type = hbase
	a1.sinks.k1.table = foo_table
	a1.sinks.k1.columnFamily = bar_cf
	a1.sinks.k1.serializer = org.apache.flume.sink.hbase.RegexHbaseEventSerializer
	a1.sinks.k1.channel = c1

hive sink??

hbase sink ?? 方法找不到


customSink
-----------------
	1.创建MySink类。
		/**
		 * custom sink ;
		 */
		public class MySink extends AbstractSink {
	
			public Status process() throws EventDeliveryException {
				Channel c = getChannel();
				Event e = c.take();
				if(e != null){
					String str = new String(e.getBody());
					System.out.println(str);
					return Status.READY;
				}
				else{
					return Status.BACKOFF;
				}
			}
		}
	
	2.导出jar,放到flume classpath
		..
	3.配置custom-s.conf
		a1.sources = r1
		a1.sinks = k1
		a1.channels = c1
	
		# Describe/configure the source
		a1.sources.r1.type = netcat
		a1.sources.r1.bind = 0.0.0.0
		a1.sources.r1.port = 8888


		# Custom sink
		a1.sinks.k1.type = com.it18zhang.myflume160.sink.MySink
	
		# Use a channel which buffers events in memory
		a1.channels.c1.type = memory
	
		# Bind the source and sink to the channel
		a1.sources.r1.channels = c1
		a1.sinks.k1.channel = c1
	
	4.测试


jdbc-channel
------------------
	1.创建msql channel
		package com.it18zhang.myflume160.channel;
	
		import java.sql.Connection;
		import java.sql.DriverManager;
		import java.sql.PreparedStatement;
		import java.sql.ResultSet;
	
		import org.apache.flume.ChannelException;
		import org.apache.flume.Event;
		import org.apache.flume.Transaction;
		import org.apache.flume.channel.AbstractChannel;
		import org.apache.flume.event.SimpleEvent;
	
		public class MyChannel extends AbstractChannel {
	
			static{
				try {
					Class.forName("com.mysql.jdbc.Driver");
					
				} catch (Exception e) {
				}
			}
			
			public synchronized void start() {
				super.start();
			}
			public void put(Event event) throws ChannelException {
				Transaction tx = getTransaction();
				try {
					tx.begin();
					
					Connection conn = DriverManager.getConnection("jdbc:mysql://192.168.231.1:3306/test", "root", "root");
					String msg = new String(event.getBody());
					PreparedStatement ppst = conn.prepareStatement("insert into t1(msg) values('"+msg+"')");
					ppst.executeUpdate();
					ppst.close();
					
					tx.commit();
				} catch (Exception e) {
					tx.rollback();
				}
				tx.close();
			}
	
			public Event take() throws ChannelException {
				Transaction tx = getTransaction();
				try {
					tx.begin();
					
					Connection conn = DriverManager.getConnection("jdbc:mysql://192.168.231.1:3306/test", "root", "root");
					conn.setAutoCommit(false);
					PreparedStatement ppst = conn.prepareStatement("select id, msg from t1");
					ResultSet rs = ppst.executeQuery();
					int id = 0 ;
					String msg = null ;
					if(rs.next()){
						id = rs.getInt(1);
						msg = rs.getString(2);
					}
					conn.prepareStatement("delete from t1 where id = " + id ).executeUpdate();
					conn.commit();
					
					rs.close();
					ppst.close();
					conn.close();
					SimpleEvent e = new SimpleEvent();
					e.setBody(msg.getBytes());
					return e ;
				} catch (Exception e) {
					tx.rollback();
				}
				tx.close();
				return null ;
			}
	
			public Transaction getTransaction() {
				return new Transaction(){
					public void begin() {
					}
	
					public void commit() {
					}
	
					public void rollback() {
					}
	
					public void close() {
					}
				};
			}
	
		}
	
	2.到包jar
		com.it18zhang.myflume160.channel.MyChannel

file channel
----------------
	a1.channels = c1
	a1.channels.c1.type = file
	a1.channels.c1.checkpointDir = /mnt/flume/checkpoint
	a1.channels.c1.dataDirs = /mnt/flume/data

可溢出的内存通道
-----------------------------
	a1.channels = c1
	a1.channels.c1.type = SPILLABLEMEMORY
	a1.channels.c1.memoryCapacity = 10000
	a1.channels.c1.overflowCapacity = 1000000
	a1.channels.c1.byteCapacity = 800000
	a1.channels.c1.checkpointDir = /mnt/flume/checkpoint
	a1.channels.c1.dataDirs = /mnt/flume/data


复本channel selector,每个通道都要存放副本
----------------------------------------
	a1.sources = r1
	a1.channels = c1 c2 c3
	a1.source.r1.selector.type = replicating
	a1.source.r1.channels = c1 c2 c3
	a1.source.r1.selector.optional = c3

Multiplexing Channel Selector
复用通道选择器:
---------------------------------
	1.创建配置文件
		a1.sources = r1
		a1.channels = c1 c2 c3 c4
		a1.sources.r1.selector.type = multiplexing
		a1.sources.r1.selector.header = state
		a1.sources.r1.selector.mapping.CZ = c1
		a1.sources.r1.selector.mapping.US = c2 c3
		a1.sources.r1.selector.default = c4
	
	2.创建头文件
		header.txt
		state=US
	
	3.启动flume agent
	
	4.使用flume-ng avro-client 
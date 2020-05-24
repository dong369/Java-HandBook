# 1 HDFS API

## 1.1 访问文件系统

使用hadoop hdfs API访问hdfs文件系统

	1、Configuration
		配置对象,加载配置文件。
		addDefaultResource();从classpath加载。
		
	2、FileSystem
		DistributedFileSystem，是FileSystem的一个实现，用于和hadoop的hdfs进行交互。
	
	3、Path
		文件或者目录的名称。/是路径分隔符,有绝对路径和相对路径之分。
		/x/x/xx/				// 绝对路径
		./hello.txt				// 相对路径 /user/ubuntu/hello.txt
		data/hello.txt			// 对应

1.2 block
-------------------
	128M,
	
	磁盘IO速率100M/s，普通机械硬盘，固态硬盘会更快
	让磁盘寻道时间(10ms)占用数据传输的1%，0.01s * 100 * 100/s = 100M = 128M

1.3 多目录结构
--------------------
	1、Namenode
		是副本概念。
	2、Datanode
		不是副本。

## 1.4 插件安装

在eclipse安装hadoop插件，方便访问hdfs

	1.下载hadoop2x-eclipse-plugin.zip
	2.解压hadoop2x-eclipse-plugin.zip/release/*.jar(3)到${eclipse_home}/plugins/下
	3.重启eclipse
	4.验证hadoop插件是否安装成功



1.5 网络拓扑
-------------------
	节点距离:两个节点达到最近的共同的祖先的跃点(经过交换机的次数)数。
	同一节点的两个进程			:	0
	同一机架不同节点的两个进程	:	2
	不同机架不同节点的两个进程	:	4

剖析文件写入过程
---------------------
	1.
	2.
	3.
	4.

副本节点的选择(机架感知)
-------------------------
	1.默认
		第一个副本在client所处的节点上。如果客户端在集群外，随机选一个。
		第二个副本和第一个不同机架的随机节点上。
		第三个副本和第二个副本相同机架，节点随机。
	2.hadoop2.7.2是
		第一个副本在client所处的节点上。如果客户端在集群外，随机选一个.
		第二个副本和第一个为相同机架.
		第三个部分位于不同机架。


自定义机架感知
----------------
	0.数据节点的量
		[rack1]
		s100
		s101
		s102
		
		[rack2]
		s103
		s104
		s105
		s106
	
	1.创建类实现DNSToSwitchMapping接口
		package com.it18zhang.rackaware;
		import java.io.FileOutputStream;
		import java.util.ArrayList;
		import java.util.List;
		import org.apache.hadoop.net.DNSToSwitchMapping;
		public class MyDNSToSwichMapping implements DNSToSwitchMapping {
	
			/**
			 * 传递的是客户端的ip列表,返回机架感知的路径列表
			 */
			public List<String> resolve(List<String> names) {



				List<String> list = new ArrayList<String>();
				if (names != null && names.size() > 0) {
					for (String name : names) {
						// 主机名 s101,
						int ip = 0;
						if (name.startsWith("s")) {
							String no = name.substring(1);
							ip = Integer.parseInt(no);
						}
						//
						else if (name.startsWith("192")) {
							ip = Integer.parseInt(name.substring(name.lastIndexOf(".") + 1));
						}
						//
						if (ip < 103) {
							list.add("/rack1/" + ip);
						} else {
							list.add("/rack2/" + ip);
						}
					}
				}
				//写入文件中
				try {
					FileOutputStream fos = new FileOutputStream("/home/ubuntu/dns.txt");
					for (String name : list) {
						fos.write((name + "\r\n").getBytes());
					}
					fos.close();
				} catch (Exception e) {
					e.printStackTrace();
				}
				return list ;
			}
	
			public void reloadCachedMappings() {
			}
	
			public void reloadCachedMappings(List<String> names) {
			}
		}
	
	2.配置文件core-site.xml
		[core-site.xml]
		<property>
		  <name>net.topology.node.switch.mapping.impl</name>
		  <value>com.it18zhang.rackaware.MyDNSToSwichMapping</value>
		</property>
	3.分发core-site.xml
	4.编译程序，打成jar，分发所有几点的hadoop的classpath下。
		/soft/hadoop/shared/common/lib

增加一个数据节点
-----------------------	
	1.克隆一个节点
	2.启动新节点
	3.修改新克隆的ip和主机名
		[/etc/hostname]
		s105
	
		[/etc/network/interfaces]
	
	4.修s100的/etc/hosts文件，然后分发
		[/etc/hosts]
		...
		192.168.231.106
	5.在s100上ssh到新节点
		首次需要yes
	
	6.修改xsync.sh 和 xcall.sh文件
		
	6.修改s100 slaves文件,再分发.
		[${hadoop_home}/etc/hadoop/slaves]

filesystem:
--------------------------------------
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s101
/center-1/layer-1/room-1/rack-2/s100

/center-3/layer-3/room-2/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100
/center-1/layer-1/room-1/rack-1/s100

hdfs架构
---------------------
	运行廉价的硬件之上的，成本较低，访问大型数据集，具有容错的特性，容错机制。
	hdfs是master/slave架够，由一个名称节点以及多个数据节点构成。nn负责namespace管理以及client的访问。
	内部文件被切块存储在数据节点的集合中。
	
	Namenode
	-------------
		存放元数据(名称,副本数,权限,块列表,)，不包含数据节点。
	
	rack aware策略
	--------------

## 日志文件

使用oie和oev查看namenode的镜像文件和编辑日志文件

	离线工具。
	$>hdfs oiv -p XML -i xxx -o ~/xxx.xml		//fsimage
	$>hdfs oev -p XML -i xxx -o ~/xxx.xml		//edit log


win7运行hadoop程序
---------------------
	需要dll,exe文件支持,默认hadoop发布包不含这些文件。
	1.解压bin.war文件到hadoop/bin目录下，替换所有文件.
	2.配置环境变量
		path=...;../hadoop/bin;../hadoop/sbin
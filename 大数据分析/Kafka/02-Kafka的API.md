1 Kafka
======================
	分布式流计算平台。
	类似于JMS消息系统发布订阅数据流。
	以分布式、副本集群方式存储数据流。
	实时处理数据流。
	
	构建实时数据流管道，水平可伸缩，容错，速度快
	
	特点
	----------------
		1.巨量数据，TB级
		2.高吞吐量
			支持每秒中百万消息。
		3.分布式。
			支持在多个server之间进行消息分区。
		4.多客户端支持
			和多语言进行协同。

1.1 组件
--------
	1、broker			// kafka服务器	zk
		log + index
		test3-1			// 主题 + 分区号
		zk znode : /brokers/topics/test3/partitions/0|1|2/state(leader isr(101,102,broker id))
	
	2、Topic				// 主题		zk
	3、producer			// 生产者		no zk
	4、consumer			// 消费者		zk


1.2 生产者
-------------
```java
package cn.hacz.kafka;

import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.clients.producer.ProducerRecord;
import org.apache.kafka.common.serialization.StringSerializer;
import org.junit.Before;
import org.junit.Test;

import java.util.Properties;

/**
 * The class/interface
 *
 * @author guodd
 * @version 1.0 use jdk 1.8
 */
public class KafkaProducerTest {
    // kafka地址
    final String single = "s10:9092";
    final String list = "s10:9092,s10:9093,s10:9094";
    // 主题
    final String topic = "test";
    // 属性配置
    Properties properties;

    @Before
    public void before() {
        properties = new Properties();
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, list);
        properties.put(ProducerConfig.CLIENT_ID_CONFIG, "producer.client.id.test");
        properties.put(ProducerConfig.KEY_SERIALIZER_CLASS_CONFIG, StringSerializer.class.getName());
        properties.put(ProducerConfig.VALUE_SERIALIZER_CLASS_CONFIG, "org.apache.kafka.common.serialization.StringSerializer");
    }

    /**
     * 进行数据的生产
     */
    @Test
    public void producer() {
        // 配置生产者客户端参数及创建相应的生产者实例。
        org.apache.kafka.clients.producer.KafkaProducer<String, String> producer 
            = new org.apache.kafka.clients.producer.KafkaProducer<>(properties);
        // 构建待发送的消息。
        ProducerRecord<String, String> topicInfo = new ProducerRecord<>(topic, "hello");
        // 发消息
        producer.send(topicInfo);
        // 关闭
        producer.close();
    }
}
```

## 1.3 发送消息

KafkaProducer发送消息分析

	生成者是线程安全的，维护了本地buffer pool，发送消息，消息进入pool，
	send方式异步，立刻返回。
	ack=all导致记录完全提交时阻塞，
												对消息进行批处理
	p.send(rec) -->p.doSend(rec,callback) --> interceptors.onSend() --> 对key+value进行串行化 -> 计算分区 ->
	
	->计算总size -> 创建TopicPartition对象 ->创建回调拦截器 -> **** Sender *****

## 1.4 分区函数

使用带有分区函数的生产者发送消息

	//new API
	public class SimplePartitioner2 implements Partitioner {
	
		public void configure(Map<String, ?> configs) {
		}
	
		public int partition(String topic, Object key, byte[] keyBytes, Object value, byte[] valueBytes, Cluster cluster) {
			return 1;
		}
	
		public void close() {
		}
	}
	
	public static void main(String[] args) {
		Properties props = new Properties();
		props.put("bootstrap.servers", "s101:9092");	//broker list
		props.put("acks", "all");
		props.put("retries", 0);
		props.put("batch.size", 16384);
		props.put("linger.ms", 1);
		props.put("buffer.memory", 33554432);
		props.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
		props.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");
		props.put("partitioner", "com.it18zhang.mykafka01001.SimplePartitioner2");
		
		Producer<String, String> producer = new KafkaProducer<String, String>(props);
		for (int i = 0; i < 5; i++){
			ProducerRecord<String, String> rec = new ProducerRecord<String, String>("test3", Integer.toString(i), "WWW" + i);
			producer.send(rec, new Callback() {
				public void onCompletion(RecordMetadata metadata, Exception exception) {
					System.out.println("ack !!");
				}
			});
		}
		producer.close();
		System.out.println("over");
	}

1.5 消费者
-----------------
```java
package cn.hacz.kafka;

import org.apache.kafka.clients.consumer.ConsumerConfig;
import org.apache.kafka.clients.consumer.ConsumerRecord;
import org.apache.kafka.clients.consumer.ConsumerRecords;
import org.apache.kafka.clients.consumer.KafkaConsumer;
import org.apache.kafka.clients.producer.ProducerConfig;
import org.apache.kafka.common.PartitionInfo;
import org.apache.kafka.common.TopicPartition;
import org.apache.kafka.common.serialization.StringDeserializer;
import org.junit.Before;
import org.junit.Test;

import java.time.Duration;
import java.util.*;
import java.util.concurrent.atomic.AtomicBoolean;

/**
 * The class/interface
 *
 * @author guodd
 * @version 1.0 use jdk 1.8
 */
public class KafkaConsumerTest {
    // kafka地址
    final String single = "s10:9092";
    final String list = "s10:9092,s10:9093,s10:9094";
    // 主题
    final String topic = "test";
    // 属性配置
    Properties properties;
    // 监听
    AtomicBoolean isRunning = new AtomicBoolean(true);

    @Before
    public void before() {
        // 配置四个必填参数
        properties = new Properties();
        // broker服务清单
        properties.put(ProducerConfig.BOOTSTRAP_SERVERS_CONFIG, list);
        // 消费者隶属的消费组的名称，默认值为""
        properties.put(ConsumerConfig.GROUP_ID_CONFIG, "group.id");
        properties.put(ProducerConfig.CLIENT_ID_CONFIG, "producer.client.id.test");
        // 序列化，生产者使用的是StringSerializer序列化
        properties.put(ConsumerConfig.KEY_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
        properties.put(ConsumerConfig.VALUE_DESERIALIZER_CLASS_CONFIG, StringDeserializer.class.getName());
    }

    @Test
    public void consumerInfo() {
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(properties);
        List<PartitionInfo> partitionInfos = consumer.partitionsFor(topic);
        for (PartitionInfo partitionInfo : partitionInfos) {
            System.out.println(Arrays.toString(partitionInfo.replicas()) + "，" + partitionInfo.partition());
        }
        consumer.close();
    }

    /**
     * 消费者
     */
    @Test
    public void consumer() {
        // 配置消费者客户端参数及创建相应的消费者实例。
        KafkaConsumer<String, String> consumer = new KafkaConsumer<>(properties);
        // 订阅主题
        consumer.subscribe(Collections.singletonList(topic));
        // 开始消费的分区
        // consumer.beginningOffsets(Collections.singletonList(new TopicPartition(topic, 1)));
        Map<TopicPartition, Long> timestampsToSearch = new HashMap<>();
        timestampsToSearch.put(new TopicPartition(topic, 0), 100000000L);
        consumer.offsetsForTimes(timestampsToSearch);
        // 可以指定消费的分区
        // consumer.assign(Collections.singletonList(new TopicPartition(topic, 1)));
        // 消费主题
        while (isRunning.get()) {
            ConsumerRecords<String, String> poll = consumer.poll(Duration.ofMillis(1000));
            for (ConsumerRecord<String, String> record : poll) {
                System.out.println(record.topic() + "," + record.value());
            }
            // 提交消费位移
            // consumer.commitAsync();
            // OffsetAndMetadata committed = consumer.committed(new TopicPartition("", 1));
        }
        // 关闭实例
        consumer.close();
    }
}
```

/consumers/g1
/consumers/g1/ids
/consumers/g1/ids/g1_Thinkpad-PC-1475218254850-a0080320
/consumers/g1/ids/g1_Thinkpad-PC-1475218293130-89728e2a
/consumers/g1/owners
/consumers/g1/owners/test3
/consumers/g1/owners/test3/0
/consumers/g1/owners/test3/1
/consumers/g1/owners/test3/2
/consumers/g1/offsets
/consumers/g1/offsets/test3
/consumers/g1/offsets/test3/0
/consumers/g1/offsets/test3/1
/consumers/g1/offsets/test3/2

消费者消费的偏移量记载的是消费者组消费的该分区的消息个数。


1.6 flume整合kafka
--------------------
	1.kafka Source
		tier1.sources.source1.type = org.apache.flume.source.kafka.KafkaSource
		tier1.sources.source1.channels = c1
		tier1.sources.source1.zookeeperConnect = s101:2181
		tier1.sources.source1.topic = test3
		tier1.sources.source1.groupId = g1
		tier1.sources.source1.kafka.consumer.timeout.ms = 100
	
	2.Kafka Sink
		a1.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink
		a1.sinks.k1.topic = test3
		a1.sinks.k1.brokerList = s101:9092
		a1.sinks.k1.requiredAcks = 1
		a1.sinks.k1.batchSize = 20
		a1.sinks.k1.channel = c1
	
	3.kafka Channel
		a1.sources = r1
		a1.sinks = k1
		a1.channels = c1
	
		# Describe/configure the source
		a1.sources.r1.type = netcat
		a1.sources.r1.bind = 0.0.0.0
		a1.sources.r1.port = 8888
	
		# Describe the sink
		a1.sinks.k1.type = logger
	
		# Use a channel which buffers events in memory
		a1.channels.c1.type   = org.apache.flume.channel.kafka.KafkaChannel
		a1.channels.c1.capacity = 10000
		a1.channels.c1.transactionCapacity = 1000
		a1.channels.c1.brokerList=s101:9092,s102:9092
		a1.channels.c1.topic=test3
		a1.channels.c1.zookeeperConnect=s101:2181
		a1.channels.c1.parseAsFlumeEvent=false		//*****?????
	
		# Bind the source and sink to the channel
		a1.sources.r1.channels = c1
		a1.sinks.k1.channel = c1


​	
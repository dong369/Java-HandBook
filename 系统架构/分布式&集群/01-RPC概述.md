# 1 RPC

RPC是远程服务调用，是一种**进程间**的通信方式，他是一种技术的思想，不是规范。

RPC的实现产品：Dubbo不能跨语言，只是Java语言使用；Thrift是可以跨语言的

RPC的核心：通讯、序列化和反序列

分布式的三个指标CAP：一致性、可用性、分区容错

# 2 CAP

Consistency：一致性

Availability：可用性

Partition tolerance：分区容错

CAP理论就是说在分布式存储系统中，最多只能实现上面的两点。而由于当前的网络硬件肯
定会出现延迟丢包等问题，所以分区容忍性是我们必须需要实现的，所以我们只能在一致性
和可用性之间进行权衡，没有 NoSQL系统能同时保证这三点。CP、AP

Zookeeper是CP，Zookeeper如何在分布式系统中实现一致性的[RAFT算法](http://thesecretlivesofdata.com/raft/)。

Redis是AP，可以添加哨兵

# 3 BASE

BASE：Basically Available（基本可用）、Soft stater软状态）、Eventually consistent（最终一致性）

三个短语的简写，BASE是对CAP中一致性和可用性权衡的结果，其来源于对大规模互联网系统分布式实践的结论，是基于CAP定理逐步演化而来的，其核心思想是即使无法做到强一致性（ trong consistency），但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性（ Eventual consistency）。接下来我们着重对BASE中的三要素进行详细讲解。
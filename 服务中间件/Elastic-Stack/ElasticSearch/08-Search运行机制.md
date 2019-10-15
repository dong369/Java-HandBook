# 1 Search的运行机制

Search执行的时候实际分两个步骤运作的
Query阶段
Fetch阶段
Query-Then-Fetch

## 1.1 Query阶段

node3在接收到用户的 search请求后，会先进行 Query阶段（此时是 Coordinating
Node角色）
2node3在6个主副分片中随机选择3个分片，发送 search request
3.被选中的3个分片会分别执行查询并排序，返回from+sze个文档ld和排序值
4.node3整合3个分片返回的from+sie个文档ld，根据排序值排序后选取from到
from+size的文档ld

## 1.2 Fetch阶段

·node3根据 Query阶段获取的文档ld列表去对应的 shard上获取文档详情数据
1.node3向相关的分片发送 multi_get请求
2.3个分片返回文档详细数据
3.node3拼接返回的结果并返回给客户



# 2 相关性算分问题

相关性算分在 shard与 shard间是相互独立的，也就意味着同-个Term的IF等值在
不同 shard上是不同的。文档的相关性算分和它所处的 shard相关
在文档数量不多时，会导致相关性算分严重不准的情况发生

解决思路有两个
一是设置分片数为1个，从根本上排除问题，在文档数量不多的时候可以考虑该方案，比如百万到干万级别的文栏数量。
二是使用 DFS Query-then-Fetch查询方式

DFS Query-then- Fetch是在拿到所有文档后再重新完整的计算一次相关性算分，耗费更
多的cpu和内存，执行性能也比较低下，一般不建议使用。使用方式如下

# 3 排序

ES默认会采用相关性算分排序，用户可以通过设定 sorting参数来自行设定排序规则

按照字符串排序比较特殊，因为ES有text和 keyword两种类型，针对text类型排序，
如下所示

排序的过程实质是对字段原始内容排序的过程，这个过程中倒排索引无法发挥作用，需
要用到正排索引，也就是通过文档ld和字段可以快速得到字段原始内容。
es对此提供了2种实现方式：
fielddata默认禁用
doc values默认启用，除了text类型

# 4 分页与遍历
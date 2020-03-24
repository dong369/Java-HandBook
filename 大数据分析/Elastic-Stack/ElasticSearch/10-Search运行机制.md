# 1 Search的运行机制

Search执行的时候实际分两个步骤运作：Query阶段、Fetch阶段、Query-Then-Fetch。

## 1.1 Query阶段

node3在接收到用户的search请求后，会先进行 Query阶段（此时是Coordinating Node角色）

node3在6个主副分片中**随机选择3个分片**，发送 search request；

被选中的3个分片会分别执行查询并排序，返回from+sze个文档ld和排序值；

node3整合3个分片返回的from+sie个文档ld，根据排序值排序后选取from到from+size的文档ld

## 1.2 Fetch阶段

node3根据Query阶段获取的文档ld列表去对应的 shard上获取文档详情数据

node3向相关的分片发送multi_get请求

3个分片返回文档详细数据

node3拼接返回的结果并返回给客户

# 2 相关性算分问题

## 2.1 理论

相关性算分在 shard与shard间是**相互独立**的，也就意味着同一个Term的IDF等值在不同shard上是不同的。

文档的相关性算分和它所处的 shard相关。

在文档数量不多时，会导致相关性算分严重不准的情况发生

解决思路有两个
一是设置分片数为1个，从根本上排除问题，在文档数量不多的时候可以考虑该方案，比如百万到干万级别的文栏数量。

二是使用 DFS Query-then-Fetch查询方式

DFS Query-then- Fetch是在拿到所有文档后再重新完整的计算一次相关性算分，耗费更多的cpu和内存，执行性能也比较低下，一般不建议使用。

## 2.2 实践

```json
# dive into search
DELETE test_search_relevance

POST test_search_relevance/_doc
{
  "name":"hello"
}

POST test_search_relevance/_doc
{
  "name":"hello,world"
}

POST test_search_relevance/_doc
{
  "name":"hello,world!a beautiful world"
}

GET test_search_relevance/_search

GET test_search_relevance

GET test_search_relevance/_search
{
  "explain": false, 
  "query": {
    "match":{
      "name":"hello"
    }
  }
}

# 解决方式1：分片设置成一个
PUT test_search_relevance
{
  "settings": {
    "index":{
      "number_of_shards":1
    }
  }
}

# 解决方式二：使用dfs_query_then_fetch查询
GET test_search_relevance/_search?search_type=dfs_query_then_fetch
{
  "query": {
    "match":{
      "name":"hello"
    }
  }
}
```

# 3 排序

ES默认会采用**相关性算分排序**，用户可以通过设定sorting参数来自行设定排序规则。如果指定了排序规则的话，ES将不再计算评分！！！

按照字符串排序比较特殊，因为ES有text和keyword两种类型，针对text类型排序，将会报错。排序的过程实质是对**字段原始内容排序的过程**，这个过程中**倒排索引无法发挥作用**，需要用到正排索引，也就是通过文档ld和字段可以快速得到字段原始内容。

ES对此提供了2种实现方式：fielddata默认禁用；doc values默认启用，除了text类型。

| 对比     | FieldData                                        | DocValues                          |
| -------- | ------------------------------------------------ | ---------------------------------- |
| 创建时机 | 搜索时即时创建                                   | 索引时创建，与倒排索引创建时机一致 |
| 创建位置 | JVM heap                                         | 磁盘                               |
| 优点     | 不会占用额外的磁盘资源                           | 不会占用heap                       |
| 缺点     | 文档过多时，即时创建花过多的时间，占用过多的heap | 减慢索引的速度，占用额外的磁盘资源 |

源码

```json
# 算分是空的
GET test_search_index/_search
{
  "query":{
    "match": {
      "username": "alfred"
    }
  },
  "sort":{
    "birth":"desc"
  }
}

# 多字段排序，会有算分
GET test_search_index/_search
{
  "query":{
    "match": {
      "username": "alfred"
    }
  },
  "sort": [
    {
      "birth": "desc"
    },
    {
      "_score": "desc"
    },
    {
      "_doc": "desc"
    }
  ]
}

PUT test_search_index/doc/0
{
  "username":"aaa"
}

DELETE test_search_index/_doc/2

GET test_search_index

GET test_search_index/_search
{
  "sort": [
    {
      "username": "asc"
    }
  ]
}

GET test_search_index/_search
{
  "sort": [
    {
      "username": "desc"
    },
    {
      "_id": "desc"
    }
  ]
}

DELETE test_search_index/_doc/5

PUT test_search_index/_doc/5
{
  "username":"alfred junior zoo"
}

DELETE test_search_index/_doc/5

GET test_search_index/_search
{
  "sort":{
    "username.keyword":"desc"
  }
}
```

## 3.1 Fielddata

Fielddata**默认是关闭的**，可以通过如下api开启，此时字符串是按照分词后的**term排序**，往往结果很难符合预期
一般是在对分词做聚合分析的时候开启。

**Fielddata只对text类型有效！！！**可以随时开启和关闭的！！！

```json
PUT prod_clinic_info/_mapping
{
  "properties": {
    "messageDeptName": {
      "type": "text",
      "analyzer": "ik_max_word",
      "search_analyzer": "ik_smart",
      "fielddata": false
    }
  }
}

# fielddata
PUT prod_clinic_info/_mapping
{
  "properties": {
    "messageDeptName": {
      "type": "text",
      "analyzer": "ik_max_word",
      "search_analyzer": "ik_smart",
      "fielddata": true
    }
  }
}
```

## 3.2 Doc Values

doc_values**默认是启用的**，除了text类型。

明确的知道这个字段不需要排序和聚合操作，可以在创建索引的时候关闭；

如果后面要再开启doc_values，需要做reindex操作。

```json
# doc values
DELETE test_doc_values

PUT test_doc_values/_doc/1
{
  "properties": {
    "username": {
      "type": "keyword",
      "doc_values": true
    },
    "hobby": {
      "type": "keyword"
    }
  }
}

PUT test_doc_values1/
{
  "mappings": {
    "properties": {
      "username": {
        "type": "keyword",
        "doc_values": false
      }
    }
  }
}

PUT test_doc_values/_doc/1
{
  "username":"alfred",
  "hobby":"basketball"
}

GET test_doc_values/_search
{
  "sort":"username"
}

GET test_doc_values/_search
{
  "sort":"hobby"
}

# can be used to get original field value for not stored field
PUT test_search_index/_mapping/_doc
{
  "properties": {
    "username":{
      "type":"text",
      "fielddata": true
    }
  }
}
```

| 对比     | FieldData                | DocValues                    |
| -------- | ------------------------ | ---------------------------- |
| 创建时机 | 搜索时及时创建           | 创建索引时，和倒排索引一致   |
| 创建位置 | JVM Heap                 | 磁盘                         |
| 优点     | 不占用额外磁盘资源       | 不占用Heap                   |
| 缺点     | 文档过多时，占用Heap过多 | 减慢索引的速度，占用额外资源 |

## 3.3 Docvalue_fields

可以通过该字段获取fielddata或者doc values中存储的内容，**默认是开启的**！！！

```json
GET test_search_index/_search
{
  "docvalue_fields": [
    "username",
    "username.keyword",
    "age"
  ]
}
```

# 4 分页与遍历

ES提供了3种方式来解决分页与遍历的问题

## 4.1 from/size

from指明开始位置；size指明获取的条数。

深度分页是一个经典的问题：在数据分片存储的情况下如何获取前1000个文档？

获取从990~1000的文档时，会在每个分片上都先获取1000个文档，然后再由Coordinating Node聚合所有分片的结果后再排序选取前1000个文档。

页数越深，处理文档越多，占用内存越多，耗时越长。尽量避免深度分页，ES通过index.max_result_window限定最多到10000条数据。

total_page = (total+page_size-1)/page_size

```json
# pagination
GET test_search_index/_search
{
  "from":4,
  "size":2
}

# total_page=(total+page_size-1)/page_size
GET test_search_index/_search
{
  "from":9998,
  "size":2
}

# ES通过index.max_result_window修改search查询最大查询条数，默认是10000
PUT /search_model/_settings
{
  "max_result_window": "10000000"
}

# ES通过index.mapping.nested_objects.limit修改search查询最大查询条数，默认是10000
PUT /search_model/_settings
{
  "index.mapping.nested_objects.limit": 10000
}

# track_total_hits查询search数据
GET search_model/_search
{
  "track_total_hits": true,
  "query": {
    "match_all": {}
  }
}

# ES通过search.max_buckets修改聚合最大查询条数，默认是10000
PUT _cluster/settings
{
  "transient": {
    "search.max_buckets": 10000000
  }
}

# track_total_hits查询聚合数据
GET /prod_clinic_info/_search
{
  "track_total_hits": true,
  "size": 0,
  "aggregations": {
    "desensitizeHospitalPatientId": {
      "terms": {
        "field": "desensitizeHospitalPatientId",
        "size": 100000
      }
    }
  }
}
```

## 4.2 scroll

遍历文档集的api，**以快照的方式**来避免深度分页的问题，不能用来做实时搜索，因为数据**不是实时**的尽量不要使用复杂的sort条件，使用doc最高效使用稍嫌复杂。

如果创建快照后添加（删除）数据，是不能被查询到的！！！

```json
# scroll为五分钟有效
GET prod_clinic_info/_search?scroll=5m
{
  "size": 1
}

POST /_search/scroll
{
  "scroll" : "5m", 
  "scroll_id": "DnF1ZXJ5VGhlbkZldGNoAwAAAAAAWMxLFlVkOHI3UllaU2ttajB"
}

# 过多的scroll调用会占用大量内存，可以通过 clear api删除过多的scroll快照
DELETE /_search/scroll
{
    "scroll_id":[
        "DnF1ZXJ5VGhlbkZldGNoAwAAAAAAWMxLFlVkOHI3UllaU2ttajB",
        "DnF1ZXJ5VGhlbkZldGNoAwAAAAAAWMxLFlVkOHI3UllaU2ttaja"
    ]
}

DELETE _search/scroll/_all

# new doc can not be searched
PUT test_search_index/doc/10
{
  "username":"doc10"
}

DELETE test_search_index/_doc/10
```

## 4.3 search after

**避免深度分页的性能问题**，**提供实时**的下一页文档获取功能。缺点是不能使用from参数，即不能指定页数；**只能下一页，不能上一页**；使用简单。

第一步为正常的搜索，但要指定sort值，并**保证值唯一**；

第二步为使用上一步最后一个文档的sort值进行查询。

如何避免深度分页问题？通过唯排序值定位将每次要处理的文档徵都控制在size内；

```json
GET /prod_clinic_info/_search
{
  "size": 1,
  "sort": [
    {
      "patientId": "desc",
      "_id": "desc"
    }
  ]
}

GET /prod_clinic_info/_search
{
  "size": 1,
  "search_after": [
    "TG00000002",
    "1000012001_TG00000002_9804_555500008_01"
  ],
  "sort": [
    {
      "_id": {
        "order": "desc"
      },
      "patientId": {
        "order": "desc"
      }
    }
  ]
}
```

| 类型         | 场景                                       |
| ------------ | ------------------------------------------ |
| From/Size    | 需要实时获取顶部的部分文档，且需要自由分页 |
| Scroll       | 需要全部文档，如导出所有数据的功能         |
| Search_After | 需要全部文档，不需要自由分页               |


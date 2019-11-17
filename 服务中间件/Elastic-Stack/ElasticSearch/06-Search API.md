# 1 概念理解

实现对es中存储的数据进行查询分析， endpoint为_search。

GET /_search

GET /my_index/_search

GET /my_index1,my_index2/_search

GET /my_s*/_search

# 2 两种形式

## 2.1 URI Search

### 2.1.1 语法操作

操作简便，方便通过命令行测试；**仅包含部分查询语法**。通过 url query参数来实现搜索，常用参数如下：

q：指定查询的语句，语法为Query String Syntax。

df：q中不指定字段时默认查询的字段，如果不指定，ES会查询所有字段。

sort：排序

timeout：指定超时时间，默认不超时

from，size：用于分页

语法01：get /my_index/_search?q=username:alfred&&sort=age:asc&from=4&sixe=10&timeout=1s

语法02：get /my_index/_search?q=alfred&df=username&sort=age:asc&from=4&sixe=10&timeout=1s

语义：查询username字段包含alfred的文档，结果按照age升序排序，返回第5-14的文档数据，如果超过一秒没有结束，则超时结束！

### 2.1.2 Query String Syntax

测试案例初始化数据

```json
# 删除
DELETE test_search_index

# 创建索引
PUT test_search_index
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  }
}

# 添加文档，注意代码不能美化执行，会报错
POST test_search_index/_bulk
{"index":{"_id":"1"}}
{"username":"guo dong","job":"java engineer","age":26,"birth":"1993-01-02","isMarried":false}
{"index":{"_id":"2"}}
{"username":"guo","job":"java senior engineer and java specialist","age":28,"birth":"1980-05-07","isMarried":true}
{"index":{"_id":"3"}}
{"username":"郭冬冬","job":"java and ruby engineer","age":20,"birth":"1999-08-07","isMarried":false}
{"index":{"_id":"4"}}
{"username":"郭冬东","job":"ruby engineer","age":30,"birth":"1989-08-07","isMarried":false}
{"index":{"_id":"5"}}
{"username":"郭冬","job":"ruby engineer","age":40,"birth":"1979-08-07","isMarried":false}
{"index":{"_id":"6"}}
{"username":"郭东升","job":"ruby engineer","age":8,"birth":"2011-08-07","isMarried":false}
{"index":{"_id":"7"}}
{"username":"郭东","job":"ruby engineer","age":50,"birth":"1969-08-07","isMarried":false}

# 查看文档设置
GET test_search_index/_settings

# 查看文档mapping字段
GET test_search_index/_mapping

# 验证数据查询
GET test_search_index/_search
```

#### 2.1.2.1 term与phrase

term是一个个的单词，phrase是一个词语是有顺序的！！！

alfred way等效于alfred OR way，满足其中一个就会返回。"alfred way"词语查询，要求先后顺序。

```json
GET test_search_index/_search?q=username:alfred way
{
  "profile": true
}

GET test_search_index/_search?q=username:"alfred way"
{
  "profile": true
}

GET test_search_index/_search?q=username:(alfred way)
{
  "profile": true
}
```

#### 2.1.2.2 泛查询

泛查询是不指定字段的查询，alfred等效于在**所有字段**去匹配该term。

```json
GET test_search_index/_search?q="郭冬"
GET test_search_index/_search?q=郭冬
GET test_search_index/_search?q=(郭冬)
# 查看ES执行的语句
GET test_search_index/_search?q="郭冬"
{
  "profile": true
}
```

#### 2.1.2.3 指定字段

name:alfred

```json
GET test_search_index/_search?q=username:"郭冬"
GET test_search_index/_search?q=username:郭冬
GET test_search_index/_search?q=username:(郭冬)
# 查看ES执行的语句
GET test_search_index/_search?q=username:"郭冬"
{
  "profile": true
}
```

#### 2.1.2.4 分组

Group分组设定，**使用括号指定匹配**的规则：

(A OR B) AND C

A:(B OR C)

A:(B C D)

```json
GET test_search_index/_search?q=username:alfred AND way
GET test_search_index/_search?q=username:(alfred AND way)
{
  "profile": "true"
}
```

#### 2.1.2.5 布尔操作符

AND（&&）， OR（||）， NOT（！）
name:(tom NOT lee)
**注意大写，不能小写**
+、-分别对应must和 must not，+在url中会被**解析为空格**，要使用encode后的结果才可以，为%2B。
name：（ tom +lee -alfred）
name：（（lee && !alfred）||（tom && lee && !alfred））

```json
GET test_search_index/_search?q=username:alfred AND way
{
  "profile": "true"
}

GET test_search_index/_search?q=username:(alfred AND way)
{
  "profile": "true"
}

GET test_search_index/_search?q=username:(alfred NOT way)
{
  "profile": "true"
}

GET test_search_index/_search?q=username:(alfred +way)
{
  "profile": "true"
}

GET test_search_index/_search?q=username:(alfred %2Bway)
{
  "profile": "true"
}

GET test_search_index/_search?q=username:(alfred -way)
{
  "profile": "true"
}
```

#### 2.1.2.6 范围查询

1. 范围查询，支持数值和日期，区间写法，闭区间用[]，开区间用{}
   age：[1 TO 10]意为1<=age<=10
   age：[1 TO 10}意为1<=age<10
   age：[1 TO ]意为age>=1
   age：[* TO 10】意为age<=10
2. 算数符号写法
   age:>=1
   age:(>=1&&<=10) 或者 age:(+>=1+<=10)

```json
GET test_search_index/_search?q=username:alfred age:>26
{
  "profile": "true"
}

GET test_search_index/_search?q=username:alfred AND age:>26
{
  "profile": "true"
}

GET test_search_index/_search?q=birth:(>1980 AND <199)
```

#### 2.1.2.7 通配符查询

`?`代表1个字符，`*`代表0或多个字符
name:t?m
name:tom*
name:t*m
通配符匹配执行**效率低**，且占用较多內存，不建议使用，如无特殊需求，不要将?、/`**`放在最前面

```json
GET test_search_index/_search?q=username:alf*
```

#### 2.1.2.8 正则表达式

name:/[mb]oat/

```json
GET test_search_index/_search?q=username:/[a]?l.*/
```

#### 2.1.2.9 模糊匹配

模糊匹配fuzzy query，name:roam~1。匹配与roam差1个character的词，比如foam roams等

```json
GET test_search_index/_search?q=username:升
GET test_search_index/_search?q=username:升~1

GET test_search_index/_search?q=username:alfed
GET test_search_index/_search?q=username:alfed~1
GET test_search_index/_search?q=username:alfd~2
```

#### 2.1.2.10 近似度匹配

近似度查询 proximity search，"fox quick"~5。以term为单位进行差异比较，比如"quick fox" "quick brown fox"都会被匹配。

```json
GET test_search_index/_search?q=username:"郭升"
GET test_search_index/_search?q=username:"郭升"~1

GET test_search_index/_search?q=job:"java engineer"
GET test_search_index/_search?q=job:"java engineer"~1
GET test_search_index/_search?q=job:"java engineer"~2
```

## 2.2 Request Body search

ES提供的完备查询语法Query DSL（Domain Specific Language）

语法：get /my_index/_search    {"query":{"term":{"user":"alfred"}}}

基于JSON定义的查询语言，主要包含如下两种类型

### 2.2.1 字段类查询

如term（词查询）、terms（词查询）、range（针对词查询）。

match（全文检索）、match_phrase（全文检索查询）等，只针对某一个字段进行查询。

字段类查询主要包括以下两类。

#### 2.2.1.1 全文匹配

针对text类型的字段进行全文检索，会对查询语句先进行分词处理，如 match，match_phrase等query类型。

Simple Query String Query类似query String，但是会忽略错误的查询语法，并且仅支持部分査询语法；

其常用的逻辑符号如下，不能使用AND、OR、NOT等关键词：+代指AND、|代指OR、-代指NOT。

```json
# 对字段作全文检索，最基本和常用的查询类型
GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "match": {
      "username": "alfred way"
    }
  }
}

# 通过operator参数可以控制单词间的匹配关系，可选项为or和and
GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "match": {
      "username": {
        "query": "alfred way",
        "operator": "and"
      }
    }
  }
}

# 通过minimum_should_match参数可以控制需要匹配的单词数
GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "match": {
      "job": {
        "query": "java and ruby engineer",
        "minimum_should_match": 3
      }
    }
  }
}

# Match Phrase Query，对字段作检索，有顺序要求，通过slop参数可以控制单词间的间隔
GET test_search_index/_search
{
  "query": {
    "match_phrase": {
      "username": {
        "query": "郭升",
        "slop": 1
      }
    }
  }
}

# Query String Query，类似于URI Search中的q参数查询
GET test_search_index/_search
{
  "query": {
    "query_string": {
      "fields": [
        "username"
      ],
      "query": "郭冬"
    }
  }
}
GET test_search_index/_search
{
  "query": {
    "query_string": {
      "default_field": "username",
      "query": "郭冬 AND 东"
    }
  }
}
GET test_search_index/_search
{
  "profile": "true", 
  "query": {
    "query_string": {
      "fields": [
        "username",
        "job"
      ],
      "query": "alfred OR (java AND ruby)"
    }
  }
}
# Simple Query String Query，类似query String，但是会忽略错误的査询语法，并且仅支持部分查询语法
GET test_search_index/_search
{
  "profile": "true", 
  "query": {
    "simple_query_string": {
      "fields": [
        "username",
        "job"
      ],
      "query": "alfred +way"
    }
  }
}
```

#### 2.2.1.2 单词匹配

不会对查询语句做分词处理，直接去匹配字段的倒排索引，如term，terms，range等 query类型。

Term Query将査询语句作为整个单词进行查询，即不对查査询语句做分词处理。

```json
GET test_search_index/_search
{
  "profile": "true", 
  "query": {
    "term": {
      "username": "alfred"
    }
  }
}
GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "terms": {
      "username": [
        "alfred",
        "way"
      ]
    }
  }
}
```

Range query范围查询主要针对数值和日期类型。

gte：大于等于；gt：大于；

lte：小于等于；lt：小于

Range Query -Date Math针对曰期提供的一种更友好地计算方式，格式如下：now-1d

now：基准时间，也可以是具体的时间，如2019-10-01，使用具体时间要用||隔离。

1d：+1h加一个小时；-d减一个小时；/d将时间设入到天。

y- years；M-months；w-weeks；d -days；h-hours；m- minutes；s- seconds

假设now为2018-01-02 12:00:00，那么如下的计算结果实际为：

| 计算公式            | 实际结果            |
| ------------------- | ------------------- |
| now+1h              | 2018-01-02 13:00:00 |
| now-1h              | 2018-01-02 11:00:00 |
| now-1h/d            | 2018-01-02 00:00:00 |
| 2016-01-01\|\|+1M/d | 2016-02-01 00:00:00 |

```json
GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "range": {
      "age": {
        "gte": 10,
        "lt": 20
      }
    }
  }
}

GET test_search_index/_search
{
  "profile": "true",
  "query": {
    "range": {
      "birth": {
        "gte": "now-2000y"
      }
    }
  }
}
```

### 2.2.2 复合查询

如bool查询等，包含一个或多个字段类查询或者复合查询语句。复合查询（Compound queries）是指包含字段类査询或复合查询的类型，主要包括以下几类：constant_score_query、bool_query、dis_max_query、function_score_query、boosting_query。Constant Score Query该查询将其内部的査询结果文档得分都设定为1或者 boost的值，多用于结合bool查询实现自定义得分。

#### 2.2.2.1 Constant_score

```json
GET test_search_index/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "match": {
          "username": "alfred"
        }
      }
    }
  }
}
```

filter只能有一个。

#### 2.2.2.2 Bool Query布尔查询

Bool Query布尔查询由一个或多个布尔子句组成，主要包含如下4个：filter只过滤符合条件的文档，不计算相关性得分、must必须符合must的所有条件，影响相关性得分、must_not文档必须不符合must_not中的所有条件，不影响相关性得分、should文档中符合should的条件，会影响相关性得分。

| 子句构成 |                                                              |
| -------- | ------------------------------------------------------------ |
| filter   | 只过滤**符合条件**的文档，不计算相关性得分，一组条件！！！   |
| must     | 文档必须符合must中的**所有条件**，会影响相关性得分           |
| must_not | 文档必须不符合must_not中的**所有条件**，不影响相关性得分     |
| should   | 文档中**符合should的条件**，会影响相关性得分（并列查询可以使用！！！） |

```json
GET test_search_index/_search
{
  "query": {
    "bool": {
      "filter": [
        {
          "term": {
            "username": "alfred"
          }
        }
      ]
    }
  }
}

GET test_search_index/_search
{
  "query": {
    "constant_score": {
      "filter": {
        "term": {
          "username.keyword": "郭冬冬"
        }
      },
      "boost": 1.2
    }
  }
}

GET test_search_index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "match": {
            "username": "alfred"
          }
        },
        {
          "match": {
            "job": "java"
          }
        }
      ]
    }
  }
}
```

Should使用分两种情况：bool查询中只包含 should，不包含must查询；bool查询中同时包含should和must查询。只包含 should时，文档必须满足**至少一个条件**，minimum_should_match可以控制满足条件的个数或者百分比。

```json
GET test_search_index/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "term": {
            "username": {
              "value": "冬"
            }
          }
        },
        {
          "term": {
            "username": {
              "value": "郭"
            }
          }
        }
      ],
      "minimum_should_match": 2
    }
  }
}
```

同时包含should和must时，文档不必满足should中的条件，但是如果满足条件，会增加相关性得分。

```json
GET test_search_index/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "username": {
              "value": "冬"
            }
          }
        }
      ],
      "should": [
        {
          "term": {
            "username": {
              "value": "东"
            }
          }
        }
      ]
    }
  }
}
```

Query Context VS Filter Context当一个查询语句位于 Query或者Filter上下文时，ES执行的结果会不同，对比如下。

| 上下文类型 | 执行类型                                                 | 使用方式                                           |
| ---------- | -------------------------------------------------------- | -------------------------------------------------- |
| Query      | 查找与查询语句最匹配的文档，对所有文档进行相关性计算排序 | query、bool中的must和should                        |
| Filter     | 查找与查询语句最匹配的文档                               | bool中的filter和must_not；constant_scort中的filter |

# 3 Count APl

获取符合条件的文档数， endpoint为_count

```json
GET test_search_index/_count
{
  "query": {
    "bool": {
      "must": [
        {
          "term": {
            "username": "alfred"
          }
        }
      ]
    }
  }
}
```

# 4 Source Filtering

过滤返回结果中_source中的字段，主要有如下几种方式

```json
# 通过URI的形式
GET test_search_index/_search
GET test_search_index/_search?_source=username

# 通过指定_source为false
GET test_search_index/_search
{
  "_source": false
}

# 直接指定
GET test_search_index/_search
{
  "_source": ["username","age"]
}

# 模糊查询
GET test_search_index/_search
{
  "_source": {
    "includes": "*i*",
    "excludes": "birth"
  }
}
```

# 5 相关性算分

相关性算分是指文档与查询语句间的相关度，英文为relevance。

通过倒排索引可以获取与査询语句相匹配的文档列表，那么如何将最符合用户查询。

需求的文档放到前列呢？本质是一个排序问题，排序的依据是相关性算分。

Term Frequency（TF）词频，即单词在该文档中出现的次数。词频越高，相关度越高。

Document Frequency（DF）文档频率，即单词出现的文档数。

Inverse Document Frequency（IDF）逆向文档频率，与文栏频率相反，简单理解为1/DF，即单词出现的文档数越少，相关度越高。

Field-length Norm文档越短，相关性越高。

ES目前主要有两个相关性算分模型，如：**TF/DF**模型；**BM25模型**5X之后的默认模型。

可以通过explain参数来查看具体的计算方法。

但要注意ES的算分是按照shard进行的，即shard的分数计算是相互独立的，所以在使用explain的时候注意分片数，可以通过设置索引的分片数为1来避免这个问题。

TFDF模型是 Lucene的经典模型；BM25模型中BM指 Best match，25指迭代了25次才计算方法，是针对TFDF的一个优化。

```json
GET test_search_index/_search
{
  "explain": true,
  "query": {
    "match": {
      "username": "alfred"
    }
  }
}
```

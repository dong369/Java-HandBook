# 1 概念理解

## 1.1 基础查询

实现对es中存储的数据进行查询分析， endpoint为_search。

GET /_search

GET /my_index/_search

GET /my_index1,my_index2/_search

GET /my_index——*/_search

# 2 URI Search

## 2.1 语法操作

操作简便，方便通过命令行测试；**但仅包含部分查询语法**。

通过url query参数来实现搜索，常用参数如下：

q：是query的简称，指定查询的语句，语法为Query String Syntax。

df：是default fields的简称，q中不指定字段时**默认查询的字段**，如果不指定，ES会查询所有字段。

sort：排序

timeout：指定超时时间，默认不超时

from，size：用于分页

语法01：get /my_index/_search?q=username:alfred&sort=age:asc&from=1&sixe=10&timeout=1s

语法02：get /my_index/_search?q=alfred&df=username&sort=age:asc&from=1&sixe=10&timeout=1s

语义：查询username字段包含alfred的文档，结果按照age升序排序，返回第1-10的文档数据，如果超过一秒没有结束，则超时结束！

## 2.2 Query String Syntax

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

## 2.3 term与phrase

term是一个个的单词，phrase是一个词语是有顺序的！！！

alfred way等效于alfred OR way，满足其中一个就会返回。

"alfred way"词语查询，要求先后顺序。

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

## 2.4 泛查询

泛查询是不指定字段的查询，alfred等效于在**所有字段**去匹配该term。前提是没有指定**df属**性！！！

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

## 2.5 指定字段

name:alfred

```json
GET test_search_index/_search?q=username:郭冬
GET test_search_index/_search?q=username:"郭冬"
GET test_search_index/_search?q=username:(郭冬)
# 查看ES执行的语句
GET test_search_index/_search?q=username:"郭冬"
{
  "profile": true
}
```

## 2.6 分组

Group分组设定，**使用括号指定匹配**的规则：(A OR B) AND C；A:(B OR C)；A:(B C D)

```json
GET test_search_index/_search?q=username:alfred AND way
GET test_search_index/_search?q=username:(alfred AND way)
{
  "profile": "true"
}
```

## 2.7 布尔操作符

AND（&&）， OR（||）， NOT（！）
name:(tom NOT lee)
**注意大写，不能小写**
+、-分别对应must和 must_not，+在url中会被**解析为空格**，要使用encode后的结果才可以，为%2B。
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

## 2.8 范围查询

1. 范围查询，支持数值和日期，区间写法，闭区间用[]，开区间用{}
   age：[1 TO 10]意为1<=age<=10
   age：[1 TO 10}意为1<=age<10
   age：[1 TO ]意为age>=1
   age：[* TO 10]意为age<=10
2. 算数符号写法
   age:>=1
   age:(>=1&&<=10) 或者age:(+>=1+<=10)

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

## 2.9 通配符查询

`?`代表1个字符，`*`代表0或多个字符
name:t?m
name:tom*
name:t*m
通配符匹配执行**效率低**，且占用较多內存，不建议使用，如无特殊需求，不要将?、/`**`放在最前面。

```json
GET test_search_index/_search?q=username:alf*
```

## 2.10 正则表达式

name:/[mb]oat/

```json
GET test_search_index/_search?q=username:/[a]?l.*/
```

## 2.11 模糊匹配

**词的模糊匹配**fuzzy query，name:roam~1。匹配与roam差1个character的词，比如foam roams等

```json
GET test_search_index/_search?q=username:升
GET test_search_index/_search?q=username:升~1

GET test_search_index/_search?q=username:alfed
GET test_search_index/_search?q=username:alfed~1
GET test_search_index/_search?q=username:alfd~2
```

## 2.12 近似度匹配

**词语近似度查询**proximity search，"fox quick"~5。以**term为单位**进行差异比较，比如"quick fox" "quick brown fox"都会被匹配。实际应用对用户输入词的一个纠错！！！

```json
GET test_search_index/_search?q=username:"郭升"
GET test_search_index/_search?q=username:"郭升"~1

GET test_search_index/_search?q=job:"java engineer"
GET test_search_index/_search?q=job:"java engineer"~1
GET test_search_index/_search?q=job:"java engineer"~2
```

# 3 Body Search

ES提供的**完备查询语法**Query DSL（Domain Specific Language）

语法：get /my_index/_search    {"query":{"term":{"user":"alfred"}}}

基于JSON定义的查询语言，主要包含如下两种类型

## 3.1 字段类查询

term（词查询）、terms（词查询）、range（针对词查询）。

match（全文检索）、match_phrase（全文检索查询）等，只针对某一个字段进行查询。

字段类查询主要包括以下两类。

### 3.1.1 全文匹配

针对text类型的字段进行全文检索，会对查询语句先进行分词处理，如match，match_phrase（词语的查询）等query类型。

Simple Query String Query类似query String，但是会忽略错误的查询语法，并且仅支持部分査询语法；

其常用的逻辑符号如下，不能使用AND、OR、NOT等关键词：+代指AND、|代指OR、-代指NOT。

```json
# 对字段作全文检索，最基本和常用的查询类型，返回文档中有alfred或way的
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

# 通过minimum_should_match参数可以控制需要匹配的单词数，包含里面至少3个词
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

# Match Phrase Query，对字段作检索，有顺序要求，通过slop参数可以控制单词间的间隔（中间和后面偏移量）
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

# Query String Query，类似于URI Search中的q参数查询，
GET test_search_index/_search
{
  "query": {
    "query_string": {
      // 指定字段中查询
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
# 其常用的逻辑符号不能是AND、OR、NOT
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

### 3.1.2 单词匹配

不会对查询语句做分词处理，直接去匹配字段的倒排索引，如term，terms，range等query类型。

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

gte：大于等于、gt：大于；lte：小于等于、lt：小于

Range Query - Date Math针对曰期提供的一种更友好地计算方式，格式如下：now-1d

now：基准时间，也可以是具体的时间，如2019-10-01，使用具体时间要用||隔离。

1d：+1h加一个小时；-d减一个小时；/d将时间设入到天。

y- years；M-months；w-weeks；d -days；h-hours；m- minutes；s- seconds

假设now为2018-01-02 12:00:00，那么如下的计算结果实际为：

| 计算公式                          | 实际结果            |
| --------------------------------- | ------------------- |
| now+1h                            | 2018-01-02 13:00:00 |
| now-1h                            | 2018-01-02 11:00:00 |
| now-1h/d        /d是舍入到        | 2018-01-02 00:00:00 |
| 2016-01-01\|\|+1M/d    /d是舍入到 | 2016-02-01 00:00:00 |

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

## 3.2 复合查询

如bool查询等，包含**一个或多个字段类查询**或者**复合查询**语句。复合查询（Compound queries）是指包含字段类査询或复合查询的类型，主要包括以下几类：constant_score_query、bool_query、dis_max_query、function_score_query、boosting_query。Constant Score Query该查询将其内部的査询结果文档得分都设定为1或者 boost的值，多用于结合bool查询实现自定义得分。

### 3.2.1 Constant_score

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

### 3.2.2 Bool Query布尔查询

Bool Query布尔查询由一个或多个**布尔子句组成**，主要包含如下4个：filter只过滤符合条件的文档，不计算相关性得分、must必须符合must的所有条件，影响相关性得分、must_not文档必须不符合must_not中的所有条件，不影响相关性得分、should文档中符合should的条件，会影响相关性得分。

| 子句构成 | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| filter   | 只过滤**符合条件**的文档，不计算相关性得分，一组条件！！！   |
| must     | 文档必须符合must中的**所有条件**，会影响相关性得分           |
| must_not | 文档必须不符合must_not中的**所有条件**，不影响相关性得分     |
| should   | 文档中**符合should的条件**，会影响相关性得分（并列查询可以使用！！！） |

#### 3.2.2.1 filter

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

#### 3.2.2.2  should

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

#### 3.2.2.3 must

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

Query Context VS Filter Context当一个查询语句位于Query或者Filter上下文时，ES执行的结果会不同，对比如下。

#### 3.2.2.4 must_not

文档必须不符合must_not中的**所有条件**，不影响相关性得分。

```properties

```

## 3.3 query和filter对比

query关注点：此文档与此查询子句的**匹配程度**如何？filter关注点：此文档和查询子句**是否匹配**吗？

| 上下文类型 | 执行类型                                                 | 使用方式                                           |
| ---------- | -------------------------------------------------------- | -------------------------------------------------- |
| Query      | 查找与查询语句最匹配的文档，对所有文档进行相关性计算排序 | query、bool中的must和should                        |
| Filter     | 查找与查询语句最匹配的文档                               | bool中的filter和must_not；constant_scort中的filter |

# 4 Exists

针对 ES 5.2.2 之前版本（如 ES 2.3.4），查询不包含某字段（无 content 字段）的数据， 支持使用 missing 或者 exists；针对 ES 5.2.2 以上版本，查询不包含某字段（无 content 字段）的数据，废弃了missing。

## 4.1 字段为null空值

```properties
# subject字段不存在（字段值是：null or []）
GET /prod_search_info/_search
{
  "query": {
    "bool": {
      "must_not": {
        "exists": {
          "field": "subject"
        }
      }
    }
  }
}

# MySQL中的查询
select eid,ent_name from ent_search where enttype_code is NULL;

# ES中的查询
GET /prod_search_info/_search?_source=nestedStudentInfos
{
  "query": {
    "bool": {
      "must_not": [
        {
          "nested": {
            "path": "nestedStudentInfos",
            "query": {
              "exists": {
                "field": "nestedStudentInfos"
              }
            }
          }
        }
      ]
    }
  }
}

GET /prod_search_info/_search?_source=nestedStudentInfos
{
  "query": {
    "bool": {
      "must_not": [
        {
          "nested": {
            "path": "nestedStudentInfos",
            "query": {
              "bool": {
                "must": [
                  {
                    "exists": {
                      "field": "nestedStudentInfos"
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}
```

## 4.2 字段为非null空

```properties
# subject字段存在的（字段值不是：null or []）
GET /prod_search_info/_search
{
  "query": {
    "exists": {
      "field": "subject"
    }
  }
}

# MySQL中的查询
select eid,ent_name from ent_search where enttype_code is NOT NULL;

# ES中的查询
GET /prod_search_info/_search?_source=nestedStudentInfos&size=10
{
  "query": {
    "nested": {
      "path": "nestedStudentInfos",
      "query": {
        "exists": {
          "field": "nestedStudentInfos"
        }
      }
    }
  }
}

GET /prod_search_info/_search?_source=nestedStudentInfos
{
  "query": {
    "bool": {
      "must": [
        {
          "nested": {
            "path": "nestedStudentInfos",
            "query": {
              "bool": {
                "must": [
                  {
                    "exists": {
                      "field": "nestedStudentInfos"
                    }
                  }
                ]
              }
            }
          }
        }
      ]
    }
  }
}
```

# 5 Distinct

## 5.1 collapse

```properties
# MySQL数据库
SELECT DISTINCT user_id FROM table WHERE user_id_type = 3;

# ES
GET /search_model/_search
{
  "query": {
    "term": {
      "user_id_type": 3
    }
  },
  "collapse": {
    "field": "user_id"
  }
}
```

## 5.2 cardinality

cartinality metric对每个bucket中的指定的field进行去重，取去重后的count，就是count(distinct)

```properties
# MySQL中count + distinct
SELECT COUNT(DISTINCT(user_id)) FROM table WHERE user_id_type = 3;

# ES
GET /search_model/_search
{
  "query": {
    "term": {
      "user_id_type": 3
    }
  },
  "aggs": {
    "count": {
      "cardinality": {
        "field": "user_id"
      }
    }
  }
}

GET /prod_search_info/_search?size=0
{
  "aggs": {
    "aa": {
      "cardinality": {
        "field": "teacherName"
      }
    }
  }
}

# 每月销售品牌数量统计，MySQL中count+group by
SELECT COUNT(DISTINCT(user_id)) FROM table GROUP BY user_id_type;

# ES
GET /tvs/sales/_search
{
  "aggs": {
    "months": {
      "date_histogram": {
        "field": "sold_date",
        "interval": "month"
      },
      "aggs": {
        "distinct_brand": {
          "cardinality": {
            "field": "brand"
          }
        }
      }
    }
  }
}
```

# 6 Count APl

## 6.1 文档数

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

## 6.2 nested数目

value_count就是单纯的计数

```properties
# value_count计数时没有改字段的不会计（""、null会计），而nested是会计的
GET /prod_search_info/_search?size=0
{
  "query": {
    "term": {
      "teacherName": {
        "value": "教授01"
      }
    }
  },
  "aggs": {
    "aa": {
      "nested": {
        "path": "nestedStudentInfos"
      },
      "aggs": {
        "bb": {
          "value_count": {
            "field": "nestedStudentInfos.studentName"
          }
        }
      }
    }
  }
}

# 过滤nested下面的字段
GET /prod_search_info/_search?size=0
{
  "aggs": {
    "aa": {
      "nested": {
        "path": "nestedStudentInfos"
      },
      "aggs": {
        "bb_filter": {
          "filter": {
            "term": {
              "nestedStudentInfos.studentName": "aa"
            }
          }
        },
        "bb": {
          "value_count": {
            "field": "nestedStudentInfos.studentName"
          }
        }
      }
    }
  }
}
```

# 7 Source过滤

Source Filtering过滤返回结果中_source中的字段，主要有如下几种方式

## 7.1 URI形式

```json
# 通过URI的形式
GET test_search_index/_search
GET test_search_index/_search?_source=username
```

## 7.2 禁用

```json
# 通过指定_source为false
GET test_search_index/_search
{
  "_source": false
}
```

## 7.3 指定

```json
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


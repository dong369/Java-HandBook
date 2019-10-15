# 1 概念理解

## 1.1 Mapping

类似数据库中的表结构定义，主要作用如下：

定义index下的字段名（ Field Name）；

定义字段的类型，比如数值型、字符串型、布尔型等；

定义倒排索引相关的配置，比如是否索引、记录position等。

语法：get http://211.144.5.80:30136/school/_mapping

# 2 Mapping操作

## 2.1 自定义mapping

Mapping中的字段类型一旦设定后，禁止直接修改，原因：Lucene实现的倒排索引生成后不允许修改。重新建立新的索引，然后做 reindex操作。

```json
{
    "mappings": {
        "properties": {
            "name": {
                "type": "keyword"
            },
            "title": {
                "type": "text"
            },
            "age": {
                "type": "integer"
            }
        }
    }
}
```

## 2.2 新增字段

通过dynamic参数来控制字段的新增

true（默认）允许自动新增字段

false不允许自动新增字段，但是文档可以正常写入，但无法对字段进行査询等操作

strict文档不能写入，报错

```json
PUT test
{
    "mappings": {
        "dynamic": false,
        "properties": {
            "name": {
                "type": "keyword"
            },
            "title": {
                "type": "text"
            },
            "age": {
                "type": "integer"
            }
        }
    }
}
```

## 2.3 copy_to

将该字段的值复制到目标字段，实现类似_all的作用，不会出现在source中，只用来搜索。

```json
{
    "mappings": {
        "properties": {
            "first_name": {
                "type": "keyword",
                "copy_to": "full_name"
            },
            "last_name": {
                "type": "keyword",
                "copy_to": "full_name"
            },
            "title": {
                "type": "text"
            },
            "age": {
                "type": "integer"
            },
            "full_name": {
                "type": "text"
            }
        }
    }
}
```

## 2.4 index

控制当前字段是否索引，默认为true，即记录索引，false不记录，即不可搜索。

```json
{
    "mappings": {
        "properties": {
            "first_name": {
                "type": "keyword",
                // 设置为false
                "index": false
            },
            "last_name": {
                "type": "keyword",
                "copy_to": "full_name"
            },
            "title": {
                "type": "text"
            },
            "age": {
                "type": "integer"
            },
            "full_name": {
                "type": "text"
            }
        }
    }
}
```

## 2.5 index_options

用于控制倒排索引记录的内容，有如下4种配置：

​	docs只记录 doc id

​	freqs记录doc id和 term frequencies

​	positions记录doc id、 term frequencies和 term position

​	offsets记录doc id、 term frequencies、 term position和 character offsets

text类型默认配置为positions，其他默认为docs

记录内容越多，占用空间越大

## 2.6 null_value

当字段遇到null值时的处理策略，默认为null，即空值，此时ES会忽略该值。可以通过设定该值设定字段的默认值。

# 3 数据类型

## 3.1 核心数据类型

​	字符串型text、 keyword
​	数值型long、 Integer、 short、byte、 double、 float、 half float、 scaled float
​	日期类型date
​	布尔类型 boolean
​	二进制类型 binary
​	范围类型 Integer_range、 float_range、 long_range、 double-range、 date_range

## 3.2 复杂数据类型

​	数组类型 array

​	对象类型 object

​	嵌套类型 nested object

​	地理位置

## 3.3 专用数据类型

​	IP

## 3.4 多字段特征

多字段特性multi-fields
允许对同一个字段采用不同的配置，比如分词，常见例子如对人名实现拼音搜索，只需要在人名中新增一个子字段为pinyin即可。

```json
PUT school
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text",
        "fields": {
          "pinyin": {
            "type": "text",
            "analyzer": "pinyin"
          }
        }
      }
    }
  }
}
```

# 4 Dynamic Mapping

## 4.1 自动识别字段类型

ES可以自动识别文档字段类型，是依靠JSON文档的字段类型来实现自动识别字段类型，从而降低用户使用成本，既可以直接向ES中写入数据，不需要像MySQL一样必须先创建表结构。

| JSON类型 | ES类型                                                       |
| -------- | ------------------------------------------------------------ |
| null     | 忽略                                                         |
| boolean  | boolean                                                      |
| 浮点类型 | float                                                        |
| 整数     | long                                                         |
| object   | object                                                       |
| array    | 由第一个非null值的类型决定                                   |
| string   | 匹配是日期设为date；匹配数值设为float/long；设为text，并附带keyword子字段 |

## 4.2 日期自动识别

日期的自动识别可以自行配置曰期格式，以满足各种需求默认是[" strict_date_optional_time","yyyy/MM/dd HH:mm:ss Z ||yyyy/MM/dd Z"]
strict_ _date _optional_time是 iSo datetime的格式，完整格式类似下面：YYYY-MM-DDThh mm ssTZD （eg 1997-07-16T19： 20： 30+01： 00）
dynamic_date_formats可以自定义日期类型，date_detection可以关闭日期自动识别的机制。

## 4.3 字符串数字识别

字符串是数字时，默认不会自动识别为整型，因为字符串中出现数字是完全合理的，numeric_detection可以开启字符串中数字的自动识别。

# 5 Dynamic Templates

允许根据ES自动识别的数据类型、字段名等来动态设定字段类型，可以实现如下效果：
​	所有字符串类型都设定为 keyword类型，即默认不分词
​	所有以 message开头的字段都设定为text类型，即分词
​	所有以long开头的字段都设定为long类型
​	所有自动匹配为double类型的都设定为foat类型，以节省空间

匹配规则一般有如下几个参数：
​	match_mapping _type匹配es自动识别的字段类型，如 boolean， long， string等
​	match，unmatch匹配字段名
​	path_match，path_unmatch匹配路径

字符串默认使用 keyword类型
​	ES默认会为字符串设置为text类型，并增加个keyword的子字段

```json
# 字符串默认使用 keyword类型 put
{
    "settings": {
        // 数组，可以指定多个匹配规则
        "dynamic_templates": [
            {
                // template的名称
                "strings": {
                    // 匹配规则
                    "match_mapping_type": "string",
                    // 设置mapping信息
                    "mapping": {
                        "type": "keyword"
                    }
                }
            }
        ]
    }
}
# 以message开头的字段都设置为text类型 put 
{
    "settings": {
        // 数组，可以指定多个匹配规则
        "dynamic_templates": [
            {
                // template的名称
                "message_as_text": {
                    // 匹配规则
                    "match_mapping_type": "string",
                    "match": "message*",
                    // 设置mapping信息
                    "mapping": {
                        "type": "text"
                    }
                }
            },
            {
                // template的名称
                "strings_as_keywords": {
                    // 匹配规则
                    "match_mapping_type": "string",                    
                    // 设置mapping信息
                    "mapping": {
                        "type": "keyword"
                    }
                }
            }
        ]
    }
}
```

# 6 建议

自定义 Mapping的操作步骤如下：
1. 写入一条文档到es的临时索引中，获取es自动生成的 mapping
2. 修改步骤1得到的 mapping，自定义相关配置
3. 使用步骤2的 mapping创建实际所需索引

```json

```

# 7 索引模板

索引模板，英文为 Index Template，主要用于在新建索引时自动应用预先设定的配置，简化索引创建的操作步骤：

​	可以设定索引的配置和 mapping

​	可以有多个模板，根据 order设置， order大的覆盖小的配置

索引模板API，endpoint为_template

```json
# get _template
# get _template/test_template1
# delete _template/test_template1
# put _template/test_template1
{
    "index_patterns": ["te*","bar*"],
    "order": 0,
    "settings": {
        "number_of_shards": 1
    },
    "mappings": {
        "_source": {
            // 不记录原始数据
            "enabled": false
        },
        "properties": {
            "nama": {
                "type": "keyword"
            }
        }
    }
}
# put _template/test_template2
{
    "index_patterns": ["test*"],
    "order": 1,
    "settings": {
        "number_of_shards": 1
    },
    "mappings": {
        "_source": {
            "enabled": true
        }
    }
}
```
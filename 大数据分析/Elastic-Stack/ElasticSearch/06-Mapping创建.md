# 1 概念理解

## 1.1 Mapping

类似数据库中的表结构定义，主要作用如下：

定义index下的字段名（ Field Name）；

定义字段的类型，比如数值型、字符串型、布尔型等；

定义倒排索引相关的配置，比如是否索引、记录position等。

语法：get school/_mapping

# 2 Mapping操作

## 2.1 自定义mapping

Mapping中的字段类型一旦设定后，禁止直接修改，原因：Lucene实现的倒排索引生成后不允许修改。

重新建立新的索引，然后做**reindex操作**。

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

true允许自动新增字段（**默认**）；

false不允许自动新增字段，但是文档**可以正常写入**，但无法对字段进行査询等操作，推荐使用！！！

strict文档不能写入，**报错**。

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

将该字段的值复制到目标字段，实现类似all的作用，不会出现在_source中，只用来搜索。

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

控制当前字段是否索引，**默认为true**，即记录索引，**false不记录倒排索引，即不可搜索**。

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

​	-docs只记录 doc id

​	-freqs记录doc id和 term frequencies

​	-positions记录doc id、 term frequencies和 term position

​	-offsets记录doc id、 term frequencies、 term position和 character offsets

text类型默认配置为positions，其他默认为docs

记录内容越多，占用空间越大！！！

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
                "type": "text",
                "index_options":"offsets"
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

## 2.6 null_value

当字段遇到null值时的处理策略，默认为null，即空值，此时ES会忽略该值。可以通过设定该值设定字段的默认值。

```json
{
    "mappings": {
        "properties": {            
            "title": {
                "type": "text",
                "index_options":"offsets"
            },
            "full_name": {
                "type": "keyword",
                "null_value":"NULL"
            }
        }
    }
}
```

# 3 数据类型

## 3.1 核心数据类型

字符串型：text、 keyword

数值型：long、 Integer、 short、byte、 double、 float、 half_float、 scaled_float

日期类型：date

布尔类型：boolean

二进制类型：binary

范围类型：Integer_range、 float_range、 long_range、 double_range、 date_range

## 3.2 复杂数据类型

数组类型：array

对象类型：object

嵌套类型：nested object

## 3.3 专用数据类型

地理位置：geo_point、geo_shape

记录IP地址：IP

实现自动补全：completion

记录分词数：token_count

记录字符串：mumur3

父子索引join类型：percolator

别名类型：alias

## 3.4 多字段特征

多字段特性multi-fields，**允许对同一个字段采用不同的配置**，比如分词，常见例子如对人名实现拼音搜索，只需要在人名中新增一个子字段为pinyin即可。

```json
PUT my_index
{
  "mappings": {
    "properties": {
      "city": {
        "type": "text",
        "fields": {
          "raw": { 
            "type":  "keyword"
          }
        }
      }
    }
  }
}

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
| array    | **由第一个非null值**的类型决定                               |
| string   | 匹配是日期设为date（默认开启）；匹配数值设为float/long（默认关闭）；设为text，并附带keyword子字段 |

```json
PUT /test_index/doc/1
{
  "username":"alfred",
  "age":14,
  "birth":"1988-10-10",
  "married":false,
  "year":"18",
  "tags":["boy","fashion"],
  "money":100.1
}
```

## 4.2 日期自动识别

日期的自动识别可以自行配置曰期格式，以满足各种需求默认是[" strict_date_optional_time","yyyy/MM/dd HH:mm:ss Z ||yyyy/MM/dd Z"]

strict_date _optional_time是ISO datetime的格式，完整格式类似下面：YYYY-MM-DDThh:mm:ssTZD（eg 1997-07-16T19:20:30+01:00）

dynamic_date_formats：可以自定义日期类型。

date_detection：可以关闭日期自动识别的机制。

```json
PUT my_index
{
  "mappings": {
    "my_type": {
      "dynamic_date_formats": ["MM/dd/yyyy"]
    }
  }
}

PUT my_index/my_type/1
{
  "create_date": "09/25/2015"
}

PUT my_index/my_type/1
{
  "create_date": "2015-09-01"
}
```

## 4.3 字符串数字识别

字符串是数字时，默认不会自动识别为整型，因为字符串中出现数字是完全合理的，numeric_detection可以开启字符串中数字的自动识别。

```json
PUT my_index
{
  "mappings": {
    "my_type": {
      "numeric_detection": true
    }
  }
}

PUT my_index/_doc/1
{
  "my_float":   "1.0", 
  "my_integer": "1" 
}
```

# 5 Dynamic Templates

## 5.1 基础认识

允许根据ES自动识别的数据类型、字段名等来**动态设定字段类型**。

可以实现的效果，所有字符串类型都设定为keyword类型，即默认不分词；所有以 message开头的字段都设定为text类型，即分词；所有以long开头的字段都设定为long类型；所有自动匹配为double类型的都设定为foat类型，以节省空间。

## 5.2 匹配规则

匹配规则一般有如下几个参数：
match_mapping_type：匹配ES自动识别的字段类型，如 boolean， long， string等。

match、unmatch：匹配字段名。

path_match、path_unmatch：匹配路径。

字符串默认使用 keyword类型ES默认会为字符串设置为text类型，并增加个keyword的子字段。

```json
# 字符串默认使用keyword类型 put
PUT /test_index
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
PUT /test_index
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

# 6 创建Mapping

## 6.1 手动创建

```properties
PUT search_model
{
  "settings": {
    "number_of_shards": 3,
    "number_of_replicas": 1,
    "analysis": {
      "analyzer": {
        "pinyin_analyzer": {
          "tokenizer": "my_pinyin"
        }
      },
      "tokenizer": {
        "my_pinyin": {
          "type": "pinyin",
          "keep_separate_first_letter": false,
          "keep_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "lowercase": true,
          "remove_duplicated_term": true
        }
      }
    }
  },
  "mappings": {
    "dynamic": "false",
    "numeric_detection": true,
    "properties": {
      "hospitalId": {
        "type": "keyword"
      },
      "patientId": {
        "type": "keyword"
      },
      "desensitizeHospitalPatientId": {
        "type": "keyword"
      },
	  "hospitalName": {
		"type": "keyword"
	  },
	  "sex": {
		"type": "keyword"
	  },
	  "dateTimeOfBirth": {
		"type": "date",
		"format": "yyyy-MM-dd HH:mm:ss"
	  },
	  "messageRace": {
		"type": "text"
	  },
      "clinicInfo": {
        "type": "nested",
        "properties": {
          "messageDeptName": {
            "type": "text"
          },
          "dateTimeRegister": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss"
          },
          "dateTimeAdmit": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss"
          },
		  "dateTimeDischarge": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss"
          },
		  "messageOutcome": {
            "type": "text"
          },
		  "ageClinic": {
            "type": "long"
          }
		  }
        },
        "diagnosisInfo": {
          "type": "nested",
		  "properties": {
          "messageDiagnosisCode": {
            "type": "text"
          },
		  "messageDiagnosisDesc": {
            "type": "text"
          }
		  }
        },
        "medicationInfo": {
          "type": "nested",
		  "properties": {
          "messageDrugName": {
            "type": "text"
          },
		  "messageFrequencyText": {
            "type": "text"
          },
		  "messageRouteName": {
            "type": "text"
          }
		  }
        },
        "pacsReport": {
          "type": "nested",
		  "properties": {
		  "messageExamItemName": {
            "type": "text"
          },
          "dateTimeObservation": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss"
          },
          "messageImagingFindings": {
            "type": "text"
          },
          "messageImagingConclusion": {
            "type": "text"
          },
		  "messageExaminationMethod": {
            "type": "text"
          },
		  "messageBodypart": {
            "type": "text"
          }
		  }
        },
        "lisReport": {
          "type": "nested",
		  "properties": {
          "messageSpecimenSourceName": {
            "type": "text"
          },
          "messageObservationName": {
            "type": "text"
          },
          "messageObservationValue": {
            "type": "text"
          },
		  "observationValueNo": {
            "type": "float"
          },
		  "abnormalFlags": {
            "type": "keyword"
          }
        }
      }
    }
  }
}
```

## 6.2 自动生成

自定义Mapping的操作步骤如下：
1. 写入一条文档到es的临时索引中，获取es自动生成的mapping
2. 修改步骤1得到的mapping，自定义相关配置
3. 使用步骤2的mapping创建实际所需索引

```json
PUT my_product_index/_doc/1
{
  "uuid": "7874678976789",
  "hospital_id": "机构ID",
  "patient_id": "病人ID",
  "patient_name": "病人姓名",
  "sex": "性别",
  "date_time_of_birth": "2019-01-01 12:12:12",
  "marital_status": "婚姻状况",
  "race": "民族",
  "nationality": "国籍",
  "sn": "身份证号",
  "phone": "电话",
  "postcode": "邮编",
  "address": "住址",
  "company": "工作单位",
  "company_phone": "单位电话",
  "card_no": "就诊卡号",
  "mrn": "病案号",
  "message_contact_name": "联系人姓名",
  "contact_relationship": "联系人关系",
  "contact_phone": "联系人电话"
}
```

## 6.3 dynamic

使用dynamic的方式！！！

```json
PUT test_index
{
  "settings": {
    "number_of_shards": 1,
    "number_of_replicas": 0
  },
  "mappings": {
    "dynamic_templates": [
      {
        "strings": {
          "match_mapping_type": "string",
          "mapping": {
            "type": "keyword"
          }
        }
      }
    ],
    "properties": {
      "body_sent": {
        "properties": {
          "bytes": {
            "type": "long"
          }
        }
      },
      "url": {
        "type": "text"
      }
    }
  }
}
```

# 7 索引模板

## 7.1 基础认识

索引模板，英文为Index Template，主要用于在新建索引时自动应用预先设定的配置，简化索引创建的操作步骤：可以设定索引的配置和mapping；可以有多个模板，根据 order设置，order**大的覆盖小的配置索引模板**API，endpoint为_template。

```json
# get _template
# get _template/test_template1
# delete _template/test_template1
put _template/test_template1
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

## 7.2 模板创建

PUT /_template/prod_template

```json
{
  "index_patterns": "prod_*",
  "order": 0,
  "settings": {
    "index": {
      "number_of_shards": 3,
      "number_of_replicas": 1
    },
    "analysis": {
      "analyzer": {
        "pinyin_analyzer": {
          "tokenizer": "my_pinyin"
        }
      },
      "tokenizer": {
        "my_pinyin": {
          "type": "pinyin",
          "keep_separate_first_letter": false,
          "keep_full_pinyin": true,
          "keep_original": true,
          "limit_first_letter_length": 16,
          "lowercase": true,
          "remove_duplicated_term": true
        }
      }
    }
  },
  "mappings": {
    "dynamic": true,
    "numeric_detection": true,
    "dynamic_date_formats": [
      "yyyy-MM-dd HH:mm:ss",
      "yyyy-MM-dd"
    ],
    "dynamic_templates": [
      {
        "strings-pinyin-text": {
          "match_mapping_type": "string",
          "match": "pinyin*",
          "mapping": {
            "type": "keyword",
            "fields": {
              "pinyin": {
                "type": "text",
                "store": false,
                "term_vector": "with_offsets",
                "analyzer": "pinyin_analyzer",
                "boost": 10
              }
            }
          }
        }
      },
      {
        "strings-ik-text": {
          "match_mapping_type": "string",
          "match": "message*",
          "mapping": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_smart",
            "fields": {
              "keyword": {
                "type": "keyword",
                "ignore_above": 256
              }
            }
          }
        }
      },
      {
        "strings-ik-pinyin-text": {
          "match_mapping_type": "string",
          "match": "describe*",
          "mapping": {
            "type": "text",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_smart",
            "fields": {
              "pinyin": {
                "type": "text",
                "store": false,
                "term_vector": "with_offsets",
                "analyzer": "pinyin_analyzer",
                "boost": 10
              }
            }
          }
        }
      },
      {
        "strings-nested": {
          "match_mapping_type": "object",
          "match": "nested*",
          "mapping": {
            "type": "nested"
          }
        }
      },
      {
        "strings-suggest": {
          "match_mapping_type": "string",
          "match": "suggest*",
          "mapping": {
            "type": "completion",
            "analyzer": "ik_max_word",
            "search_analyzer": "ik_smart"
          }
        }
      },
      {
        "strings-keyword": {
          "match_mapping_type": "string",
          "mapping": {
            "type": "keyword"
          }
        }
      },
      {
        "strings-date": {
          "match_mapping_type": "string",
          "match": "dateTime*",
          "mapping": {
            "type": "date",
            "format": "yyyy-MM-dd HH:mm:ss||yyyy-MM-dd||epoch_millis"
          }
        }
      }
    ]
  }
}
```

## 7.3 关闭动态

```properties
PUT /prod_patient_info/_mapping
{
  "dynamic": false
}

PUT /prod_patient_info/_mapping
{
  "numeric_detection": false
}
```


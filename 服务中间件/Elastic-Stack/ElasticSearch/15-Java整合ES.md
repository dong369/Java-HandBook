# 1 使用方式

## 1.1 软件架构

SpringBoot + REST Client

## 1.2 项目介绍

SpringBoot整合ES的方式：TransportClient、Elasticsearch SQL、REST Client、Data-ES。

TransportClient即将弃用，所以这种方式不考虑

## 1.3 Data-ES

Spring提供的封装的方式，好像底层也是基于TransportClient，Elasticsearch7.0后的版本不怎么支持，SpringBoot的Spring Boot Data Elasticsearch Starter最高版本2.1.7.RELEASE下载的是Spring Data Elasticsearch的3.1.5版本对应的是Elasticsearch 6.4.3版本，Spring Data Elasticsearch最新版3.1.10对应的还是Elasticsearch 6.4.3版本，我安装的是最新的Elasticsearch 7.3版本所以也没办法使用。

## 1.4 Elasticsearch SQL

将Elasticsearch的Query DSL用SQL转换查询，早期有一个第三方的插件Elasticsearch-SQL，后来随着官方也开始做这方面，这个插件好像就没怎么更新了，有兴趣的可以查看：https://www.cnblogs.com/jajian/p/10053504.html

## 1.5 REST Client

官方推荐使用，所以我们采用这个方式，这个分为两个Low Level REST Client和High Level REST Client，Low Level REST Client是早期出的API比较简陋了，还需要自己去拼写Query DSL，High Level REST Client使用起来更好用，更符合面向对象的感觉，两个都使用下吧

# 2 基础架子搭建

## 2.1 依赖

```java
<dependency>
    <groupId>org.elasticsearch</groupId>
    <artifactId>elasticsearch</artifactId>
    <version>7.3.0</version>
</dependency>

<!-- Java Low Level REST Client -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-client</artifactId>
    <version>7.3.0</version>
</dependency>

<!-- Java High Level REST Client -->
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>7.3.2</version>
</dependency>
```

## 2.2 配置



## 2.3 开发技巧

先写DSL语句=>再写业务代码=>打印业务代码中的DSL=>进行DSL比对

## 2.4 测试



# 3 查询操作

## 3.1 基础查询

### matchAllQuery

matchAllQuery()方法用来匹配全部文档。

### matchQuery

不能写为matchQuery("name", "to*")

matchQuery("filedname","value") 匹配单个字段，匹配字段名为 filedname, 值为 value 的文档

<table border="1" cellspacing="0" cellpadding="0"><tbody><tr><td valign="top"><p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; QueryBuilder qb = QueryBuilders.matchQuery("title", "article");</p></td></tr></tbody></table>

### multiMatchQuery

多个字段匹配某一个值，搜索name 中或interest 中包含有music的文档（必须与 music 一致）。

QueryBuilder queryBuilder = QueryBuilders.multiMatchQuery("music", "name", "interest");

### wildcardQuery

模糊查询，? 匹配单个字符，* 匹配多个字符

**[java]** [view plain](https://blog.csdn.net/lom9357bye/article/details/52852533 "view plain") [copy](https://blog.csdn.net/lom9357bye/article/details/52852533 "copy")

WildcardQueryBuilder queryBuilder = QueryBuilders.wildcardQuery("name",  "*jack*");// 搜索名字中含有 jack 文档（name 中只要包含 jack 即可）

### commonTermQuery

一种略高级的查询，充分考虑了 stop-word 的低优先级，提高了查询精确性。

将 terms 分为了两种：more-importent（low-frequency） and less important（high-frequency）。

less-important 比如 stop-words，eg：the and。

QueryBuilder qb = QueryBuilders.commonTermsQuery("title","article");      

### termQuery(client)

```
* termQuery("key", obj) 完全匹配

```

```
* termsQuery("key", obj1, obj2..)   一次匹配多个值

```

    QueryBuilder qb = QueryBuilders.termQuery("title","article");
    QueryBuilder qb = QueryBuilders.termsQuery("title","article","relevence");

### testPrefixQuery

 前缀

参考网址：https://www.cnblogs.com/wenbronk/p/6432990.html

```
/**

```

```
     * 前缀查询

```

```
     */

```

```
    @Test

```

```
    public void testPrefixQuery() {

```

```
        QueryBuilder queryBuilder = QueryBuilders.matchQuery("user", "kimchy");

```

```
        searchFunction(queryBuilder);

```

```
    }

```

### rangeQuery

闭区间 QueryBuilderquery = QueryBuilders.rangeQuery("age").from(10).to(20); 

开区间 QueryBuilder query = QueryBuilders.rangeQuery("age").gt(10).lt(20);

QueryBuilder qb = QueryBuilders

                                   .rangeQuery("like")
    
                                    .gte(5)
    
                                   .lt(7);

//               QueryBuilderqb = QueryBuilders

//                                 .rangeQuery("like")

//                                 .from(5)

//                                  .to(7)

//                                  .includeLower(true)// 包含上届

//                                  .includeUpper(false);// 包含下届

### nested query

在关系查询中，存在一对多和多对一的关系。因为就会出现两种查询情况。

在解释查询关系之前，需要理解一下 Relationship Name，如文档中 contact 和 account 的关系  ，一个 Account 会有多个 contact，一个 Contact 也会有多个 Account，但是最终归结的关系为 Account 对 contact 的关系为一对多。也就是说 在 contact 上保存有对 account'的引用，这个引用的名称就是 RelationshipName（区别于 field name），类似于外键的名称。

下面介绍两种查询

1、多对一的查询。

     salesforce 中特有的__r 模式，直接关联到 parent 上，如 contact 上存有对 account 的引用，那么我可以直接关联出 account 上的相关字段。

**[sql]** [view plain](https://blog.csdn.net/gaofly89/article/details/44854927 "view plain") [copy](https://blog.csdn.net/gaofly89/article/details/44854927 "copy")

1. **select** id,**name** ,account.**name**,account.id **from** contact  

2、一对多的查询

     嵌入式查询 (nestedquery), 这种方式适合在父的一端查询相关子的记录。如：我想查找到负责这个 account 的全部 contact。

**[sql]** [view plain](https://blog.csdn.net/gaofly89/article/details/44854927 "view plain") [copy](https://blog.csdn.net/gaofly89/article/details/44854927 "copy")

1. **select** id,**name**,(**select** id,**name** **from** contacts)  from account  


查询结果如图：

这样就会关联出所以的 contact 数据，contact 部分的展示形式 json 串。**注意 contacts 不是对象名称，是 Relationshipname**

### other

QueryBuilder qb =QueryBuilders._existsQuery_("str");

        //QueryBuilder qb =QueryBuilders.prefixQuery("name", "prefix");
    
        //QueryBuilder qb =QueryBuilders.regexpQuery("user", "k.*y");
    
           正则表达式

<table border="1" cellspacing="0" cellpadding="0"><tbody><tr><td valign="top"><pre>/**
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; * 模糊查询
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; * 不能用通配符, 不知道干啥用
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; */
</pre><p align="left">//QueryBuilder <u>qb</u> = QueryBuilders.fuzzyQuery("name", "<u>kimzhy</u>");</p></td></tr></tbody></table>

        //QueryBuilder qb =QueryBuilders.typeQuery("my_type");

<table border="1" cellspacing="0" cellpadding="0"><tbody><tr><td valign="top"><pre>/**
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; * 只查询一个id的
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; * QueryBuilders.idsQuery(String...type).ids(Collection&lt;String&gt; ids)
</pre><pre>&nbsp;&nbsp;&nbsp;&nbsp; */
</pre><p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; //QueryBuilder <u>qb</u> = QueryBuilders.idsQuery("my_type","type2").addIds("1","2","5");</p></td></tr></tbody></table>
3.2 聚合查询 
--------------------

AggsQueryTest

### avgQuery

         publicstatic void avgQuery(Client client) {
    
                  SearchResponseres = null;
    
                  AvgBuilderagg = AggregationBuilders
    
                                   .avg("avg_num")
    
                                   .field("like");
    
                  res= client.prepareSearch("search_test")
    
                                   .setTypes("article")
    
                                   .setSearchType(SearchType.DFS_QUERY_THEN_FETCH)
    
                                   .addAggregation(agg)
    
                                   .setFrom(0)
    
                                   .setSize(10)
    
                                   .execute().actionGet();
    
                  System.out.println(res);
    
                  //on shutdown
    
                  client.close();
    
         }

### minQuery

         MinBuilderagg = AggregationBuilders
    
                                   .min("min_num")
    
                                   .field("like");

### maxQuery

                  MaxBuilderagg = AggregationBuilders
    
                                   .max("max_num")
    
                                   .field("like");

### valueCountQuery

// 统计个数，值计算聚合

                  SearchResponseres = null;
    
                  ExtendedStatsBuilderagg = AggregationBuilders
    
                                   .extendedStats("extended_stats_num")
    
                                   .field("like");

### extendedStatsQuery

// 统计聚合 (一堆)

返回聚合分析后所有指标，比 Stats 多三个统计结果：平方和、方差、标准差

<table border="0" cellspacing="0" cellpadding="0" width="800"><tbody><tr><td><p align="left">1</p><p align="left">2</p><p align="left">3</p><p align="left">4</p><p align="left">5</p></td><td><p align="left">{</p><p align="left">&nbsp;&nbsp;&nbsp;&nbsp;"aggs" : {</p><p align="left">&nbsp;&nbsp;&nbsp;&nbsp;"grades_stats" : {"extended_stats" : { "field" : "grade"} }</p><p align="left">&nbsp;&nbsp;&nbsp;&nbsp;}</p><p align="left">}</p></td></tr></tbody></table>

         ExtendedStatsBuilder agg =AggregationBuilders._extendedStats_("extended_stats_num").field("like");

### percentileQuery

                  PercentilesBuilderagg = AggregationBuilders
    
                                   .percentiles("percentile_num")
    
                                   .field("like")
    
                                   .percentiles(95,99,99.9);

### percentileRankQuery

// 百分比

                  PercentileRanksBuilderagg = AggregationBuilders
    
                                   .percentileRanks("percentile_rank_num")
    
                                   .field("like")
    
                                   .percentiles(3,5);

### rangeQuery

// 范围

AggregationBuilder agg =

                          AggregationBuilders
    
                                  .range("agg")
    
                                  .field("like")
    
                                  .addUnboundedTo(3)
    
                                  .addRange(3, 5)
    
                                  .addUnboundedFrom(5); 

### histogramQuery

// 柱状图聚合

### dateHistogramQuery

// `按日期间隔分组`

### 获取聚合里面的结果

```
TopHitsBuilder thb=  AggregationBuilders.topHits(
);

```

### 嵌套的聚合

```
NestedBuilder nb= AggregationBuilders.nested(
).path(
);

```

### 反转嵌套

```
AggregationBuilders.reverseNested(
).path(
);

```

### 二级分组的例子

上面这些基本就是常用的聚合查询了，在嵌套（nested）下面的子聚合查询就是嵌套查询了，除了嵌套查询，其他的聚合查询也可以无限级添加子查询

举个例子

```
SearchRequestBuilder 
 = client.prepareSearch(
).setTypes(
);

```

```
TermsBuilder 
=  AggregationBuilders.terms(
).field(
);

```

```
TermsBuilder 
=  AggregationBuilders.terms(
).field(
);

```

```
.subAggregation(
)

```

```
.addAggregation(
);

```

```
        Terms terms= 
.
().getAggregations().
(
);

```

```
(Terms.Bucket name_buk:terms.getBuckets()){

```

```
                         Terms terms_age= name_buk.getAggregations().
(
);

```

```
(Terms.Bucket age_buk:terms_age.getBuckets()){

```

```
                                  System.
.println(name_buk.getKey()+
+age_buk.getKey()+
+age_buk.getDocCount());

```

```
                         }

```

```
}

```

3.3 嵌套查询
------

### constantScoreQuery

```
QueryBuilder queryBuilder = QueryBuilders.constantScoreQuery(QueryBuilders.termQuery("name","kimchy")).boost(2.0f);
```

### booQuery

```
   /**

```

```
     * 组合查询

```

```
     * must(QueryBuilders) :   AND

```

```
     * mustNot(QueryBuilders): NOT

```

```
     * should:                  : OR

```

```
     */

```

    **publicstaticvoid** booQuery(Client client) {// 最有用的嵌套查询
    
        SearchResponse res = **null**;
    
        QueryBuilder qb =QueryBuilders._boolQuery_()
    
                .should(QueryBuilders._termQuery_("title", "02"))

//              .mustNot(QueryBuilders.termQuery("title","article"))

                .should(QueryBuilders._termQuery_("title", "relevance"));

//              .filter(QueryBuilders.termQuery("title","article"));

        res = client.prepareSearch("search_test").setTypes("article").setSearchType(SearchType.**_DFS_QUERY_THEN_FETCH_**)
    
                .setQuery(qb).setFrom(0).setSize(10).execute().actionGet();
    
        **for** (SearchHit hit : res.getHits().getHits()) {
    
            System.**_out_**.println(hit.getSourceAsString());
    
    }

### 经典案例

如果需要查询 (addr = Beijing) && (sex = false) && (10 < age< 20) 的 doc：

    public static QueryBuilder createQuery() {
    
        BoolQueryBuilder query = QueryBuilders.boolQuery();
    
        // addr = Beijing
    
        query.must(new QueryStringQueryBuilder("Beijing").field("addr"));
    
        // sex = falese
    
        query.must(new QueryStringQueryBuilder("false").field("sex"));
    
        // age ∈ (10,20)
    
        query.must(new RangeQueryBuilder("age").gt(10).lt(20));
    
        return query;
    
    }

返回结果：

{"pid":168,"age":16,"sex":false,"name":"Tom","addr":"Beijing"}

{"pid":276,"age":19,"sex":false,"name":"Bill","addr":"Beijing"}

{"pid":565,"age":16,"sex":false,"name":"Brown","addr":"Beijing"}

{"pid":73,"age":13,"sex":false,"name":"David","addr":"Beijing"}

### disMaxQuery

```
/**

```

```
     * disMax查询

```

```
     * 对子查询的结果做union, score沿用子查询score的最大值, 

```

```
     * 广泛用于muti-field查询

```

```
     */

```

```
    @Test

```

```
    public void testDisMaxQuery() {

```

```
        QueryBuilder queryBuilder = QueryBuilders.disMaxQuery()

```

```
            .add(QueryBuilders.termQuery("user", "kimch"))  // 查询条件

```

```
            .add(QueryBuilders.termQuery("message", "hello"))

```

```
            .boost(1.3f)

```

```
            .tieBreaker(0.7f);

```

```
        searchFunction(queryBuilder);

```

```
    }

```

4 本案例数据导入
=========

curl -XPUT'http://169.254.135.217:9200/search_test/' -d '{

   "settings" : {

     "index" : {
    
     "number_of_shards" : 3,
    
     "number_of_replicas" : 1
    
     }

   },

   "mappings" : {

       "article" : {
    
         "properties" : {
    
           "title" : {"type" : "string"},
    
           "body" : {"type" : "string"},
    
           "like" : {"type" : "long"},
    
           "publish_date" : {"type" : "date"}
    
         }
    
       }
    
    }

}'

curl -XGET'http://169.254.135.217:9200/search_test/_mapping?pretty'

curl -XGET'http://169.254.135.217:9200/search_test/_mapping/article?pretty'

curl -XHEAD -i'http://169.254.135.217:9200/search_test/article'

/search_test/article/1

{

 "title": "What's relevance?",

 "body": "atticle body of relevence:Term frequency/inversedocument frequency",

 "like": "1",

 "publish_date": "2016-03-24"

}

/search_test/article/2

{

"title": "article 02",

"body": "article 02 atticlebody of relevence:Term frequency/inverse document frequency",

"like": "2",

"publish_date":"2016-05-24"

}

/search_test/article/3

{

"title": "article 03",

"body": "article 03 atticlebody of relevence:Term frequency/inverse document frequency",

"like": "3",

"publish_date":"2016-07-24"

}

/search_test/article/4

{

"title": "article 04",

"body": "article 04 atticlebody of relevence:Term frequency/inverse document frequency",

"like": "4",

"publish_date":"2016-09-24"

}

/search_test/article/5

{

"title": "article 05",

"body": "article 04 atticlebody of relevence:Term frequency/inverse document frequency",

"like": "5",

"publish_date":"2016-11-24"

}

/search_test/article/6

{

"title": "Quick brownrabbits",

"body": "Brown rabbits arecommonly seen.",

"like": "6",

"publish_date":"2016-12-24"

}

/search_test/article/7

{

"title": "Keeping petshealthy",

"body": "My quick brown foxeats rabbits on a regular basis.",

"like": "7",

"publish_date":"2017-11-24"

}
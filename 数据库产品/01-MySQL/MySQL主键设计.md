先来一个测，看看主键长度对查询和插入性能的影响。 
两个表 
t_book 主键是 string 长度 36 字节 
t_student 主键是 bigint 8 字节

```sql
CREATE TABLE `t_book` (
  `book_id` varchar(36) NOT NULL,
  `book_desc` varchar(255) DEFAULT NULL,
  `book_name` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`book_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

CREATE TABLE `t_student` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `create_time` datetime DEFAULT NULL,
  `sname` varchar(255) DEFAULT NULL,
  `update_time` datetime DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1000001 DEFAULT CHARSET=utf8mb4;
```

通过程序每个表插入 100w 条数据 
t_book 花费的时间是 t_student 的十倍以上 
（已经有 100W 再通过程序每次插入 1，连续 100 次。 t_book 花费 35 分钟，t_student 花费 4 分钟）

再看看 查询 count(*) 的性能

> SELECT count(_) from t_book 
> query time 3.187s 
> SELECT count(_) from t_student 
> query time 0.274s

差距在 10 倍以上

数据是 200W 时候

> SELECT count(_) from t_book 
> query time 7.056s 
> SELECT count(_) from t_student 
> query time 0.556s

差距依然在 10 倍以上

### 插入速度慢原因

因为插入时候主键的生成策略用的是 uuid 
因为是无序的，所以每次索引生成时候需要重排序，浪费时间。

### 查询速度慢原因

查询 connt（*）时候，需要将索引读入到内存中。 
如果索引长度越长，同样多的索引需要存储的磁盘空间越大。磁盘 IO 也是系统中最大的瓶颈。导致整个性能下降。

所以考虑到性能，主键最好是 递增的并且占用存储空间小。

单表情况
----

可以是 long 型， 自增主键。

分表情况
----

需要一个 id 生成器。  
timestemp + 序列号 + 机器号 组成的 数字  
需要保证时间不能回滚，否则会出现重复的 id

[![](https://upload.jianshu.io/users/upload_avatars/8451293/b98c1e79-b449-49ec-aeb5-0c5baca94e2f.jpg?imageMogr2/auto-orient/strip|imageView2/1/w/120/h/120/format/webp)](https://www.jianshu.com/u/fcf9094885e3)

### 被以下专题收入，发现更多相似内容

### 推荐阅读[更多精彩内容](https://www.jianshu.com/)

*   1. 在 github 上搜索 tcc-transaction 开源框架 图片. png 2. 在本地下载源码程序 图片. png...
    
*   实现方式 在 spring 中使用 websocket 有多种方式： 在 spring mvc 中使用，这种不做介绍 在 web...
    
*   项目概要： SpringMVC-01 项目：整合 Spring+SpringMVC+MyBatis ，实现增删查改翻页...
    
*   一、Spring Boot 基本概念 Spring 的出现使得 JAVA EE 开发更加容易，但是配置起来却相对比较麻烦...
    
*   参考资料：官方参考文档 Spring Boot 入门之基础篇（一) 1. 简单介绍 Spring Boot 可以轻松创建...
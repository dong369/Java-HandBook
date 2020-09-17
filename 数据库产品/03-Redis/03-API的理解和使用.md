# 1 使用和理解

Redis APIE的使用和理解
通用命令
列表类型
字符串类型
集合类型
哈希类型
有序集合类型

## 1.1 通用命令

keys：遍历所有key。例如：keys *

dbsize：计算key的总数

exists key：是否存在指定key。存在返回1，不存在返回0

del key：删除指定key。成功返回1；不成功返回0

expire key secondes：指定key的过期时间

ttl：查看过期时间，-1代表key存在，没有过期时间；-2是key存在

persist：修改过期时间为不过期

type key：查看key的类型

## 1.2 常用数据类型

string、hash、list、set、zset

进行空间换时间的方式、时间换空间

## 1.2 计数

incr key：加一

decr key：减一

incrby key k：加指定值

decr key k：减指定值



## 1.3 单线程架构



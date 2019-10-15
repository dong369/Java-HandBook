# 1 使用和理解

## 1.1 通用命令

keys：遍历所有key。例如：keys *

dbsize：计算key的总数。

exists key：是否存在指定key。存在返回1；不存在返回0。

del key：删除指定key。成功返回1；不成功返回0。

expire（ttl、persist） key secondes：指定key的过期时间。（-1代表key存在，没有过期时间；-2是key存在）

type key：查看key的类型。

## 1.2 数据结构内部编码



## 1.3 单线程架构



# 2 字符串类型



# 3 集合类型

◆哈希类型
◆有序集合类型
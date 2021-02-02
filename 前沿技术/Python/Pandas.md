# 1 基本认识

我们并不是不愿意学习新的知识，只是在学习之前我们更想知道学习他们能够帮助我们解决什么问题。

那么问题来了：numpy已经能够帮助我们处理数据，能够结合matplotlib解决我们数据分析的问题，那么pandas学习的目的在什么地方呢？

numpy能够帮我们处理处理数值型数据，但是这还不够

很多时候，我们的数据除了数值之外，还有字符串，还有时间序列等

比如：我们通过爬虫获取到了存储在数据库中的数据

比如：之前youtube的例子中除了数值之外还有国家的信息，视频的分类(tag)信息，标题信息等

所以，numpy能够帮助我们处理数值，但是pandas除了处理数值之外(基于numpy)，还能够帮助我们处理其他类型的数据。

## 1.1 安装



## 1.2 导入



## 1.3 入门



## 1.4 问题



# 2 数据类型

## 2.1 Series

Series 一维，带标签数组

```python
import string

import pandas as pd
import numpy as np

# 创建series的两种方式
ser_dict = {"name": "guo", "age": 12, "tel": 1888}
data_rang = pd.Series(np.arange(10), index=list(string.ascii_uppercase[:10]), dtype=int)
data_dict = pd.Series(ser_dict)
print(data_dict)
print(data_rang)

# 取值
print(data_dict[[0, 1, 2]])
print(data_dict[["age", "tel"]])
print(data_dict[0:4])
print(data_dict[0:4:2])
print(data_rang[data_rang > 6])

# 索引、值
print(data_rang.index)
print(data_rang.values)

```

## 2.2 DataFrame

DataFrame 二维，Series容器

# 3 Series操作

Series 一维，带标签数组

## 3.1 创建

```python
# 创建series的两种方式
ser_dict = {"name": "guo", "age": 12, "tel": 1888}
ser_rang = pd.Series(np.arange(10), index=list(string.ascii_uppercase[:10]), dtype=int)
data_dict = pd.Series(ser_dict)
print(data_dict)
print(ser_rang)

```

## 3.2 类型

```python
print(type(data_dict))
print(type(ser_rang))
print(data_dict.dtype)
print(ser_rang.dtype)

```



## 3.3 取值

由于series是一维数组，所以不涉及取多列操作。

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：guodd
@File    ：maintest.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import pandas as pd

ds_dict = pd.Series({"name": "guo", "age": 23, "tel": 1001})
ds_range = pd.Series(range(2, 9))
print(ds_dict.shape[0])
# 取值
print(ds_dict[[0, 1, 2]])
print(ds_dict[["age", "tel"]])
print(ds_dict[0:4])
print(ds_dict[0:4:2])
print(ds_range[ds_range > 6])
# print(ds_range[[1], [2]])  # 错误写法

```



## 3.4 索引/值

```python
print(ser_rang.index)
print(ser_rang.values)
```



## 3.5 排序

```python
# 索引排序
print(ser_rang.sort_index(ascending=False))
# 值排序
print(ser_rang.sort_values(ascending=False))
```



# 4 DataFrame操作

对象既有行索引，又有列索引

行索引，DataFrame表明不同行，横向索引，叫index，0轴，axis=0

列索引，表名不同列，纵向索引，叫columns，1轴，axis=1

## 4.1 创建



## 4.2 基本属性

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：pd02.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import pandas as pd

file_data = pd.read_csv("../csv/video/GBvideos.csv")
# 行数、列数
print(file_data.shape)
# 列数据类型
print(file_data.dtypes)
# 数据维度
print(file_data.ndim)
# 行索引
print(file_data.index)
# 列索引
print(file_data.columns)
# 值信息
print(file_data.values)

```



## 4.3 数据整体

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：pd02.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import pandas as pd

file_data = pd.read_csv("../csv/video/GBvideos.csv")
# 显示头部信息，默认五行
print(file_data.head())
# 显示尾部信息，默认后五行
print(file_data.tail())
# 行数、列数、列索引、列非空个数、列类型、内存情况
print(file_data.info())
# 结果值：计数、均值、标准差等
print(file_data.describe())

```

## 4.4 取值

1.通过**标签**索引行数据

2.dfdf.loc.iloc 通过**位置**获取行数据

```python
import string
import pandas as pd
import numpy as np

df_dict = pd.DataFrame({"name": ["guo", "zhang"], "age": [23, np.nan], "tel": [1001, 12]},
                       index=list(string.ascii_lowercase[:2]),
                       columns=list(string.ascii_uppercase[:3]))
df_range = pd.DataFrame(np.arange(24).reshape(3, 8),
                        index=list(string.ascii_lowercase[0:3]),
                        columns=list(string.ascii_uppercase[:8]))

# 数据格式
print(df_dict)
print(df_range)

# 形状
print(df_range.shape[0])
print(df_range.shape[1])

# 取连续行
print(df_range[0:2])
# print(df_range[1, 2]) 错误写法
print(df_range.loc[["a"], ["A"]])
# 不连续行
print(df_range.iloc[[0, 2], :])

# 指定列
print(df_range["A"])
# 不连续列
print(df_range[["A", "B"]])
# 连续列
print(df_range.iloc[:, 0:3])

# 任意块
print(df_range.iloc[1:3, 2:5])

```



## 4.5 索引/值

```python
import string
import pandas as pd
import numpy as np

df_dict = pd.DataFrame({"name": ["guo", "zhang"], "age": [23, np.nan], "tel": [1001, 12]},
                       index=list(string.ascii_lowercase[:2]),
                       columns=list(string.ascii_uppercase[:3]))
df_range = pd.DataFrame(np.arange(24).reshape(3, 8),
                        index=list(string.ascii_lowercase[0:3]),
                        columns=list(string.ascii_uppercase[:8]))

print(df_range.index)
print(df_range.values)

```



## 4.6 排序



## 4.7 缺失处理

我们的数据缺失通常有两种情况：一种就是空，None等，在pandas是NaN(和np.nan一样)；另一种是我们让其为0。

## 4.8 布尔



## 4.9 数据合并



## 4.10 分组

join、merge

## 4.11 聚合

groupby

## 4.12 复合索引



# 5 时间序列

## 5.1 创建

```python
import pandas as pd

date_df = pd.date_range(start='20200101', end='20210101', freq="M")
print(date_df)

```

## 5.2 重采样

重采样：指的是将时间序列从一个频率转化为另一个频率进行处理的过程，将高频率数据转化为低频率数据为降采样，低频率转化为高频率为升采样。

pandas提供了一个resample的方法来帮助我们实现频率转化。



## 5.3 PeriodIndex

之前所学习的DatetimeIndex可以理解为时间戳

那么现在我们要学习的PeriodIndex可以理解为时间段



periods = pd.PeriodIndex(year=data["year"],month=data["month"],day=data["day"],hour=data["hour"],freq="H")

那么如果给这个时间段降采样呢？



# 5 列表推导式

## 5.1 基本格式

`[* for i in k]:  `*可以是一个函数，变量为i（也可以与i无关），k为一个可迭代对象，如列表。

1、一句代码输出一个1到5的立方

2、一句代码创建一个列表，包含10个60-100的随机整数

```python
import random

print([i ** 3 for i in range(1, 6)])
print([random.randint(60, 100) for _ in range(10)])
```

## 5.2 for循环嵌套



## 5.3 筛选功能

```python
print([i for i in range(12, 16) if i != 13])
```

# 
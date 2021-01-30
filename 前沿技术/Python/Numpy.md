# 1 基本认识

Numpy是Python中科学计算的基础包。主要提供**多维数组对象**，各种派生对象（如掩码数组和矩阵），以及用于数组快速操作的各种API，有包含数学、逻辑、形状、排序、选择、输入输出、离散愽立叶变换、基本线性代数，基本统计运算和随机模拟。

## 1.1 安装

numpy.array(parameter)，接收Python中的列表、元组作为参数。

```python
pip install numpy
conda install numpy
```

## 1.2 导入

```python
import numpy as np
```

## 1.3 入门

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

array = np.array([1, 2, 3])
print(array)
print(type(array))

```

## 1.4 问题

1、什么是numpy

2、numpy基础

3、numpy常用方法

4、numpy统计方法

5、快速；方便；科学计算的基础库

# 2 数组对象

常见的数组基本到三维：页、行、列

## 2.1 N维数组

平时用的最多的是一维数组、二维数组。

## 2.2 创建方式

```python
# arr = np.array(1, 4) 错误写法
# 正确的三种方式
arr1 = np.array([2 3 4 5 6 7 8 9])
arr2 = np.array(range(2, 10))  # 包含2，不包含10
arr3 = np.arange(2, 10)  # 包含2，不包含10
print(arr1)
print(arr1.shape)

# 三维
aa = np.array([
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
])
```

## 2.3 轴

NumPy的主要对象是同构多维数组。它是一个元素表（通常是数字），所有类型都相同，由非负整数元组索引。在NumPy维度中称为轴。

二位数组

![image-20210124113432772](../../插图/image-20210124113432772.png)

三维数组

![image-20210124113413641](../../插图/image-20210124113413641.png)

# 3 基本操作

## 3.1 读取

1、CSV文件读取

转置，交换轴

```python
np.loadtxt(fname,dtype=np.float,delimiter=None,skiprows=0,usecols=None,unpack=False)
```

![image-20210125132917731](../../插图/image-20210125132917731.png)

2、自定义数据

```python
data_path = "../csv/video/GB_video_data_numbers.csv"
dn = np.loadtxt(data_path, delimiter=",", dtype=np.int)
print(dn[:, 2:])

```



3、数据库



4、其他

## 3.2 索引

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

arr = np.arange(24).reshape(3, 8)
print(arr[1])

```



## 3.3 切片

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

arr = np.arange(24).reshape(3, 8)
# 全部数据
print(arr)
print("*" * 100)

# x轴方向，行数
print(arr.shape[0])
# y轴方向，列数
print(arr.shape[1])
print("*" * 100)

# 点数据
print(arr[1, 3])
print("*" * 100)

# 单行
print(arr[1])
# 相邻多行
print(arr[1:3])
# 不相邻多行
print(arr[[0, 2]])
print("*" * 100)

# 单列
print(arr[:, 1])
# 相邻多列
print(arr[:, 1:4])
# 不相邻多列
print(arr[:, [1, 2]])

```

## 3.4 形状

1、基本使用

2、常用方法

flatten()

3、形状变换

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

arr_data = np.arange(24)
arr_one = arr_data.flatten()
arr_two = arr_data.reshape(3, 8)
arr_three = arr_data.reshape(2, 3, 4)
print(arr_one.tolist())
print(arr_two)
print(arr_three)

```



## 3.5 运算





## 3.6 赋值

修改行列的值，我们能够很容易的实现，但是如果条件更复杂呢？比如我们想要把t中小于10的数字替换为3

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

two_arr = np.arange(45).reshape(5, 9)
# 赋值
two_arr[1:3, 4:6] = 0
print(two_arr)

```



## 3.7 布尔替换

小于10的数字替换为0，把大于10的替换为10，应该怎么做？？

注意：会直接修改数组中的值，因为等价于赋值操作

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

two_arr = np.arange(45).reshape(5, 9)
# 大于6的替换成6
two_arr[two_arr > 6] = 6
print(two_arr)

```



## 3.8 三位替换

小于10的数字替换为0，把大于20的替换为20，应该怎么做？？

注意：不会直接修改数组中的值，区别布尔操作

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

two_arr = np.arange(45).reshape(5, 9)
# 大于6的替换成6
np.where(two_arr < 9, 0, 10)
print(two_arr)

```



## 3.9 裁剪替换

小于10的替换为10，大于18的替换为了18，但是nan没有被替换，那么nan是什么？

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

two_arr = np.arange(45).reshape(5, 9)
# 裁剪替换
print(two_arr.clip(9, 18))

```



## 3.10 转置

T、transpose()、swapaxes(1,0)

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

arr_data = np.arange(24).reshape(3, 8)
print(arr_data.T)
print(arr_data.transpose())
print(arr_data.swapaxes(1, 0))

```



## 3.11 Nan

1、nan(NAN,Nan)：not a number表示不是一个数字。

2、nan和任何值计算都为nan。

3、什么时候numpy中会出现nan

​      当我们读取本地的文件为float的时候，如果有缺失，就会出现nan

​      当做了一个不合适的计算的时候(比如无穷大(inf)减去无穷大)

## 3.12 Inf

1、inf(-inf,inf)：infinity,inf表示正无穷，-inf表示负无穷。

2、什么时候回出现inf包括（-inf，+inf）

​      比如一个数字除以0，（python中直接会报错，numpy中是一个inf或者-inf）

## 3.13 函数



## 3.14 拼接

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：py
@File    ：np02.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

# 文件读取
gb_data_path = "../csv/video/GB_video_data_numbers.csv"
us_data_path = "../csv/video/GB_video_data_numbers.csv"
gb_dn = np.loadtxt(gb_data_path, delimiter=",", dtype=np.int)
us_dn = np.loadtxt(us_data_path, delimiter=",", dtype=np.int)

# 构建标识列
gb_flag = np.zeros((gb_dn.shape[0], 1))
us_flag = np.ones((us_dn.shape[0], 1))

# 水平拼接
gb_stack_data = np.hstack((gb_dn, gb_flag))
us_stack_data = np.hstack((us_dn, us_flag))

# 垂直拼接
all_data = np.vstack((gb_stack_data, us_stack_data))

print(all_data)

```

## 3.15 随机

![image-20210125164145476](../../插图/image-20210125164145476.png)

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

np.random.seed(10)
print(np.random.rand())
print(np.random.random())
print(np.random.randint(12, 90), (5, 6))
print(np.random.uniform(1, 1.5, (3, 4)))
print(np.random.normal(1, 1.5, (2, 3)))

```



## 3.17 方阵

```python
print(np.eye(3, 6))
```

## 3.17 方法

求和：t.sum(axis=None)

均值：t.mean(a,axis=None) 受离群点的影响较大

中值：np.median(t,axis=None)

最大值：t.max(axis=None)

最小值：t.min(axis=None)

极值：np.ptp(t,axis=None) 即最大值和最小值只差

标准差：t.std(axis=None)

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：studypy
@File    ：np03.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import numpy as np

two_arr = np.arange(45).reshape(5, 9)
print(two_arr)
# 求和
print(list(two_arr.sum(axis=0)))
# 均值
print(list(two_arr.mean(axis=0)))
# 最小
print(list(two_arr.min(axis=0)))
# 最大
print(list(two_arr.max(axis=0)))
# 标准差，标准差是一组数据平均值分散程度的一种度量。一个较大的标准差，代表大部分数值和其平均值之间差异较大；
# 一个较小的标准差，代表这些数值较接近平均值反映出数据的波动稳定情况，越大表示波动越大，约不稳定
print(list(two_arr.std(axis=0)))
# 中值
print(list(np.median(two_arr, axis=0)))
# 极值，最大值和最小值只差
print(list(np.ptp(two_arr, axis=0)))

```

获取最大值最小值的位置

np.argmax(t,axis=0)

np.argmin(t,axis=1)

创建一个全0的数组: np.zeros((3,4))

创建一个全1的数组:np.ones((3,4))

创建一个对角线为1的正方形数组(方阵)：np.eye(3)
# 1 基本操作

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

# 2 数组对象

常见的数组基本到三维：页、行、列

## 2.1 N维数组

2.1.1 



```python
aa = np.array([
    [[1, 2], [3, 4]],
    [[5, 6], [7, 8]]
])
```






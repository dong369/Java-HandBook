# 1 认识

## 1.1 下载



## 1.2 导入



## 1.3 使用

折线图体现变化；散点图体现关系；条形图统计离散数据；直方图统计连续数据

## 1.4 问题

1、为什么要使用matplotlib？

2、matlib和matplotlib的关系是什么？

3、如何画图？

# 2 基本要点

## 2.1 创建

导包、数据、填充数据、画图



## 2.2属性

画的大小、字体



## 2.3 图信息

标题、X轴信息、Y轴信息



## 2.4 刻度

X轴、Y轴



## 2.5 图例

legend

## 2.6 多图



# 3 折线图

## 3.1 特点

折线图：以折线的上升或下降来表示统计数量的增减变化的统计图。

特点：能够显示数据的**变化趋势**，反映事物的变化情况。(变化)

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：折线图
@File    ：plt01.py
@Author  ：guodd
@IDE     ：PyCharm
"""
import matplotlib
from matplotlib import pyplot as plt

# 整体配置（字体、展示图片大小分辨率）
matplotlib.rc("font", family="MicroSoft YaHei")
plt.figure(figsize=(20, 8), dpi=80)

# 数据（x、y轴信息）
x = range(1, 9)
y = range(11, 19)

# 样式（标题、标注）
plt.title("统计")
plt.xlabel("x轴")
plt.ylabel("y轴")
plt.grid(alpha=0.4, linestyle=':')

# 自定义刻度
_x_labels = ["{}岁".format(i) for i in x]
_y_labels = ["{}%".format(j) for j in y]
plt.xticks(x, _x_labels)
plt.yticks(y, _y_labels)

# 填充数据、其他信息
plt.plot(x, y, label="aa", color="red")
# plt.plot(range(1, 5), range(1, 5), label="bb", color="blue")

# 标记信息生效
plt.legend(loc="best")

plt.grid(alpha=0.4)

# 进行保存
plt.savefig("./plt.png")

# 展示图片
plt.show()

```

## 3.2 画图

# 4 柱状图

## 4.1 特点

条形图：排列在工作表的列或行中的数据可以绘制到条形图中。

特点：绘制连**离散**的数据,能够一眼看出各个数据的大小,比较数据之间的差别。(统计)

## 4.2 画图

```python
""""
@Project ：纵向条形图
@File    ：plt02.py
@Author  ：guodd
@IDE     ：PyCharm
"""
from matplotlib import pyplot as plt

interval = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45, 60, 90]
width = [0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.2, 0.8]
quantity = [836, 2737, 3723, 3926, 3596, 1438, 3273, 642, 824, 613, 215, 47]

# 设置图形大小
plt.figure(figsize=(20, 8), dpi=80)

plt.bar(range(12), quantity, width=width)
# plt.barh(range(len(a)), b, height=0.3, color="orange") 横向

# 设置x轴的刻度
_x = [i - 0.5 for i in range(13)]
_x_labels = interval + [150]
plt.xticks(_x, _x_labels)

# plt.grid(alpha=0.4)
plt.show()

```



# 5 直方图

## 5.1 特点

直方图：由一系列高度不等的纵向条纹或线段表示数据分布的情况。 

一般用横轴表示数据范围，纵轴表示分布情况。

特点：绘制连**续性**的数据,展示一组或者多组数据的分布状况(统计)

## 5.2 画图

```python
""""
@Project ：直方图
@File    ：plt6.py
@Author  ：guodd
@IDE     ：PyCharm
"""
from matplotlib import pyplot as plt

a = [131, 98, 125, 131, 124, 139, 131, 117, 128, 108, 135, 138, 131, 102, 107, 114, 119, 128, 121, 142, 127, 130, 124,
     101, 110, 116, 117, 110, 128, 128, 115, 99, 136, 126, 134, 95, 138, 117, 111, 78, 132, 124, 113, 150, 110, 117, 86,
     95, 144, 105, 126, 130, 126, 130, 126, 116, 123, 106, 112, 138, 123, 86, 101, 99, 136, 123, 117, 119, 105, 137,
     123, 128, 125, 104, 109, 134, 125, 127, 105, 120, 107, 129, 116, 108, 132, 103, 136, 118, 102, 120, 114, 105, 115,
     132, 145, 119, 121, 112, 139, 125, 138, 109, 132, 134, 156, 106, 117, 127, 144, 139, 139, 119, 140, 83, 110, 102,
     123, 107, 143, 115, 136, 118, 139, 123, 112, 118, 125, 109, 119, 133, 112, 114, 122, 109, 106, 123, 116, 131, 127,
     115, 118, 112, 135, 115, 146, 137, 116, 103, 144, 83, 123, 111, 110, 111, 100, 154, 136, 100, 118, 119, 133, 134,
     106, 129, 126, 110, 111, 109, 141, 120, 117, 106, 149, 122, 122, 110, 118, 127, 121, 114, 125, 126, 114, 140, 103,
     130, 141, 117, 106, 114, 121, 114, 133, 137, 92, 121, 112, 146, 97, 137, 105, 98, 117, 112, 81, 97, 139, 113, 134,
     106, 144, 110, 137, 137, 111, 104, 117, 100, 111, 101, 110, 105, 129, 137, 112, 120, 113, 133, 112, 83, 94, 146,
     133, 101, 131, 116, 111, 84, 137, 115, 122, 106, 144, 109, 123, 116, 111, 111, 133, 150]

# 计算组数
d = 3  # 组距
num_bins = (max(a) - min(a)) // d
print(max(a), min(a), max(a) - min(a))
print(num_bins)

# 设置图形的大小
plt.figure(figsize=(20, 8), dpi=80)
plt.hist(a, num_bins)

# 设置x轴的刻度
plt.xticks(range(min(a), max(a) + d, d))

plt.grid()

plt.show()

```



# 6 散点图

## 6.1 特点

散点图：用两组数据构成多个坐标点，考察坐标点的分布,判断两变量之间是否存在某种关联或总结坐标点的分布模式。

特点：判断变量之间是否存在数量**关联趋势**,展示离群点(分布规律)

## 6.2 画图

```python
#!/usr/bin/env python
# -*- coding: utf-8 -*-
""""
@Project ：散点图
@File    ：plt07.py
@Author  ：guodd
@IDE     ：PyCharm
"""
from matplotlib import pyplot as plt
import matplotlib

y_3 = [11, 17, 16, 11, 12, 11, 12, 6, 6, 7, 8, 9, 12, 15, 14, 17, 18, 21, 16, 17, 20, 14, 15, 15, 15, 19, 21, 22, 22,
       22, 23]
y_10 = [26, 26, 28, 19, 21, 17, 16, 19, 18, 20, 20, 19, 22, 23, 17, 20, 21, 20, 22, 15, 11, 15, 5, 13, 17, 10, 11, 13,
        12, 13, 6]

x_3 = range(1, 32)
x_10 = range(51, 82)

# 设置图形大小
matplotlib.rc("font", family="MicroSoft YaHei")
plt.figure(figsize=(20, 8), dpi=80)

# 调整x轴的刻度
_x = list(x_3) + list(x_10)
_xtick_labels = ["3月{}日".format(i) for i in x_3]
_xtick_labels += ["10月{}日".format(i - 50) for i in x_10]
plt.xticks(_x[::3], _xtick_labels[::3], rotation=45)

# 添加图例
plt.legend(loc="upper left")

# 添加描述信息
plt.xlabel("时间")
plt.ylabel("温度")
plt.title("标题")

# 使用scatter方法绘制散点图,和之前绘制折线图的唯一区别
plt.scatter(x_3, y_3, label="3月份")
plt.scatter(x_10, y_10, label="10月份")

# 展示
plt.show()

```


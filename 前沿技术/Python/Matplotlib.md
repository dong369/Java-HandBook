# 1 认识

## 1.1 折线图

折线图体现变化；散点图体现关系；条形图统计离散数据；直方图统计连续数据

折线图:以折线的上升或下降来表示统计数量的增减变化的统计图

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

## 1.2 条形图

条形图：排列在工作表的列或行中的数据可以绘制到条形图中。

特点：绘制连**离散**的数据,能够一眼看出各个数据的大小,比较数据之间的差别。(统计)



## 1.3 直方图

直方图：由一系列高度不等的纵向条纹或线段表示数据分布的情况。 

一般用横轴表示数据范围，纵轴表示分布情况。

特点：绘制连**续性**的数据,展示一组或者多组数据的分布状况(统计)

## 1.4 散点图

散点图：用两组数据构成多个坐标点，考察坐标点的分布,判断两变量

之间是否存在某种关联或总结坐标点的分布模式。

特点：判断变量之间是否存在数量**关联趋势**,展示离群点(分布规律)

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


# 1 基础

## 1.1 列表推导式

### 1.1.1 基本格式

`[* for i in k]:  `*可以是一个函数，变量为i（也可以与i无关），k为一个可迭代对象，如列表。

1、一句代码输出一个1到5的立方

2、一句代码创建一个列表，包含10个60-100的随机整数

```python
import random

print([i ** 3 for i in range(1, 6)])
print([random.randint(60, 100) for _ in range(10)])
```

### 1.1.2 for循环嵌套



### 1.1.3 筛选功能

```python
print([i for i in range(12, 16) if i != 13])
```


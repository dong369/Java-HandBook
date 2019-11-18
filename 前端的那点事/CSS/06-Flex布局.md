### 1.1 前言

布局的传统解决方案是基于盒状模型，依赖 `display` + `position` + `float` 方式来实现，灵活性较差。2009 年，W3C 提出了一种新的方案 - Flex，Flex 是 Flexible Box 的缩写，意为” 弹性布局”。Flex 可以简便、完整、响应式地实现多种页面布局。下面我们就从基础语法开始，一起来感受下 Flex 的魅力吧！

### 1.2 基本概念

采用 Flex 布局的元素，称为 Flex 容器（flex container），简称 "容器"。它的所有子元素自动成为容器成员，称为 Flex 项目（flex item），简称 "项目"。

![](https://30ke.cn/u/201904/viswglwxox.jpg)

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做 main start，结束位置叫做 main end；交叉轴的开始位置叫做 cross start，结束位置叫做 cross end。

Flex 项目默认沿主轴排列。单个项目占据的主轴空间叫做 main size，占据的交叉轴空间叫做 cross size。

Flex 属性分为两部分，一部分作用于容器称容器属性，另一部分作用于项目称为项目属性。下面我们就分部的来介绍它们。

### 2.1 flex 容器定义

##### 基本语法：

```
.box {
  display: flex; 
}


```

上述写法，定义了一个 flex 容器，根据设值的不同可以是块状容器或内联容器。这使得直接子结点拥有了一个 flex 上下文。

### 2.2 容器属性 - flex-direction

`flex-direction` 属性定义主轴的方向（即项目的排列方向）。

##### 基本语法：

```
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}


```

*   `row` 表示从左向右排列
*   `row-reverse` 表示从右向左排列
*   `column` 表示从上向下排列
*   `column-reverse` 表示从下向上排列

##### 示例说明：

![](https://30ke.cn/u/201904/2wp8hsbvej.jpg)

### 2.3 容器属性 - flex-wrap

缺省情况下，Flex 项目都排在一条线（又称 "轴线"）上。我们可以通过`flex-wrap`属性的设置，让 Flex 项目换行排列。

##### 基本语法：

```
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}


```

*   `nowrap`(缺省)：所有 Flex 项目单行排列
*   `wrap`：所有 Flex 项目多行排列，按从上到下的顺序
*   `wrap-reverse`：所有 Flex 项目多行排列，按从下到上的顺序

##### 示例说明：

![](https://30ke.cn/u/201904/yiqngv9tgw.jpg)

观察下述演示程序 ，理解不同属性的含义。点击演示区域，可启动或停止演示。

### 2.4 容器属性 - flex-flow

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`

##### 基本语法：

```
.box {
    flex-flow: <‘flex-direction’> || <‘flex-wrap’>
}


```

### 2.5 容器属性 - justify-content

##### 基本语法：

```
.box  {
    justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}


```

*   `flex-start`(缺省)：从启点线开始顺序排列
*   `flex-end`：相对终点线顺序排列
*   `center`：居中排列
*   `space-between`：项目均匀分布，第一项在启点线，最后一项在终点线
*   `space-around`：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和
*   `space-evenly`：项目均匀分布，所有项目之间及项目与边框之间距离相等

##### 示例说明：

![](https://30ke.cn/u/201904/ao48o5xfqw.jpg)

### 2.6 容器属性 - align-items

`align-items`属性定义项目在交叉轴上的对齐方式。

##### 基本语法：

```
.box {
  align-items: stretch | flex-start | flex-end | center | baseline;
}


```

*   `stretch`(缺省)：交叉轴方向拉伸显示
*   `flex-start`：项目按交叉轴起点线对齐
*   `flex-end`：项目按交叉轴终点线对齐
*   `center`：交叉轴方向项目中间对齐
*   `baseline`：交叉轴方向按第一行文字基线对齐

##### 示例说明：

![](https://30ke.cn/u/201904/rwrne1upvy.jpg)

### 2.7 容器属性 - align-content

`align-content`属性定义了在交叉轴方向的对齐方式及额外空间分配，类似于主轴上`justify-content`的作用。

##### 基本语法：

```
.box {
    align-content: stretch | flex-start | flex-end | center | space-between | space-around ;
}


```

*   `stretch` (缺省)：拉伸显示
*   `flex-start`：从启点线开始顺序排列
*   `flex-end`：相对终点线顺序排列
*   `center`：居中排列
*   `space-between`：项目均匀分布，第一项在启点线，最后一项在终点线
*   `space-around`：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和

##### 示例说明：

![](https://30ke.cn/u/201904/m9jzzancqf.jpg)

### 3.1 项目属性 - order

缺省情况下，Flex 项目是按照在代码中出现的先后顺序排列的。然而`order`属性可以控制项目在容器中的先后顺序。

##### 基本语法：

```
.item {
  order: <integer>; 
}


```

按`order`值从小到大顺序排列，可以为负值，缺省为 0。

##### 示例说明：

![](https://30ke.cn/u/201904/mdruu3js6t.jpg)

### 3.2 项目属性 - flex-grow

`flex-grow`属性定义项目的放大比例，`flex-grow` 值是一个单位的正整数，表示放大的比例。默认为 0，即如果存在额外空间，也不放大，负值无效。

如果所有项目的 flex-grow 属性都为 1，则它们将等分剩余空间（如果有的话）。如果一个项目的 flex-grow 属性为 2，其他项目都为 1，则前者占据的剩余空间将比其他项多一倍。

##### 基本语法：

```
.item {
  flex-grow: <number>; 
}


```

##### 示例说明：

![](https://30ke.cn/u/201904/nojviureqa.jpg)

### 3.3 项目属性 - flex-shrink

`flex-shrink`属性定义了项目的缩小比例，默认为 1，即如果空间不足，该项目将缩小。0 表示不缩小，负值无效。

##### 基本语法：

```
.item {
  flex-shrink: <number>; 
}


```

##### 示例说明：

![](https://30ke.cn/u/201904/l6p8dtehlg.jpg)

### 3.4 项目属性 - flex-basis

`flex-basis`属性定义项目在分配额外空间之前的缺省尺寸。属性值可以是长度（20%，10rem 等）或者关键字`auto`。它的默认值为 auto，即项目的本来大小。

##### 基本语法：

```
.item {
  flex-basis: <length> | auto; 
}


```

##### 示例说明：

![](https://30ke.cn/u/201904/y5jd1gpqux.jpg)

### 3.5 项目属性 - flex

`flex`属性是`flex-grow`, `flex-shrink` 和`flex-basis`的简写，默认值为 0 1 auto。后两个是可选属性。

##### 基本语法：

```
.item {
  flex: none | [ <'flex-grow'> <'flex-shrink'>? || <'flex-basis'> ]
}


```

一般推荐使用这种简写的方式，而不是分别定义每一个属性。

### 3.6 项目属性 - align-self

`align-self`属性定义项目的对齐方式，可覆盖 align-items 属性。默认值为 auto，表示继承父元素的 align-items 属性，如果没有父元素，则等同于 stretch。

##### 基本语法：

```
.item {
  align-self: auto | flex-start | flex-end | center | baseline | stretch;
}


```

除了 auto 值以外，`align-self` 属性与容器的`align-items`属性基本一致。

##### 示例说明：

![](https://30ke.cn/u/201904/q2ak9pkguj.jpg)

### 4.1 兼容性

| Chrome | Safari | Firefox | Opera | IE   | Android | iOS  |
| ------ | ------ | ------- | ----- | ---- | ------- | ---- |
| 21+    | 6.1+   | 22+     | 12.1+ | 11+  | 4.4+    | 7.1+ |

Flexbox 需要一些特定的前缀以支持大多数的浏览器。甚至还存在完全不同的属性名称或属性值。这就需要 [Autoprefixer](https://github.com/postcss/autoprefixer) 或类似的 CSS 后处理器辅助。

### 4.2 参考文献

### 4.3 结语

以上是 @毛瑞总结的 Flex 弹性布局语法手册，不足之处请各位程序员多多指正！

费力原创十分不易，请大家转载时一定要明确注明出处来自【[三十课](https://30ke.cn/)】！
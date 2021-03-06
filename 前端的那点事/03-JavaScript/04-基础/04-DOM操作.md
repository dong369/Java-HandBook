# 1. 前言

本文中简要罗列了 JavaScript 操作 Dom 的基本方法，其中包括元素查询、文档结构遍历、属性及内容操作、创建节点、插入节点及删除节点等内容。虽然 JQuery 更便利，但我还是喜欢用原生的 API。

# 2. 选择器

## 2.1 ID 选择器

通过 ID 选取元素是最简单和常用的选取元素的方法，ID 选择器性能优于其它选择器。

```javascript
var title = document.getElementById("title");
```

ID 不存在，返回 `null` 。

## 2.2 名称选择器

基于 name 属性的值选取元素区别于 ID 选择器。其一，name 属性值 不是必须唯一，多个元素可能有同样的名称；其二，name 属性只在少数 HTML 元素中有效，包括表单、表单元素、`iframe` 及 `img` 元素。

```
var sports = document.getElementsByName("sports");
```

## 2.3 标签选择器

利用 HTML 元素的标签名称选取指定类型的元素。

```
var h1 = document.getElementsByTagName("h1");
```

## 2.4 类选择器

通过 HTML 的 class 属性值选择元素。

```
var title = document.getElementsByClassName("title");
```

## 2.5 CSS单元素选择器

通过 CSS 样式表选择器的强大语法，来选择元素。返回第一个匹配的元素。

```
var title = document.querySelector("#title");   
var h1 = document.querySelector("h1");
```

## 2.6 CSS多元素选择器

这是最强大的终极选择器

```
var h1s = document.querySelectorAll("h1");
```

# 3. 节点相关

## 3.1 父节点

父节点 - parentNode，返回父节点，如果 `document` 对象调用则返回 `null`。

```
var parent = node.parentNode;
```

## 3.2 子节点

子节点 - childNodes，返回所有子节点，即 NodeList 对象。

```
var children = node.childNodes;
```

## 3.3 首子节点

首子节点 - firstChild，返回第一个子节点。

```
var first = node.firstChild;
```

## 3.4 尾子节点

尾子节点 - lastChild，返回最后一个子节点。

```
var last = node.lastChild;
```

## 3.5 下一兄弟节点

下一兄弟节点 - nextSibling，返回下一个兄弟节点。

```
var next = node.nextSibling;
```

## 3.6 前一兄弟节点

前一兄弟节点 - previousSibling，返回前一个兄弟节点。

```
var previous = node.previousSibling;
```

## 3.7 节点类型

节点类型 - nodeType，返回节点类型的数字表示。

*   1 代表 `Element` 节点
*   3 代表 `Text` 节点
*   8 代表 `Comment` 节点
*   9 代表 `Document` 节点
*   11 代表 `DocumentFragment` 节点

## 3.1.8 节点值

节点值 - nodeValue，返回 `Text` 节点 或 `Comment` 节点的值。

```
var value = node.nodeValue;


```

## 3.1.9 节点名

节点名 - nodeName，返回元素的标签名，以大写形式表示。

```
var name = node.nodeName;


```

# 3. 元素相关

## 3.1 子元素

子元素-children，返回所有子元素。

```
var children = node.children;
```

## 3.2 首子元素

> 元素是节点的一种。

首子元素 - firstElementChild，返回所有子元素中的第一个。

```
var first = node.firstElementChild;


```

## 3.3 尾子元素

尾子元素 - lastElementChild，返回所有子元素中的最后一个。

```
var last= node.lastElementChild;
```

## 3.4 下一兄弟元素

下一兄弟元素 - nextElementSibling，返回下一个兄弟元素。

```
var next = node.nextElementSibling;


```

## 3.2.5 前一兄弟元素

前一兄弟元素 - previousElementSibling，返回前一兄弟元素。

```
var previous = node.previousElementSibling;


```

## 3.2.6 子元素数量

返回子元素的数量。

```javascript
var count = node.childElementCount;
```

# 4. 属性相关

## 4.1 标准属性

表示 HTML 文档元素的 `HTMLElement` 对象定义了读 / 写属性，它们对应于元素的 HTML 属性。`HTMLElement` 定义了通用的 HTML 属性，包括 id、lang、dir、事件处理程序 `onclick` 及表单相关属性等。

```
form.action = "http://30ke.cn";
form.method = "post";
```

## 4.2 非标准属性

### 4.2.1 获取属性值

获取属性值 - getAttribute，返回非标准的 HTML 属性的值。

```
var width = img.getAttribute("width");
```

### 4.2.2 属性值设置

属性值设置 - setAttribute，设置非标准的 HTML 属性的值。

```
img.setAttribute("width","400px");
```

### 4.2.3 属性存在检测

属性存在检测 - hasAttribute，返回布尔值，用来检测属性是否存在。

```
var result = img.hasAttribute("width");


```

### 4.2.4 删除属性

删除属性 - removeAttribute，删除某一属性。

```
img.removeAttribute("width");


```

### 4.3 数据集属性 - dataset

在 HTML5 文档中，任意以 `data-` 为前缀的小写的属性名字都是合法的。这些 “数据集属性” 定义了一种标准的、附加额外数据的方法。

```
var x = img.dataset.x;


```

### 4.4 元素属性 - attributes

Node 节点定义了 `attributes` 属性，针对 Element 对象，`attributes` 是元素所有属性的类数组对象。

```
var attributes = img.attributes;


```

索引 `attributes` 对象得到的值是 Attr 对象。Attr 的 name 和 value 返回该属性的名字和值。

### 5.1 元素内容 - innerHTML

`innerHTML` 属性以字符串形式返回这个元素的内容。也可以用来替换元素当前内容。

```
var innerHTML = parent.innerHTML;     
parent.innerHTML = "<h1>三十课</h1>";  


```

### 5.2 元素及内容 - outerHTML

`outerHTML` 属性以字符串形式返回这个元素及内容。也可以用来替换当前元素及内容。

```
var outerHTML = parent.outerHTML;     
parent.outerHTML= "<h1>三十课</h1>";  


```

### 5.3 纯文本元素内容 - textContent

查询或替换纯文本元素内容的标准方法是用 Node 的 textContent 属性来实现。在 IE 中，可以用 Element 的 `innerText` 属性来代替。

```
console.log(title.textContent);
title.textContent = "三十课2";


```

### 6.1 创建元素节点 - createElement

使用 document 对象的 `createElement()` 方法创建新的 Element 节点。

```
var h1 = document.createElement("h1");
```

### 6.2 创建文本节点 - createTextNode

创建纯文本节点。

```
var txt = document.createTextNode("三十课");
```

### 6.3 创建文档片段 - createDocumentFragment

使用文档片段通常会带来更好的性能。因为文档片段存在于内存中，并不在 DOM 树中，所以将子元素插入到文档片段时不会引起页面回流（对元素位置和几何上的计算）。

```
var fragment = document.createDocumentFragment();
```

创建注释节点不经常使用。

```
var comment = document.createComment("Created by 毛瑞");
```

### 6.5 节点克隆 - cloneNode

通过复制已存在的节点来创建新的文档节点。传参数 true 表示深克隆，false 表示浅复制。

```
var title2 = title.cloneNode(true);


```

### 7.1 插入子节点 - appendChild

在指定元素上插入子节点，并使其成为该节点的最后一个子节点。

```
parent.appendChild(h2); 
```

### 7.2 节点前插入 - insertBefore

*   在父节点上调用本方法
*   第一参数表示待插入的节点
*   第二参数是父节点中已经存在的子节点，新节点插入到该节点的前面

```
parent.insertBefore(h1,h2);
```

### 8.1 删除子节点 - removeChild

*   在父节点上调用
*   参数是待删除的节点

```
parent.removeChild(h2);
```

### 8.2 替换子节点 - replaceChild

*   在父节点上调用
*   第一个参数是新节点
*   第二个参数是需要替换的节点

```
parent.replaceChild(h2n , h2);  
```

### 9.1 结语

本人知识水平有限，在汇编的过程中时有错误发生，如发现请不吝指正！多谢。
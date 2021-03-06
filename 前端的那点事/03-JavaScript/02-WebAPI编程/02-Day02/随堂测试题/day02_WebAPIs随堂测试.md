**考点：操作自定义属性**

单选题

关于操作元素的class属性，正确的是：

A，element.setAttribute('className', 'active');

B，element.class = 'active'; 

C，element.className = 'active';

D，element.class-name = 'active';

答案：c

解析：操作元素属性的格式为：元素对象.属性名 = 值; 但是class属性特殊，用className。使用setAttribute操作class时用class

难度: ☆☆



**考点：操作自定义属性**

多选题

有一标签<div data-uname="ls"></div> ，可以获取标签自定义属性的选项是：

A，divObj.dataset["uame"]

B，divObj.dataset.uname 

C，divObj.getAttribute('data-uname')

D，divObj.data-uname

答案：ABC

解析：1.兼容性获取   element.getAttribute(‘data-index’);2.H5新增 element.dataset.index  或者 element.dataset[‘index’]   

难度: ☆☆☆





**考点：节点的层级关系**

单选题

关于节点操作，下面选项能获取子元素节点的是：

A，element.child

B，element.childNode

C，element.childNodes

D，element.children

答案：D

解析：获取子节点使用childNodes，获取子元素节点使用children

难度: ☆





**考点：节点的层级关系**

多选题

关于节点的层级关系操作，下面描述正确的是：

A，使用子节点.parentNode可以找到父节点

B，使用父节点.children 找到所有的子元素

C，使用父节点.children 找到所有的子节点

D，使用节点.parentNode.parentNode可以找到父节点的父节点

答案：ABD

解析：节点之间的层级关系，获取父节点使用parentNode，获取子节点使用childNodes，获取子元素children

难度: ☆☆
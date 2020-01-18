**考点：window对象**

单选题

下列哪些选项，不是window对象的属性：

A，pageX

B，location

C，history

D， navigator  

答案：A

解析：window对象是顶级对象，location、history、navigator这些对象都是其属性。pageX是鼠标事件对象的属性。

难度: ☆





**考点：window对象**

多选题

下列选项描述正确的是：

A，onload和DOMContentLoaded都是页面加载事件，没有区别

B，DOMContentLoaded有浏览器兼容问题

C，全局变量和函数都是window对象的属性和方法

D， window对象的方法在调用时可以省略不写window

答案：BCD

解析：onload是页面内容全部加载完触发。DOMContentLoaded是DOM元素加载完触发，且从IE9开始支持。window对象是顶级对象，定义在全局作用域中的变量、函数都会变成 window 对象的属性和方法。

难度: ☆☆



**考点：定时器**

多选题

关于定时器，描述正确的是：

A，setTimeout()开启的定时器只会触发1次

B，定时器一旦开启，不能清除

C，清除定时器时需要传入定时器标识

D，开启定时器时，单位是毫秒

答案：ACD

解析：setTimeou()相当于炸弹，只会触发1次。定时器都可以被清除，清除时需要指定清除哪个。

难度: ☆☆





**考点：定时器**

多选题

下面代码，能正确开启定时器的是：

A，window.setTimeout(function(){console.log('开启定时器')}, 1000);

B，setInterval(function(){console.log('开启定时器')}, 1000);

C，setTimeout(function(){console.log('开启定时器')},0);

D，setTimeout(function(){console.log('开启定时器')});

答案：ABCD

解析：定时器的参数不写默认为0。

难度: ☆☆






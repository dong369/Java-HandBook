**考点：创建添加元素**

多选题

下列哪些选项可以实现创建元素：

A，element.push('<p>传智播客</p>')

B，element.pop('<p>传智播客</p>');

C，element.innerHTML = '<p>传智播客</p>';

D， document.createElement("p")';

答案：CD

解析：创建元素的方法

难度: ☆☆





**考点：创建添加元素**

多选题

关于添加元素，下列选项描述正确的是：

A，innerHTML会覆盖原来的元素

B，appendChild 是在父元素内部追加

C，insertBefore 可以在父元素内部指定的位置添加

D， createElement创建的元素立即会添加在页面

答案：ABC

解析：创建添加元素的方法，D选项，createElement要配合其他添加方法使用。

难度: ☆☆



**考点：事件监听**

多选题

关于事件监听，描述正确的是：

A，可以给同一元素同一事件注册多个监听器

B，addEventListener() 有浏览器兼容问题 

C，addEventListener() 方法有3个参数

D，低版本的IE可以使用attacheEvent代替addEventListener

答案：ABCD

解析：IE9以后才支持addEventListener，低版本IE使用attacheEvent。

难度: ☆☆





**考点：事件对象**

单选题

关于事件对象，描述正确的是：

A，事件对象的属性中保存了跟事件相关的一系列信息

B，事件触发时就会产生事件对象

C，事件对象的获取有兼容性问题

D，通过事件对象可以阻止事件冒泡和默认行为

答案：ABCD

解析：事件对象有很多方法和属性，保存了事件相关的信息，获取时有兼容性问题，使用事件对象可以阻止事件冒泡和默认行为

难度: ☆☆





**考点：事件对象**

单选题

下列选项，哪个可以正确获取到兼容了各个浏览器的事件对象：

A，document.onclick = function(evt){  var e=window.event||event;  };

B，document.onclick  = function(evt){  var e=window.evt||event;  };

C，document.onclick  = function(evt){  var e=window.event||evt;  };

D，document.onclick  = function(evt){  var e=window.evt||evt;  };

答案：C

解析：事件对象的兼容性考虑。低版本的IE，通过 window.event获取事件对象。

难度: ☆☆
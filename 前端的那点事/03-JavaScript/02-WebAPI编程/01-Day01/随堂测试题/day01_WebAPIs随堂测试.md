**考点：获取元素**

单选题

关于获取元素,以下描述正确的是:

A，document.getElementById()获取到的是元素集合

B，document.getElementsByTagName()获取到的是单个元素

C， document.querySelector()获取到的是元素集合

D， document.getElementsByClassName()有浏览器兼容性问题

答案: D

解析: A选项，返回的是单个元素对象或null；B选项返回的是元素集合；C选项返回的单个元素对象；D选项，是h5新增的方法，所以有浏览器兼容性问题

难度: ☆☆ 









**考点：获取元素**

单选题

关于获取元素,以下获取到单个元素的方法是:

A，document.getElementById()

B，document.getElementsByTagName()

C， document.querySelectorAll()

D， document.getElementsByClassName()

答案: D

解析: A选项，返回的是单个元素对象或null；

难度: ☆





**考点：获取元素**

多选题

关于获取元素,以下获取到元素集合的方法是:

A，document.getElementById()

B，document.getElementsByClassName()

C， document.querySelector()

D， document.querySelectorAll()

答案: AD

解析: A选项和D选项，返回的是元素集合；

难度: ☆

 





**考点：注册事件**

单选题

点击一个按钮，弹出对话框, ____应该填写的正确代码是():

~~~javascript
<body>
    <button id="btn">唐伯虎</button>
    <script>
        var btn = document.getElementById('btn');
		____________
    </script>
</body>
~~~

A, btn.onclick = function() {

​            alert('点秋香');

​      }

B, btn.onclick = alert('点秋香');

C, btn.click = function() {

​            alert('点秋香');

​        }

D, btn.click()

答案: A

解析: 考查注册单击事件，选项A是正确的格式

难度: ☆☆ 









**考点: 注册事件**

多选题

关于事件，下列描述正确的是：

A，当页面一打开，所有的事件就会被触发

B，注册事件的格式为： 事件源.事件 = 事件处理程序

C，一个事件只能触发执行一次

D，事件函数内this指的是当前触发事件的元素

答案: BD

解析: A选项，页面加载完只会触发页面加载事件；B选项，正确的注册事件方式；C选项，事件是触发一次执行一次；D选项，事件处理函数中this指向事件源对象；

难度: ☆☆







**考点：操作元素内容**

多选题

关于操作元素的内容，说法正确的是：

A，innerHTML可以识别渲染html标签

B，innerText不可以识别html标签 

C，element.innerHTML = ''; 和 element.innerText = ''; 作用一样

D，var content = element.innerHTML; 和 var content = element.innerText; 的作用一样

答案：ABC

解析：innerText和innerHTML的区别。修改内容时,innerHTML可以识别渲染标签，innerText不可以，所有AB选项正确。C选项，都是清空标签的内容，正确。D选项，获取内容时，innerHTML会原封不动的获取内容包括标签、空白、换行，而innerText会过滤标签、空白、换行。

难度: ☆☆☆







**考点：操作元素属性**

单选题

关于操作元素的属性，错误的是：

A，element.id = 'box01';

B，element.src = 'image/new.jpg'; 

C，element.title = '黑马程序员';

D，element.class = 'contentLeft';

答案：D

解析：操作元素常用属性。操作元素属性的格式为：元素对象.属性名 = 值; 但是class属性特殊，用className。

难度: ☆☆


# 1 HTTP通信

## 1.1 流程

客户端将**请求**打包成HTTP的请求报文（HTTP协议格式的请求数据）

采用TCP传输发送给服务器端

服务器接收到请求报文后按照HTTP协议进行解析

服务器根据解析后获知的客户端请求进行逻辑执行

服务器将执行后的结果封装成HTTP的响应报文（HTTP协议格式的响应数据）

采用刚才的TCP连接将响应报文发送给客户端

客户端按照HTTP协议解析响应报文获取结果数据

## 1.2 细节

客户端不一定是浏览器，也可以是PC软件、手机APP、程序

根据服务器端的工作，将其分为两部分：服务器、业务程序

服务器：与客户端进行tcp通信，接收、解析、打包、发送http格式数据

业务程序：根据解析后的请求数据执行逻辑处理，形成要返回的数据交给服务器

服务器与Python业务程序的配合使用**WSGI协议**。

# 2 Web框架

## 2.1 Web框架

能够被服务器调用起来，根据客户端的不同请求执行不同的逻辑处理形成要返回的数据的程序

核心：实现路由和视图（业务逻辑处理）

## 2.2 框架的轻重

重量级的框架：为方便业务程序的开发，提供了丰富的工具、组件，如Django

轻量级的框架：只提供Web框架的核心功能，自由、灵活、高度定制，如Flask、Tornado、Bottle

## 2.3 Web开发的任务

视图开发：根据客户端请求实现业务逻辑（视图）编写

模板、数据库等其他的都是为了帮助视图开发，不是必备的

# 3 认识Flask

## 3.1 简介

Flask诞生于2010年，是Armin ronacher（人名）用Python语言基于Werkzeug工具箱编写的轻量级Web开发框架。它主要面向需求简单的小应用。

Flask本身相当于一个内核，其他几乎所有的功能都要用到扩展（邮件扩展Flask-Mail，用户认证Flask-Login），都需要用第三方的扩展来实现。比如可以用Flask-extension加入ORM、窗体验证工具，文件上传、身份验证等。Flask没有默认使用的数据库，你可以选择MySQL，也可以用NoSQL。其 WSGI 工具箱采用 Werkzeug（路由模块） ，模板引擎则使用 Jinja2 。

可以说Flask框架的**核心就是Werkzeug和Jinja2**。 

Python最出名的框架要数Django，此外还有Flask、Tornado等框架。虽然Flask不是最出名的框架，但是Flask应该算是**最灵活**的框架之一，这也是Flask受到广大开发者喜爱的原因。

## 3.2 与Django对比

django提供了：

django-admin快速创建项目工程目录

manage.py 管理项目工程

orm模型（数据库抽象层）

admin后台管理站点

缓存机制

文件存储系统

用户认证系统

而这些，flask都没有，都需要扩展包来提供

## 3.3 Flask扩展包

Flask-SQLalchemy：操作数据库；

Flask-migrate：管理迁移数据库；

Flask-Mail:邮件；

Flask-WTF：表单；

Flask-script：插入脚本；

Flask-Login：认证用户状态；

Flask-RESTful：开发REST API的工具；

Flask-Bootstrap：集成前端Twitter Bootstrap框架；

Flask-Moment：本地化日期和时间；

## 3.4 Flask文档

中文文档： http://docs.jinkan.org/docs/flask/

英文文档： http://flask.pocoo.org/docs/0.11/

# 4 虚拟环境

## 4.1 Virtual

虚拟环境是一个互相隔离的目录

1、mkvirtualenv flask_py2

2、pip install flask==0.10.1

 

pip freeze > requirements.txt

pip install –r requirements.txt

## 4.2 Conda

```python
conda -V
conda create -n py2.6 python=2.6
conda activate py2.6
conda remove py2.6
deactivate
```

## 4.3 hello word

```python
# 导入Flask类
from flask import Flask, render_template

# Flask类接收一个参数__name__表示当前模块的名字。
# 模块名，flask是以这个模块所在的目录为总目录，默认情况这个目录中的static为静态目录，templates为模板目录
app = Flask(__name__)


# 将 '/' 和 函数index的对应关系添加到路由中。
# 装饰器的作用是将路由映射到视图函数index
@app.route('/hello')
def hello_world():
    return 'Hello World!'


@app.route('/info')
def info():
    return "java", 200, {"aa": "bb"}


@app.route('/tem')
def tem():
    return render_template("index.html")


# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    # 监听用户请求
    # 如果有用户请求到来，则执行app的__call__方法
    # app.__call__
    app.run()

```

解决pycharm运行Flask指定ip、端口更改无效，后来查了一下官网文档，原来Flask 1.0 版本不再支持之前的FLASK_ENV 环境变量了。

Prior to Flask 1.0 the FLASK_ENV environment variable was not supported and you needed to enable debug mode by exporting FLASK_DEBUG=1. This can still be used to control debug mode, but you should prefer setting the development environment as shown above.

pycharm会自动识别出来你的flask项目（即使你创建项目的时候并没有选择flask框架的模板），但是在你运行的时候依旧是下图所示，右上角以flask的logo运行的。我们只需要**切换成python模式运行**就行啦。

# 5 视图

## 5.1 app对象

### 5.1.1 初始化参数

```python
import_name：app = Flask(__name__)，如何理解__name__

static_url_path：默认是static

static_folder：默认‘static’

template_folder：默认‘templates’
```

import_name和static_url_path的区别：import_name是**请求地址**如果包含指定的路径会到static_url_path下**寻找静态资源**。

### 5.1.2 配置参数

```python
app.config.from_pyfile("yourconfig.cfg") 
或
app.config.from_object()
```

在Django中，有一个程序的配置文件settings.py，但是在flask中并没有settings.py这个文件，不过不必担心，flask提供了3种应用配置的方式，分别如下：

1、文件配置，app.config.from_pyfile(file)：使用配置文件

```python
# 开启debug
DEBUG = True
# MySQL配置
SQLALCHEMY_DATABASE_URI = mysql://root:passw0rd@127.0.0.1:3306/test
SQLALCHEMY_COMMIT_ON_TEARDOWN = True
SQLALCHEMY_TRACK_MODIFICATIONS = True
SQLALCHEMY_ECHO = True
# CORS防御
WTF_CSRF_ENABLED = True
SECRET_KEY = 'BB'

# 文件名: 配置文件一般是.cfg结尾
app.config.from_pyfile("config.cfg")
```

2、对象配置，app.config.from_object(obj)：使用对象配置参数

```python
class Config(object):
    DEBUG = True
    ITCAST = 'PYTHON'

app.config.from_object(Config)
```

3、直接配置，app.config：直接操作全局对象

```python
app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:passw0rd@127.0.0.1:3306/test'
# 设置每次请求结束后会自动提交数据库中的改动
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
# 查询时会显示原始SQL语句
app.config['SQLALCHEMY_ECHO'] = True
db = SQLAlchemy(app)
```



### 5.1.3 在视图读取配置参数

1、如果可以拿到app对

```python
app.config.get() 
```

2、不能拿到app对象

```python
from flask current_app
current_app.config.get()
```

### 5.1.4 app.run的参数

```python
# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    print(app.url_map)
    app.run(host="0.0.0.0", port=5000)
```

## 5.2 路由

### 5.2.1 app.url_map查看所有路由

 ```python
print(app.url_map)
 ```



### 5.2.2 同一路由装饰多个视图函数

 ```python
@app.route('/hello', methods=["GET"])
def hello():
    return 'hello1'


@app.route('/hello', methods=['POST'])
def hello2():
    return 'hello2'
 ```



### 5.2.3 同一视图多个路由装饰器

 ```python
@app.route('/hi1')
@app.route('/hi2')
def hi():
    return 'hi page'
 ```



### 5.2.4 利用methods限制访问方式

```python
@app.route("/index", methods=["GET", "POST"])
def index():
    # request中包含了前端发送过来的所有数据
    # 通过request.form可以直接提取请求体中表单格式的数据，是一个类
    name = request.form.get("name", "zhangsan")
    age = request.form.get('age')
    print(request.form)
    return "hello name=%s, age=%s " % (name, age)
```



### 5.2.5 使用url_for进行反解析

 

### 5.2.6 动态路由

```python
# 路由传递的参数默认当做string处理，这里指定int，尖括号中冒号后面的内容是动态的
@app.route('/user/<int:sid>')
def it_cast(sid):
    return 'hello it_cast %d' % sid
```



### 5.2.7 自定义转换器



## 5.3 请求参数

```python
from flask import request
```

就是Flask中表示当前请求的request对象，request对象中保存了一次HTTP请求的一切信息。

### 5.3.1 mimetype

| 属性    | 说明                           | 类型           |
| :------ | :----------------------------- | :------------- |
| data    | 记录请求的数据，并转换为字符串 | ·              |
| form    | 记录请求中的表单数据           | MultiDict      |
| args    | 记录请求中的查询参数           | MultiDict      |
| cookies | 记录请求中的cookie信息         | Dict           |
| headers | 记录请求中的报文头             | EnvironHeaders |
| method  | 记录请求使用的HTTP方法         | GET/POST       |
| url     | 记录请求的URL地址              | string         |
| files   | 记录请求上传的文件             | ·              |

### 5.3.2 form表单

```python
@app.route(rule='/formObj', methods=['POST'])
def form_obj():
    # 直接获取
    name = request.form.get("name", "guo")
    # 后端接收字典dict数据
    post_form = dict(request.form)
    print(post_form)
    return jsonify(code=200, status=0, message='ok', data={})



@app.route("/index", methods=["GET", "POST"])
def index():
    # request中包含了前端发送过来的所有数据
    # form和data，查询请求体数据
    name = request.form.get("name", "zhangsan")
    age = request.form.get('age')

    print(request.data) # 二进制的数据
    print(request.data.decode('utf8')) # 需要解码成utf8
    return "hello name=%s, age=%s" % (name, age, city)


@app.route("/index", methods=["GET", "POST"])
def index():
   # request中包含了前端发送过来的所有数据
   # 通过request.form可以直接提取请求体中表单格式的数据，是一个类
   name = request.form.get("name", "zhangsan")
   age = request.form.get('age')
   print(request.form)
   return "hello name=%s, age=%s " % (name, age)
```



### 5.3.3 json字符串

```python
@app.route(rule='/jsonStr', methods=['POST'])
def info():
    # 方式一
    data01 = request.get_json()
    print(data01)
    # 方式二
    data02 = request.get_data()
    print(data02)
    return jsonify(code=200, status=0, message='ok', data={})
```



### 5.3.4 上传文件

```python
request.files
```

已上传的文件存储在内存或是文件系统中一个临时的位置。你可以通过请求对象的 files 属性访问它们。每个上传的文件都会存储在这个字典里。它表现近乎为一个标准的 Python file 对象，但它还有一个 save() 方法，这个方法允许你把文件保存到服务器的文件系统上。这里是一个用它保存文件的例子:

```python
@app.route('/upload', methods=['GET', 'POST'])
def upload_file():
    if request.method == 'POST':
        f = request.files['the_file']
        print(f)
        f.save("d:/aa.docx")
        return jsonify("保存成功！", 200)
```

 如果你想知道上传前文件在客户端的文件名是什么，你可以访问filename属性。但请记住， 永远不要信任这个值，这个值是可以伪造的。如果你要把文件按客户端提供的文件名存储在服务器上，那么请把它传递给Werkzeug 提供的secure_filename() 函数。

## 5.4 异常处理

### 5.4.1 abort函数

```python
from flask import abort
```

类比Java中的**throw关键字**，直接抛异常。

### 5.4.2 自定义异常处理

```python
@app.errorhandler(404)
def handler_404(err):
    return '您请求的页面不存在了，请确认后再次访问！%s' % err
```

## 5.5 响应数据 

### 5.5.1 元组

可以返回一个元组，这样的元组必须是(response, status, headers)的形式，且至少包含一个元素。 

status 值会覆盖状态代码， headers可以是一个列表或字典，作为额外的消息标头值。

```python
@app.route('/tum_info')
def tum_info():
    return "成功", 200
```



### 5.5.2 make_response

```python
@app.route('/make_res')
def make_res():
    res = make_response()
    res.headers['aa'] = 'bb'
    res.status = "400 my error info"
    return res
```



### 5.5.3 Json数据

 ```python
@app.route('/json_info')
def json_info():
    return jsonify(code=200, status=0, message='ok', data={})
 ```



### 5.5.4 重定向

 ```python
from flask import redirect
 ```



## 5.6 cookie

### 5.6.1 设置

```python
@app.route('/make_res')
def make_res():
    res = make_response()
    res.set_cookie('aa', value='bb', max_age=None)
    return res
```



### 5.6.2 读取删除

```python
@app.route('/make_res')
def make_res():
    res = make_response()
    # 在请求中获取存入的cookie
    print(request.cookies['aa'])
    # 删除cookie信息
    res.delete_cookie('aa')
    return res
```



## 5.7 session

### 5.7.1 设置

```python
from flask import session

session["name"] = "guo"
session["age"] = 12
```

需要设置secret_key

```properties
SECRET_KEY='abc'
或
app.config.set("SECRET_KEY")="abc"
```



### 5.7.2 读取

```python
print(session.get("name"))
```



## 5.8 上下文

###  5.8.1 请求上下文

请求上下文(request context) 

request和session都属于请求上下文对象。

 

###  5.8.2 应用上下文

应用上下文(application context)，current_app和g都属于应用上下文对象。

current_app：表示当前运行程序文件的程序实例。

g：处理请求时，用于临时存储的对象，每次请求都会重设这个变量。

 

## 5.9 请求钩子

### 5.9.1 四种钩子

请求钩子是通过装饰器的形式实现，Flask支持如下四种请求钩子：

before_first_request：在处理第一个请求前运行。

@app.before_first_request

before_request：在每次请求前运行。

after_request(response)：如果没有未处理的异常抛出，在每次请求后运行。

teardown_request(response)：在每次请求后运行，即使有未处理的异常抛出。

# 6 Jinja2模板

flask本身没有加载模板的能力，需要开发者自己编写，但是目前flask使用了三方的模板系统：jinja2模板系统，jinja2是仿照django的模板系统开发的一套模板系统，但是功能比django的模板系统强。

## 6.1 基本流程

使用flask中的render_template渲染模板

```python
@app.route('/')
def info():
    return render_template('index.html', name='python')
```



## 6.2 变量

1、后台数据

```python
@app.route('/')
def info():
    my_disc = {"name": "guo", "age": 12}
    return render_template('index.html', my_disc=my_disc)
	# return render_template('index.html', **my_disc)
```

2、前端展示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
姓名：{{ my_disc.name }}
年龄：{{ my_disc.age }}
</body>
</html>
```



## 6.3 过滤器

### 6.3.1 字符串过滤器



### 6.3.2 支持链式使用过滤器



### 6.3.3 列表过滤器



### 6.3.4 自定义过滤器

自定义的过滤器名称如果和内置的过滤器重名，会覆盖内置的过滤器。

1、方式一

```python
# 定义
def lsit_filter(li1):
    retrun li1[::2]
# 注册
app.add_template_filter(lsit_filter,"li1")
```



2、方式2

```python
@app.template_filter("li2")
def list_filter(li2):
    return li2[::3]
```

## 6.4 表单

使用Flask-WTF表单扩展，可以帮助进行**CSRF验证**，帮助我们快速定义表单模板，而且可以帮助我们在视图中验证表的数据

 ```python
pip install Flask-WTF
 ```

### 6.4.1 自己处理

不使用Flask-WTF扩展时，表单需要自己处理



### 6.4.2 使用Flask-WTF扩展

1、需要设置SECRET_KEY的配置参数

 ```python
# 配置
app.config['SECRET_KEY'] = 'you can't know'
或csrf专用令牌
app.config['WTF_CSRF_SECRET_KEY'] = 'IT 666 to 709 kill pic'
# 页面
{{ form.csrf_token }}
或者
{{ form.hidden_tag() }}
# 关闭保护
1、全局禁用：app.config中设置WTF_CSRF_ENABLED = False
2、单个表单禁用：生成表单时加入参数form = UserFrom(csrf_enabled=False)
 ```

2、模板页



3、视图函数

web表单是web应用程序的基本功能。

它是HTML页面中负责数据采集的部件。表单有三个部分组成：表单标签、表单域、表单按钮。表单允许用户输入数据，负责HTML页面数据采集，通过表单将用户输入的数据提交给服务器。

在Flask中，为了处理web表单，我们一般使用Flask-WTF扩展，它封装了WTForms，并且它有验证表单数据的功能。

WTForms支持的HTML标准字段

| 字段对象            | 说明                                |
| :------------------ | :---------------------------------- |
| StringField         | 文本字段                            |
| TextAreaField       | 多行文本字段                        |
| PasswordField       | 密码文本字段                        |
| HiddenField         | 隐藏文本字段                        |
| DateField           | 文本字段，值为datetime.date格式     |
| DateTimeField       | 文本字段，值为datetime.datetime格式 |
| IntegerField        | 文本字段，值为整数                  |
| DecimalField        | 文本字段，值为decimal.Decimal       |
| FloatField          | 文本字段，值为浮点数                |
| BooleanField        | 复选框，值为True和False             |
| RadioField          | 一组单选框                          |
| SelectField         | 下拉列表                            |
| SelectMultipleField | 下拉列表，可选择多个值              |
| FileField           | 文本上传字段                        |
| SubmitField         | 表单提交按钮                        |
| FormField           | 把表单作为字段嵌入另一个表单        |
| FieldList           | 一组指定类型的字段                  |

WTForms常用验证函数

| 验证函数     | 说明                                     |
| :----------- | :--------------------------------------- |
| DataRequired | 确保字段中有数据                         |
| EqualTo      | 比较两个字段的值，常用于比较两次密码输入 |
| Length       | 验证输入的字符串长度                     |
| NumberRange  | 验证输入的值在数字范围内                 |
| URL          | 验证URL                                  |
| AnyOf        | 验证输入值在可选列表中                   |
| NoneOf       | 验证输入值不在可选列表中                 |

使用Flask-WTF需要配置参数SECRET_KEY。

CSRF_ENABLED是为了CSRF（跨站请求伪造）保护。 SECRET_KEY用来生成加密令牌，当CSRF激活的时候，该设置会根据设置的密匙生成加密令牌。

在HTML页面中直接写form表单：

## 6.5 控制语句

### 6.5.1 if语句

{% if %} {% endif %}

### 6.5.2 for语句

{% for item in samples %} {% endfor %}

## 6.6 宏

类似于python中的函数，宏的作用就是在模板中重复利用代码，避免**代码冗余**。

### 6.6.1 不带参数宏

1、定义

{% macro input() %}

 <input type="text"

​     name="username"

​     value=""

​     size="30"/>

{% endmacro %}

2、使用

{{ input() }}

 

### 6.6.2 带参数宏

1、定义

{% macro input(name,value='',type='text',size=20) %}

  <input type="{{ type }}"

​      name="{{ name }}"

​      value="{{ value }}"

​      size="{{ size }}"/>

{% endmacro %}

 

2、使用

{{ input(value='name',type='password',size=40)}}

 

### 6.6.3 将宏单独封装

1、文件名可以自定义macro.html

{% macro input() %}

   <input type="text" name="username" placeholde="Username">

  <input type="password" name="password" placeholde="Password">

  <input type="submit">

{% endmacro %}

 

2、在其它模板文件中先导入，再调用

{% import 'macro.html' as func %}

{% func.input() %}

 

## 6.4 模板继承

extend

```python
{% block top %}
顶部菜单
{% endblock top %}

{% block content %}
{% endblock content %}

{% block bottom %}
底部
{% endblock bottom %}
```



```python
{% extends 'base.html' %}
{% block content %}
需要填充的内容
{% endblock content %}
```



## 6.5 模板包含

include

Jinja2模板中，除了宏和继承，还支持一种代码重用的功能，叫包含(Include)。它的功能是将另一个模板整个加载到当前模板中，并直接渲染。

示例：

include的使用

{\% include 'hello.html' %}

包含在使用时，如果包含的模板文件不存在时，程序会抛出TemplateNotFound异常，可以加上ignore missing关键字。如果包含的模板文件不存在，会忽略这条include语句。

示例：

include的使用加上关键字ignore missing

{\% include 'hello.html' ignore missing %}

- 宏、继承、包含：
  - 宏(Macro)、继承(Block)、包含(include)均能实现代码的复用。
  - 继承(Block)的本质是代码替换，一般用来实现多个页面中重复不变的区域。
  - 宏(Macro)的功能类似函数，可以传入参数，需要定义、调用。
  - 包含(include)是直接将目标模板文件整个渲染出来。

## 6.6 特殊变量/方法

flask在模板中使用特殊变量和方法

### 6.6.1 config

 ```python
config 对象就是Flask的config对象，也就是 app.config 对象。

{{ config.SQLALCHEMY_DATABASE_URI }}
 ```



### 6.6.2 request

request常用的属性如下

|  属性   |              说明              |      类型      |
| :-----: | :----------------------------: | :------------: |
|  data   | 记录请求的数据，并转换为字符串 |       *        |
|  form   |      记录请求中的表单数据      |   MultiDict    |
|  args   |      记录请求中的查询参数      |   MultiDict    |
| cookies |     记录请求中的cookie信息     |      Dict      |
| headers |       记录请求中的报文头       | EnvironHeaders |
| method  |     记录请求使用的HTTP方法     |    GET/POST    |
|   url   |       记录请求的URL地址        |     string     |
|  files  |       记录请求上传的文件       |       *        |

```python
{{ request.url }}
```



### 6.6.3 url_for 

 url_for() 会返回传入的路由函数对应的URL，所谓路由函数就是被 app.route() 路由装饰器装饰的函数。如果我们定义的路由函数是带有参数的，则可以将这些参数作为命名参数传入。

```python
{{ url_for('index') }}

{{ url_for('post', post_id=1024) }}
```

get_flashed_messages方法

返回之前在Flask中通过 flash() 传入的信息列表。把字符串对象表示的消息加入到一个消息队列中，然后通过调用 get_flashed_messages() 方法取出。

```python
{% for message in get_flashed_messages() %}
    {{ message }}
{% endfor %}
```

### 6.6.4 闪现

存一次；取一次。

1、后台

```python
from flask import flash

flash("hello")
```

2、页面

```python
for msg in get_flashed_messages()
```



# 7 Flask扩展

## 7.1 Flask-Script

1、安装模块

```shell
pip install Flask-Script
```

2、使用模块

```python
from flask import Flask
from flask_script import Manager

app = Flask(__name__)
app.config.from_pyfile("config.cfg")
manager = Manager(app)


@app.route('/')
def hello_world():
    return 'Hello World!'


if __name__ == '__main__':
    manager.run()

```

3、启动命令

```python
python app.py runserver -h 0.0.0.0 -p 8888
```

4、shell脚本



## 7.2 Flask-SQLalchemy

```properties
# 安装一个flask-sqlalchemy的扩展，模型类转SQL语句，结果转模型类
pip install flask-sqlalchemy
# 要连接mysql数据库，仍需要安装flask-mysqldb，python2中用的是mysql-python
pip install flask-mysqldb
```

使用Flask-SQLAlchemy扩展操作数据库，首先需要建立数据库连接。数据库连接通过URL指定，而且程序使用的数据库必须保存到**Flask配置对象**的SQLALCHEMY_DATABASE_URI键中。

Flask的数据库设置：app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test3'

# 8 数据库 

Web应用中普遍使用的是关系模型的数据库，关系型数据库把所有的数据都存储在表中，表用来给应用的实体建模，表的列数是固定的，行数是可变的。它使用结构化的查询语言。关系型数据库的列定义了表中表示的实体的数据属性。比如：商品表里有name、price、number等。 Flask本身不限定数据库的选择，你可以选择SQL或NOSQL的任何一种。也可以选择更方便的SQLALchemy，类似于Django的ORM。SQLALchemy实际上是对数据库的抽象，让开发者不用直接和SQL语句打交道，而是通过Python对象来操作数据库，在舍弃一些性能开销的同时，换来的是开发效率的较大提升。

SQLAlchemy是一个关系型数据库框架，它提供了高层的ORM和底层的原生数据库的操作。flask-sqlalchemy是一个简化了SQLAlchemy操作的flask扩展。

1、数据库服务

2、数据库基本操作命令

## 8.1 数据库设置

数据库连接配置：

```properties
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/test3'
```

常用的SQLAlchemy字段类型

| 类型名       | python中类型      | 说明                                                |
| ------------ | ----------------- | --------------------------------------------------- |
| Integer      | int               | 普通整数，一般是32位                                |
| SmallInteger | int               | 取值范围小的整数，一般是16位                        |
| BigInteger   | int或long         | 不限制精度的整数                                    |
| Float        | float             | 浮点数                                              |
| Numeric      | decimal.Decimal   | 普通整数，一般是32位                                |
| String       | str               | 变长字符串                                          |
| Text         | str               | 变长字符串，对较长或不限长度的字符串做了优化        |
| Unicode      | unicode           | 变长Unicode字符串                                   |
| UnicodeText  | unicode           | 变长Unicode字符串，对较长或不限长度的字符串做了优化 |
| Boolean      | bool              | 布尔值                                              |
| Date         | datetime.date     | 时间                                                |
| Time         | datetime.datetime | 日期和时间                                          |
| LargeBinary  | str               | 二进制文件                                          |

常用的SQLAlchemy列选项

| 选项名      | 说明                                              |
| ----------- | ------------------------------------------------- |
| primary_key | 如果为True，代表表的主键                          |
| unique      | 如果为True，代表这列不允许出现重复的值            |
| index       | 如果为True，为这列创建索引，提高查询效率          |
| nullable    | 如果为True，允许有空值，如果为False，不允许有空值 |
| default     | 为这列定义默认值                                  |

常用的SQLAlchemy关系选项

| 选项名         | 说明                                                         |
| -------------- | ------------------------------------------------------------ |
| backref        | 在关系的另一模型中添加反向引用                               |
| primary join   | 明确指定两个模型之间使用的联结条件                           |
| uselist        | 如果为False，不使用列表，而使用标量值                        |
| order_by       | 指定关系中记录的排序方式                                     |
| secondary      | 指定多对多中记录的排序方式                                   |
| secondary join | 在SQLAlchemy中无法自行决定时，指定多对多关系中的二级联结条件 |

## 8.2 基础操作

在Flask-SQLAlchemy中，插入、修改、删除操作，均由数据库会话管理。会话用db.session表示。在准备把数据写入数据库前，要先将数据添加到会话中然后调用commit()方法提交会话。

数据库会话是为了保证数据的一致性，避免因部分更新导致数据不一致。提交操作把会话对象全部写入数据库，如果写入过程发生错误，整个会话都会失效。

数据库会话也可以回滚，通过db.session.rollback()方法，实现会话提交数据前的状态。

在Flask-SQLAlchemy中，查询操作是通过query对象操作数据。最基本的查询是返回表中所有数据，可以通过过滤器进行更精确的数据库查询。

1、数据添加到会话中

```python
user = User(name='python')
db.session.add(user)
db.session.commit()
```

2、视图数据定义到模型类中

```python
from flask import Flask
from flask_sqlalchemy import SQLAlchemy


app = Flask(__name__)

# 设置连接数据库的URL
app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@127.0.0.1:3306/Flask_test'

# 设置每次请求结束后会自动提交数据库中的改动，不提倡使用
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
# 模型类和数据库进行同步一直
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
# 查询时会显示原始SQL语句
app.config['SQLALCHEMY_ECHO'] = True
db = SQLAlchemy(app)

class Role(db.Model):
    # 定义表名
    __tablename__ = 'roles'
    __table_args__ = ({'comment': '角色表'})  # 表注释
    # 定义列对象
    id = db.Column(db.Integer, primary_key=True, comment="主键ID")
    name = db.Column(db.String(64), unique=True)
    us = db.relationship('User', backref='role')

    # repr()方法显示一个可读字符串
    def __repr__(self):
        return 'Role:%s'% self.name

class User(db.Model):
    __tablename__ = 'users'
    __table_args__ = ({'comment': '用户表'})  # 表注释
    id = db.Column(db.Integer, primary_key=True, comment="主键ID")
    name = db.Column(db.String(64), unique=True, index=True)
    email = db.Column(db.String(64),unique=True)
    pswd = db.Column(db.String(64))
    role_id = db.Column(db.Integer, db.ForeignKey('roles.id'))

    def __repr__(self):
        return 'User:%s'%self.name
if __name__ == '__main__':
    db.drop_all()
    db.create_all()
    ro1 = Role(name='admin')
    ro2 = Role(name='user')
    db.session.add_all([ro1,ro2])
    db.session.commit()
    us1 = User(name='wang',email='wang@163.com',pswd='123456',role_id=ro1.id)
    us2 = User(name='zhang',email='zhang@189.com',pswd='201512',role_id=ro2.id)
    us3 = User(name='chen',email='chen@126.com',pswd='987654',role_id=ro2.id)
    us4 = User(name='zhou',email='zhou@163.com',pswd='456789',role_id=ro1.id)
    db.session.add_all([us1,us2,us3,us4])
    db.session.commit()
    app.run(debug=True)
```

3、查询操作

```python
User.query.filter_by(name='wang').all() # 名字是wang的所有数据
User.query.first() # 第一条数据
User.query.all() # 全部数据
User.query.filter(User.name.endswith('g')).all() # g结尾的数据
User.query.get() # 指定ID
User.query.filter(User.name!='wang').all() # 不等于wang
User.query.filter(and_(User.name!='wang',User.email.endswith('163.com'))).all() # and
User.query.filter(or_(User.name!='wang',User.email.endswith('163.com'))).all() # or
User.query.filter(not_(User.name=='chen')).all() # 取返
User.query.order_by(User.id.asc).all() # 排序
User.query(role_id,count).group_by(User.role_id).all() # 分组
```

4、关联查询



5、删除

```python
db.session.delete(user);
db.session.commit();
```



6、修改

```python
# 方式一
db.session.add(user);
db.session.commit();
# 方式二
db.session.query().update({"name":"guod"});
```



## 8.3 自定义模板类

定义模型

模型表示程序使用的数据实体，在Flask-SQLAlchemy中，模型一般是Python类，继承自db.Model，db是SQLAlchemy类的实例，代表程序使用的数据库。

类中的属性对应数据库表中的列。id为主键，是由Flask-SQLAlchemy管理。db.Column类构造函数的第一个参数是数据库列和模型属性类型。

```python
#coding=utf-8
from flask import Flask,render_template,url_for,redirect,request
from flask_sqlalchemy import SQLAlchemy
from flask_wtf import FlaskForm
from wtforms.validators import DataRequired
from wtforms import StringField,SubmitField

app = Flask(__name__)

app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql://root:mysql@localhost/test1'
app.config['SQLALCHEMY_COMMIT_ON_TEARDOWN'] = True
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = True
app.config['SECRET_KEY']='s'

db = SQLAlchemy(app)

#创建表单类，用来添加信息
class Append(Form):
    au_info = StringField(validators=[DataRequired()])
    bk_info = StringField(validators=[DataRequired()])
    submit = SubmitField(u'添加')


@app.route('/',methods=['GET','POST'])
def index():
    # 查询所有作者和书名信息
    author = Author.query.all()
    book = Book.query.all()
    # 创建表单对象
    form = Append()
    if form.validate_on_submit():
        # 获取表单输入数据
        wtf_au = form.au_info.data
        wtf_bk = form.bk_info.data
        # 把表单数据存入模型类
        db_au = Author(name=wtf_au)
        db_bk = Book(info=wtf_bk)
        # 提交会话
        db.session.add_all([db_au,db_bk])
        db.session.commit()
        # 添加数据后，再次查询所有作者和书名信息
        author = Author.query.all()
        book = Book.query.all()
        return render_template('index.html',author=author,book=book,form=form)
    else:
        if request.method=='GET':
            render_template('index.html', author=author, book=book,form=form)
    return render_template('index.html',author=author,book=book,form=form)

# 删除作者
@app.route('/delete_author<id>')
def delete_author(id):
    # 精确查询需要删除的作者id
    au = Author.query.filter_by(id=id).first()
    db.session.delete(au)
    # 直接重定向到index视图函数
    return redirect(url_for('index'))

# 删除书名
@app.route('/delete_book<id>')
def delete_book(id):
    # 精确查询需要删除的书名id
    bk = Book.query.filter_by(id=id).first()
    db.session.delete(bk)
    # 直接重定向到index视图函数
    return redirect(url_for('index'))


if __name__ == '__main__':
    db.drop_all()
    db.create_all()
    # 生成数据
    au_xi = Author(name='我吃西红柿',email='xihongshi@163.com')
    au_qian = Author(name='萧潜',email='xiaoqian@126.com')
    au_san = Author(name='唐家三少',email='sanshao@163.com')
    bk_xi = Book(info='吞噬星空',lead='罗峰')
    bk_xi2 = Book(info='寸芒',lead='李杨')
    bk_qian = Book(info='飘渺之旅',lead='李强')
    bk_san = Book(info='冰火魔厨',lead='融念冰')
    # 把数据提交给用户会话
    db.session.add_all([au_xi,au_qian,au_san,bk_xi,bk_xi2,bk_qian,bk_san])
    # 提交会话
    db.session.commit()
    app.run(debug=True)
```



## 8.4 数据库迁移

在开发过程中，需要修改数据库模型，而且还要在修改之后更新数据库。最直接的方式就是删除旧表，但这样会丢失数据。

更好的解决办法是使用数据库迁移框架，它可以追踪数据库模式的变化，然后把变动应用到数据库中。

在Flask中可以使用Flask-Migrate扩展，来实现数据迁移。并且集成到Flask-Script中，所有操作通过命令就能完成。

为了导出数据库迁移命令，Flask-Migrate提供了一个MigrateCommand类，可以附加到flask-script的manager对象上。

首先要在虚拟环境中安装Flask-Migrate。

```python
pip install flask-migrate
```

文件database.py

## 8.5 邮件扩展



```python
from flask import Flask
from flask_mail import Mail, Message

app = Flask(__name__)
#配置邮件：服务器／端口／传输层安全协议／邮箱名／密码
app.config.update(
    DEBUG = True,
    MAIL_SERVER='smtp.qq.com',
    MAIL_PROT=465,
    MAIL_USE_TLS = True,
    MAIL_USERNAME = '371673381@qq.com',
    MAIL_PASSWORD = 'goyubxohbtzfbidd',
)

mail = Mail(app)

@app.route('/')
def index():
 # sender 发送方，recipients 接收方列表
    msg = Message("This is a test ",sender='371673381@qq.com', recipients=['shengjun@itcast.cn','371673381@qq.com'])
    #邮件内容
    msg.body = "Flask test mail"
    #发送邮件
    mail.send(msg)
    print "Mail sent"
    return "Sent　Succeed"

if __name__ == "__main__":
    app.run()
```



# 9 测试

## 9.1 为什么要测试？

Web程序开发过程一般包括以下几个阶段：[需求分析，设计阶段，实现阶段，测试阶段]。其中测试阶段通过人工或自动来运行测试某个系统的功能。目的是检验其是否满足需求，并得出特定的结果，以达到弄清楚预期结果和实际结果之间的差别的最终目的。

## 9.2 测试的分类

测试从软件开发过程可以分为：单元测试、集成测试、系统测试等。在众多的测试中，与程序开发人员最密切的就是单元测试，因为单元测试是由开发人员进行的，而其他测试都由专业的测试人员来完成。所以我们主要学习单元测试。

## 9.3 什么是单元测试

程序开发过程中，写代码是为了实现需求。当我们的代码通过了编译，只是说明它的语法正确，功能能否实现则不能保证。 因此，当我们的某些功能代码完成后，为了检验其是否满足程序的需求。可以通过编写测试代码，模拟程序运行的过程，检验功能代码是否符合预期。

单元测试就是开发者编写一小段代码，检验目标代码的功能是否符合预期。通常情况下，单元测试主要面向一些功能单一的模块进行。

举个例子：一部手机有许多零部件组成，在正式组装一部手机前，手机内部的各个零部件，CPU、内存、电池、摄像头等，都要进行测试，这就是单元测试。

在Web开发过程中，单元测试实际上就是一些“断言”（assert）代码。

断言就是判断一个函数或对象的一个方法所产生的结果是否符合你期望的那个结果。 python中assert断言是声明布尔值为真的判定，如果表达式为假会发生异常。单元测试中，一般使用assert来断言结果。

## 9.4 断言方法的使用

```python
if not expression:
    raise AssertionError
```



```python
assertEqual     如果两个值相等，则pass
assertNotEqual  如果两个值不相等，则pass
assertTrue      判断bool值为True，则pass
assertFalse     判断bool值为False，则pass
assertIsNone    不存在，则pass
assertIsNotNone 存在，则pass
```

## 9.4 如何测试

```python

```



## 9.5 基本写法

1、基本写法

```python
import unittest


class TestMain(unittest.TestCase):
    # 该方法会首先执行，相当于做测试的准备工作
    def setUp(self):
        print("aa")

    # 该方法会在测试代码执行完后执行，相当于做测试后的扫尾工作
    def tearDown(self):
        print("bb")

    def test_app_exists(self):
        print("cc")
```

2、案例写法

```python

```

# 10 蓝图

## 10.1 为什么学习蓝图

我们学习Flask框架，是从写单个文件，执行hello world开始的。我们在这单个文件中可以定义路由、视图函数、定义模型等等。但这显然存在一个问题：随着业务代码的增加，将所有代码都放在单个程序文件中，是非常不合适的。这不仅会让代码阅读变得困难，而且会给后期维护带来麻烦。

我们在一个文件中写入多个路由，这会使代码维护变得困难。

**问题：一个程序执行文件中，功能代码过多。**就是让代码模块化。根据具体不同功能模块的实现，划分成不同的分类，降低各功能模块之间的耦合度。python中的模块制作和导入就是基于实现功能模块的封装的需求。

**尝试用模块导入的方式解决：** 我们把上述一个py文件的多个路由视图函数给拆成两个文件：app.py和admin.py文件。app.py文件作为程序启动文件，因为admin文件没有应用程序实例app，在admin文件中要使用app.route路由装饰器，需要把app.py文件的app导入到admin.py文件中。

1、循环引用，解决方式推迟导入，放入到方法中

2、app.route("get_goods")(get_goods)，进行后补装饰器

3、蓝图方式

## 10.2 什么是蓝图

蓝图：用于实现单个应用的视图、模板、静态文件的集合。

蓝图就是模块化处理的类。

简单来说，蓝图就是一个存储操作路由映射方法的容器，主要用来实现客户端请求和URL相互关联的功能。 在Flask中，使用蓝图可以帮助我们实现模块化应用的功能。

## 10.3 蓝图的运行机制

蓝图是保存了一组将来可以在应用对象上执行的操作。注册路由就是一种操作,当在程序实例上调用route装饰器注册路由时，这个操作将修改对象的url_map路由映射列表。当我们在蓝图对象上调用route装饰器注册路由时，它只是在内部的一个延迟操作记录列表defered_functions中添加了一个项。当执行应用对象的 register_blueprint() 方法时，应用对象从蓝图对象的 defered_functions 列表中取出每一项，即调用应用对象的 add_url_rule() 方法，这将会修改程序实例的路由映射列表。

## 10.4 蓝图使用

1、创建蓝图对象

```python
from flask import Blueprint

# 创建蓝图对象，蓝图就是一个小模块的抽象概念
app_orders = Blueprint("orders", __name__)
```

2、注册蓝图路由

```python
# 定义具体视图函数
@app_orders.route("/order_list")
def order_list():
    return "java"
```

3、在程序实例中注册蓝图

```python
from orders import app_orders
# 注册蓝图
app.register_blueprint(app_orders)
# app.register_blueprint(app_orders, url_prefix="/order")
```

4、如果是**目录模块**

包模块中__init__.py

```python
from flask import Blueprint

app_sys = Blueprint("app_sys", __name__, template_folder="templates", static_folder="static")
# 在__init__.py文件被执行的时候，把视图加载进去，让蓝图与应用程序知道视图的存在
from .sys import app_sys
```



```python
from . import app_sys


@app_sys.route("/list")
def sys():
    return "sys"
```

如果主templates和子templates都有页面，那么就以主templates为主！

# 11 RESTfull

2000年，Roy Thomas Fielding博士在他的博士论文《Architectural Styles and the Design of Network-based Software Architectures》中提出了几种软件应用的架构风格，REST作为其中的一种架构风格在这篇论文中进行了概括性的介绍。

REST:Representational State Transfer的缩写，翻译：“具象状态传输”。一般解释为“表现层状态转换”。

REST是设计风格而不是标准。是指客户端和服务器的交互形式。我们需要关注的重点是如何设计REST风格的网络接口。

- REST的特点：
- 具象的。一般指表现层，要表现的对象就是资源。比如，客户端访问服务器，获取的数据就是资源。比如文字、图片、音视频等。
- 表现：资源的表现形式。txt格式、html格式、json格式、jpg格式等。浏览器通过URL确定资源的位置，但是需要在HTTP请求头中，用Accept和Content-Type字段指定，这两个字段是对资源表现的描述。
- 状态转换：客户端和服务器交互的过程。在这个过程中，一定会有数据和状态的转化，这种转化叫做状态转换。其中，GET表示获取资源，POST表示新建资源，PUT表示更新资源，DELETE表示删除资源。HTTP协议中最常用的就是这四种操作方式。
  - RESTful架构：
  - 每个URL代表一种资源；
  - 客户端和服务器之间，传递这种资源的某种表现层；
  - 客户端通过四个http动词，对服务器资源进行操作，实现表现层状态转换。

## 11.1 域名



## 11.2 版本



## 11.3 路径



# 12 部署

当我们执行下面的hello.py时，使用的flask自带的服务器，完成了web服务的启动。在生产环境中，flask自带的服务器，无法满足性能要求，我们这里采用**Gunicorn**做wsgi容器，来部署flask程序。

Gunicorn（绿色独角兽）是一个Python WSGI的HTTP服务器。从Ruby的独角兽（Unicorn ）项目移植。该Gunicorn服务器与各种Web框架兼容，实现非常简单，轻量级的资源消耗。Gunicorn直接用命令启动，不需要编写配置文件，相对uWSGI要容易很多。

区分几个概念：

**WSGI：**全称是Web Server Gateway Interface（web服务器网关接口），它是一种规范，它是web服务器和web应用程序之间的接口。它的作用就像是桥梁，连接在web服务器和web应用框架之间。

**uwsgi：**是一种传输协议，用于定义传输信息的类型。

**uWSGI：**是实现了uwsgi协议WSGI的web服务器。

我们的部署方式： nginx + gunicorn + flask

gunicorn 并不支持windows，只能在linux 上跑

gunicorn -w 4 -b 127.0.0.1:5000 -D --access-logfile ./log/log main:app

## 12.1 Gunicorn

web开发中，部署方式大致类似。简单来说，前端代理使用Nginx主要是为了实现分流、转发、负载均衡，以及分担服务器的压力。Nginx部署简单，内存消耗少，成本低。Nginx既可以做正向代理，也可以做反向代理。

正向代理：请求经过代理服务器从局域网发出，然后到达互联网上的服务器。

特点：服务端并不知道真正的客户端是谁。

反向代理：请求从互联网发出，先进入代理服务器，再转发给局域网内的服务器。

特点：客户端并不知道真正的服务端是谁。

区别：正向代理的对象是客户端。反向代理的对象是服务端。

1、安装Gunicorn，并不支持windows，**只能在linux 上跑**

```python
pip install gunicorn
```

2、查看命令行选项

安装gunicorn成功后，通过命令行的方式可以查看gunicorn的使用信息。

```python
gunicorn -h
```

3、运行

```python
# 直接运行，默认启动的127.0.0.1::8000
gunicorn 运行文件名称:Flask程序实例名
# 指定进程和端口号：-w: 表示进程（worker）;-b：表示绑定ip地址和端口号（bind）;-D后台运行;运行文件名称:Flask程序实例名
gunicorn -w 4 -b 127.0.0.1:5001 -D --access-logfile ./log/log main:app
```

## 12.2 配置Nginx

```properties
server {
    # 监听80端口
    listen 80;
    # 本机
    server_name localhost; 
    # 默认请求的url
    location / {
        # 请求转发到gunicorn服务器
        proxy_pass http://127.0.0.1:5001; 
        # 设置请求头，并将头信息传递给服务器端 
        proxy_set_header Host $host; 
    }
}
```



# 13 性能

## 13.1 人员角度

普通用户认为的网站性能：网站性能对于普通用户来说，最直接的体现就是响应时间。用户在浏览器上直观感受到的网站响应速度，即从客户端发送请求，到服务器返回响应内容的时间。

做为网站开发人员来说，网站性能通常会和普通的用户理解的不一样。

普通用户感受到的网站性能，并不只是由网站服务器决定的。它还包括客户端计算机和服务器通信的时间，网站服务器处理响应的时间，客户端浏览器构造请求解析响应数据的时间。甚至，不同的计算机性能、不同浏览器解析HTML的速度、不同网络运营商提供的网络带宽房屋的差异，这些都会导致用户感受到响应时间，可能大于网站服务器处理请求的时间。

开发人员认为的网站性能：开发人员关注的主要是服务器应用程序本身，以及相关配套系统的性能。包括并发处理能力、系统稳定性、响应延迟等技术指标。

对性能优化的主要手段，包括使用缓存加速数据读取，使用集群提高数据吞吐能力，使用异步消息加快请求响应，使用代码改善程序性能。

运维人员认为的网站性能：运维人员关注的主要是服务器基础设施和资源利用率。如服务器硬件的配置、网络运营商的带宽、数据中心的网络架构等。主要优化手段有使用高性价比的服务器、建设优化骨干网络、利用虚拟化技术优化资源利用等。
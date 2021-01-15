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

# 4 创建虚拟环境

## 4.1 Virtual

虚拟环境是一个互相隔离的目录

1、mkvirtualenv flask_py2

2、pip install flask==0.10.1

 

pip freeze > requirements.txt

pip install –r requirements.txt

## 4.2 Conda

```py
conda -V
conda create -n py2.6 python=2.6
conda activate py2.6
activate py2.6
deactivate
```

# 5 视图

```python
# 导入Flask类
from flask import Flask

# Flask类接收一个参数__name__表示当前模块的名字。模块名，flask是以这个模块所在的目录为总目录，默认情况这个目录中的static为静态目录，templates为模板目录
app = Flask(__name__)


# 装饰器的作用是将路由映射到视图函数index
@app.route('/')
def hello_world():
    return 'Hello World!'


@app.route('/java')
def java():
    return "java", 200


# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    app.run()

```



## 5.1 app对象

### 5.1.1 初始化参数

```python
import_name: 

static_url_path:

static_folder: 默认‘static’

template_folder: 默认‘templates’
```

### 5.1.2 配置参数

```python
app.config.from_pyfile(“yourconfig.cfg”) 

或

app.config.from_object()
```

### 5.1.3 在视图读取配置参数

```python
app.config.get() 
或 
current_app.config.get()
```

### 5.1.4 app.run的参数

```python
app.run(host=”0.0.0.0”, port=5000)
```

### 5.1.5 应用配置

在Django中，有一个程序的配置文件settings.py，但是在flask中并没有settings.py这个文件，不过不必担心，flask提供了3种应用配置的方式，分别如下：

- app.config.from_pyfile(file)：使用配置文件
- app.config.from_object(obj)：使用对象配置参数
- app.config：直接操作全局对象

1、文件配置

```python
DEBUG = True
SQLALCHEMY_DATABASE_URI = mysql://root:passw0rd@127.0.0.1:3306/test
SQLALCHEMY_COMMIT_ON_TEARDOWN = True
SQLALCHEMY_TRACK_MODIFICATIONS = True
SQLALCHEMY_ECHO = True

# 文件名: 配置文件一般是.cfg结尾
app.config.from_pyfile("config.cfg")
```

2、对象配置

```python
class Config(object):
    DEBUG = True
    ITCAST = 'PYTHON'

app.config.from_object(Config)
```

3、直接配置

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



```python
# 导入Flask类
from flask import Flask, render_template, request, jsonify
from flask_sqlalchemy import SQLAlchemy
import json

# Flask类接收一个参数__name__
app = Flask(__name__)
app.config.from_pyfile("config.cfg")
db = SQLAlchemy(app)
db.init_app(app)


class Person(db.Model):
    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    username = db.Column(db.String(32), unique=True)
    password = db.Column(db.String(32))
    nickname = db.Column(db.String(32), default="", comment="信息")
    age = db.Column(db.Integer, default=18)
    gender = db.Column(db.String(16), default="男")
    score = db.Column(db.Float, nullable=True)


db.create_all()


# 装饰器的作用是将路由映射到视图函数index
@app.route('/')
def hello_world():
    return render_template("demo.html")


@app.route('/hello', methods=["GET"])
def hello():
    return 'hello1'


@app.route('/hello', methods=['POST'])
def hello2():
    return 'hello2'


@app.route('/hi1')
@app.route('/hi2')
def hi():
    return 'hi page'


@app.route('/java/<userId>', methods=['GET'])
def java(userId):
    print(userId)
    return "java", 200


@app.route("/index", methods=["GET", "POST"])
def index():
    # request中包含了前端发送过来的所有数据
    # 通过request.form可以直接提取请求体中表单格式的数据，是一个类
    name = request.form.get("name", "zhangsan")
    age = request.form.get('age')
    print(request.form)
    return "hello name=%s, age=%s " % (name, age)


@app.route('/json1')
def json1():
    # json就是字符串
    data = {
        "name": "ah",
        "age": 24
    }

    # json.dumps(字典) 将python的字典转换为json字符串
    # json.loads(字符串) 将字符串转换为python中的字典

    json_str = json.dumps(data)
    #
    # return json_str, 200, {'Content-Type': 'application/json'}

    # jsonify帮助转为json数据，并设置响应头 Content-Type 为 application/json
    return jsonify(data)
    # return jsonify(city='sz', country='china')


# 定义错误的处理方法
@app.errorhandler(404)
def handle_404_error(err):
    """自定义处理错误方法"""
    # 这个函数的返回值会是前端用户看到的最终结果

    return "出现了404错误，错误信息: %s" % err


# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    print(app.url_map)
    app.run()

```



## 5.2 路由

### 5.2.1 app.url_map查看所有路由

 

### 5.2.2 同一路由装饰多个视图函数

 

### 5.2.3 同一视图多个路由装饰器

 

### 5.2.4 利用methods限制访问方式

@app.route('/sample', methods=['GET', 'POST'])

### 5.2.5 使用url_for进行反解析

 

### 5.2.6 动态路由



### 5.2.7 自定义转换器



## 5.3 请求参数

```python
from flask import request
```

就是Flask中表示当前请求的request对象，request对象中保存了一次HTTP请求的一切信息。request常用属性如下:

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

```



### 5.3.4 上传文件

request.files

已上传的文件存储在内存或是文件系统中一个临时的位置。你可以通过请求对象的 files 属性访问它们。每个上传的文件都会存储在这个字典里。它表现近乎为一个标准的 Python file 对象，但它还有一个 save() 方法，这个方法允许你把文件保存到服务器的文件系统上。这里是一个用它保存文件的例子:

 



如果你想知道上传前文件在客户端的文件名是什么，你可以访问 filename 属性。但请记住， 永远不要信任这个值，这个值是可以伪造的。如果你要把文件按客户端提供的文件名存储在服务器上，那么请把它传递给 Werkzeug 提供的 secure_filename() 函数:



## 5.4 异常处理

### 5.4.1 abort函数

from flask import abort

### 5.4.2 自定义异常处理

```python
@app.errorhandler(404)

def error(e):

return '您请求的页面不存在了，请确认后再次访问！%s'%e
```

## 5.5 响应数据 

### 5.5.1 元组

可以返回一个元组，这样的元组必须是 **(response, status, headers)** 的形式，且至少包含一个元素。 status 值会覆盖状态代码， headers可以是一个列表或字典，作为额外的消息标头值。

### 5.5.2 make_response

resp = make_response()

resp.headers[“sample”] = “value”

resp.status = “404 not found”

### 5.5.3 Json数据

 

### 5.5.4 重定向

from flask import redirect

 

## 5.6 cookie

### 设置和读取

make_response

 

set_cookie(key, value=’’, max_age=None)

 

delete_cookie(key)

## 5.7 session

### 设置和读取

from flask import session

 

需要设置secret_key

 

## 5.8 上下文

###  请求上下文与应用上下文

请求上下文(request context) 

request和session都属于请求上下文对象。

 

应用上下文(application context)

current_app和g都属于应用上下文对象。

 

current_app:表示当前运行程序文件的程序实例。

g:处理请求时，用于临时存储的对象，每次请求都会重设这个变量。

 

## 5.9 请求钩子

请求钩子是通过装饰器的形式实现，Flask支持如下四种请求钩子：

 

before_first_request：在处理第一个请求前运行。

 

@app.before_first_request

 

before_request：在每次请求前运行。

 

after_request(response)：如果没有未处理的异常抛出，在每次请求后运行。

 

teardown_request(response)：在每次请求后运行，即使有未处理的异常抛出。

 

# 6 Flask扩展

## 5.1 Flask-Script

pip install Flask-Script



## 5.2 Flask-SQLalchemy

```properties
pip install flask-sqlalchemy

pip install flask-mysqldb
```



# 7 Jinja2模板

flask本身没有加载模板的能力，需要开发者自己编写，但是目前flask使用了三方的模板系统：jinja2模板系统，jinja2是仿照django的模板系统开发的一套模板系统，但是功能比django的模板系统强。

## 6.1 基本流程

使用flask中的**render_template**渲染模板

## 6.2 变量



## 6.3 过滤器

### 6.3.1 字符串过滤器

**safe****：禁用转义；**

  <p>{{ '<em>hello</em>' | safe }}</p>
**capitalize****：把变量值的首字母转成大写，其余字母转小写；**

  <p>{{ 'hello' | capitalize }}</p>
**lower****：把值转成小写；**

  <p>{{ 'HELLO' | lower }}</p>
**upper****：把值转成大写；**

  <p>{{ 'hello' | upper }}</p>
**title****：把值中的每个单词的首字母都转成大写；**

  <p>{{ 'hello' | title }}</p>
**trim****：把值的首尾空格去掉；**

  <p>{{ ' hello world ' | trim }}</p>
**reverse:****字符串反转；**

  <p>{{ 'olleh' | reverse }}</p>
**format:****格式化输出；**

  <p>{{ '%s is %d' | format('name',17) }}</p>
**striptags****：渲染之前把值中所有的HTML****标签都删掉；**

  <p>{{ '<em>hello</em>' | striptags }}</p>
### 6.3.2 支持链式使用过滤器

<p>{{ “ hello world  “ | trim | upper }}</p>
### 6.3.3 列表过滤器

**first****：取第一个元素**

  <p>{{ [1,2,3,4,5,6] | first }}</p>
**last****：取最后一个元素**

  <p>{{ [1,2,3,4,5,6] | last }}</p>
**length****：获取列表长度**

  <p>{{ [1,2,3,4,5,6] | length }}</p>
**sum****：列表求和**

  <p>{{ [1,2,3,4,5,6] | sum }}</p>
**sort****：列表排序**

  <p>{{ [6,2,3,1,5,4] | sort }}</p>
### 6.3.4 自定义过滤器

自定义的过滤器名称如果和内置的过滤器重名，会覆盖内置的过滤器。

 

方式一：

​     通过 **add_template_filter (****过滤器函数****,** **模板中使用的过滤器名字****)**

![文本框: def filter_double_sort(../../../../Java-HandBook/插图/clip_image015.gif):     return ls[::2] app.add_template_filter(filter_double_sort,'double_2')  ](file:///C:/Users/DELL/AppData/Local/Temp/msohtmlclip1/01/clip_image015.gif)

 

方式二：

​     通过装饰器 **app.template_filter (****模板中使用的装饰器名字)**

![文本框: @app.template_filter(../../../../Java-HandBook/插图/clip_image016.gif'db3') def filter_double_sort(ls):     return ls[::-3] ](file:///C:/Users/DELL/AppData/Local/Temp/msohtmlclip1/01/clip_image016.gif)

## 6.4 表单

使用Flask-WTF表单扩展，可以帮助进行CSRF验证，帮助我们快速定义表单模板，而且可以帮助我们在视图中验证表的数据

 

**pip install Flask-WTF**

### 6.4.1 不使用Flask-WTF扩展时，表单需要自己处理

![文本框: #模板文件 <form method='post'>     <input type="text" name="username" placeholder='Username'>     <input type="password" name="password" placeholder='password'>     <input type="submit"> </form> ](../../../../Java-HandBook/插图/clip_image017.gif)![文本框: from flask import Flask,render_template,request  @app.route('/login',methods=['GET','POST']) def login():     if request.method == 'POST':         username = request.form['username']         password = request.form['password']         print username,password     	return “success” 	else: 		return render_template(“login.html”)  ](file:///C:/Users/DELL/AppData/Local/Temp/msohtmlclip1/01/clip_image018.gif)

### 6.4.2 使用Flask-WTF扩展

需要设置 SECRET_KEY 的配置参数

 

模板页：

![文本框: <form method="post">         #设置csrf_token         {{ form.csrf_token(../../../../Java-HandBook/插图/clip_image019.gif) }}         {{ form.us.label }}         <p>{{ form.us }}</p>         {{ form.ps.label }}         <p>{{ form.ps }}</p>         {{ form.ps2.label }}         <p>{{ form.ps2 }}</p>         <p>{{ form.submit() }}</p>         {% for x in get_flashed_messages() %}             {{ x }}         {% endfor %}  </form> ](file:///C:/Users/DELL/AppData/Local/Temp/msohtmlclip1/01/clip_image019.gif)

 

视图函数

![文本框: rf#coding=utf-8 from flask import Flask,render_template, redirect,url_for,session,request,flash  #导入wtf扩展的表单类 from flask_wtf import FlaskForm #导入自定义表单需要的字段 from wtforms import SubmitField,StringField,PasswordField #导入wtf扩展提供的表单验证器 from wtforms.validators import DataRequired,EqualTo app = Flask(../../../../Java-HandBook/插图/clip_image020.gif) app.config['SECRET_KEY']='1'  #自定义表单类，文本字段、密码字段、提交按钮 class Login(Flask Form):     us = StringField(label=u'用户：',validators=[DataRequired()])     ps = PasswordField(label=u'密码',validators=[DataRequired(),EqualTo('ps2','err')])     ps2 = PasswordField(label=u'确认密码',validators=[DataRequired()])     submit = SubmitField(u'提交')  #定义根路由视图函数，生成表单对象，获取表单数据，进行表单数据验证 @app.route('/',methods=['GET','POST']) def index():     form = Login()     if form.validate_on_submit():         name = form.us.data         pswd = form.ps.data         pswd2 = form.ps2.data         print name,pswd,pswd2         return redirect(url_for('login'))     else:         if request.method=='POST': flash(u'信息有误，请重新输入！')      return render_template('index.html',form=form) if __name__ == '__main__':     app.run(debug=True)  ](file:///C:/Users/DELL/AppData/Local/Temp/msohtmlclip1/01/clip_image020.gif)

## 6.5 控制语句

### 6.5.1 if语句

{% if %} {% endif %}

### 6.5.2 for语句

{% for item in samples %} {% endfor %}

 

## 6.6 宏

类似于python中的函数，宏的作用就是在模板中重复利用代码，避免代码冗余。

 

### 6.6.1 不带参数宏的定义与使用

**定义：**

{% macro input() %}

 <input type="text"

​     name="username"

​     value=""

​     size="30"/>

{% endmacro %}

 

**使用**

{{ input() }}

 

### 6.6.2 带参数宏的定义与使用

定义

{% macro input(name,value='',type='text',size=20) %}

  <input type="{{ type }}"

​      name="{{ name }}"

​      value="{{ value }}"

​      size="{{ size }}"/>

{% endmacro %}

 

使用

{{ input(value='name',type='password',size=40)}}

 

### 6.6.3 将宏单独封装在html文件中

 

文件名可以自定义macro.html

 

{% macro input() %}

 

  <input type="text" name="username" placeholde="Username">

  <input type="password" name="password" placeholde="Password">

  <input type="submit">

{% endmacro %}

 

在其它模板文件中先导入，再调用

 

{% import 'macro.html' as func %}

{% func.input() %}

 

## 6.4 模板继承

extend

## 6.5 模板包含

include

 

## 6.6 flask在模板中使用特殊变量和方法

### 6.6.1 config

 

### 6.6.2 request

 

### 6.6.3 url_for 

 

# 8 数据库 





# 9 测试





# 10 部署

gunicorn 并不支持windows，只能在linux 上跑

gunicorn -w 4 -b 127.0.0.1:5000 -D --access-logfile ./log/log main:app
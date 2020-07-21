# 移动App第5天-豆瓣电影

## Node.js设置跨域

```

app.use('*', function (req, res, next) {

	// 设置请求头为允许跨域

    res.header("Access-Control-Allow-Origin", "*");

    // 设置服务器支持的所有头信息字段

    res.header("Access-Control-Allow-Headers", "Content-Type,Content-Length, Authorization, Accept,X-Requested-With");

    // 设置服务器支持的所有跨域请求的方法

    res.header("Access-Control-Allow-Methods", "POST,GET");

    // next()方法表示进入下一个路由

    next();

});

```



## Promise规范

1. 定义：就是一个异步的代码规范；

2. 好处：

 + 更好的帮我们解决回调地狱问题

 + 能帮我们很好的实现代码的复用



## 基于Promise规范的fetch API的使用



## 项目结构搭建和布局

1. 运行`npm install antd --save`安装ant design

2. 导入相关组件：

```

import { DatePicker } from 'antd';

```

3. 导入样式：

```

import 'antd/dist/antd.css';

```



### 实现ANT组件的按需加载

1. 运行`cnpm i babel-plugin-import --save-dev`

2. 修改`.babelrc`文件：

```

{

    "presets":["es2015", "stage-0", "react"],

    "plugins":[

        "transform-runtime",

        ["import", { "libraryName": "antd", "style": "css" }]

        ]

}

```

3. 然后只需从 antd 引入模块即可，**无需单独引入样式**。等同于下面手动引入的方式。



## 使用react-router-dom实现路由跳转

+ HashRouter：是一个路由的跟容器，一个应用程序中，一般只需要唯一的一个HashRouter容器即可！将来，所有的Route和Link都要在HashRouter中进行使用

 - 注意：HashRouter中，只能有唯一的一个子元素

+ Link：是相当于超链接一般的存在；点击Link，跳转到相应的路由页面！负责进行路由地址的切换！

+ Route：是路由匹配规则，当路由地址发生切换的时候，就会来匹配这些定义好的Route规则，如果有能匹配到的路由规则，那么，就会展示当前路由规则所对应的页面！

+ Route：除了是一个匹配规则之外，还是一个占位符，将来，此Route所匹配到的组件页面，将会展示到Route所在的这个位置！

```

// 其中path指定了路由匹配规则，component指定了当前规则所对应的组件

<Route path="" component={}></Route>

```

+ 注意：react-router中的路由匹配，是进行模糊匹配的！可以通过`Route`身上的`exact`属性，来表示当前的`Route`是进行精确匹配的

+ 可以使用`Redirect`实现路由重定向

```

    // 导入路由组件

    import {Route, Link, Redirect} from 'react-router-dom'



    <Redirect to="/movie/in_theaters"></Redirect>

```



## this.prop和Route的关系【重要】



## 获取到参数之后，从服务器获取电影数据



## 使用Node服务器转接豆瓣API



## 渲染电影列表



## 相关文章

+ [ANT DESIGN 一个 UI 设计语言](https://ant.design/index-cn)

+ [react-router-dom](https://reacttraining.com/react-router/web/guides/quick-start)

+ [豆瓣电影API地址](https://developers.douban.com/wiki/?title=api_v2)

 - [正在热映 - in_theaters](https://api.douban.com/v2/movie/in_theaters)

 - [即将上映 - coming_soon](https://api.douban.com/v2/movie/coming_soon)

 - [top250](https://api.douban.com/v2/movie/top250)

 - [电影详细信息 - subject](https://api.douban.com/v2/movie/subject/26309788)

+ [跨域资源共享 CORS 详解 - 阮一峰](http://www.ruanyifeng.com/blog/2016/04/cors.html)

+ [Request - Simplified HTTP client](https://github.com/request/request)

+ [CSS3 transform 属性](http://www.w3school.com.cn/cssref/pr_transform.asp)

+ [ES6 - Promise规范 - 阮一峰](http://es6.ruanyifeng.com/#docs/promise)

+ [刘龙彬 - 博客园 - Javascript中Promise的简单使用](http://www.cnblogs.com/liulongbinblogs/p/6731288.html)

+ [Javascript 中的神器——Promise](http://www.jianshu.com/p/063f7e490e9a)

+ [MDN - Fetch API](https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API)

+ [MDN - Response](https://developer.mozilla.org/zh-CN/docs/Web/API/Response)

+ [fetch-jsonp - 支持JSONP的Fetch实现](https://www.npmjs.com/package/fetch-jsonp)
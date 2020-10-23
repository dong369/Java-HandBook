1 前言
==

在做 vue 中大型项目的时候，官方推荐使用 axios，但是原生的 axios 可能对项目的适配不友好，所以，在工程开始的来封装一下 axios，保持全项目数据处理的统一性。此文主要讲在 vue-cil 项目中如何封装 axios，封装请求，封装公共的 api，页面如何调用请求。

2 前期配置
=============

新建 vue 项目，下载 axios，并在 main.js 中导入 axios

```js
npm i axios -S
```

```js
// main.js
import axios from "axios";
```

3 代理地址
====================

代理的作用是：**把请求代理转发到其他服务器的中间件**；

例如：我们当前主机为`http://localhost:8080/`，现在我们有一个需求，如果我们请求`/api`，我们不希望由3000来处理这个请求，而希望由另一台服务器`https://www.example.org/api`来处理这个请求怎么办？

这时候就要使用代理来解决！

在项目 config 目录下的修改 index.js 文件，这里是主要书写配置多个后台接口。

## 3.1 旧版本

vue cil2 旧版本的代理配置——config/index.js

```js
module.exports = {
    dev: {
        proxyTable: {
            // 第一个代理
            '/api-test': {
                target: 'https://www.example.org:', /// 目标服务器 host
                ws: true, //是否启用websocket
                secure: true, // 如果是https接口，需要配置这个参数
                changeOrigin: true, // // 默认false，是否需要改变原始主机头为目标URL，是否跨域
                pathRewrite: {
                    '^/api-test/old': '/api-test/new' // 重写请求，比如我们源访问的是api-test/old，那么请求会被解析为/api-test/new
                }
            },
            // 第二个代理
            '/api-else': {
                target: 'https://197.32.22.33:8090',
                ws: true, //是否启用websocket
                secure: true,
                changeOrigin: true,
                pathRewrite: {
                    '^/api-else': '' //默认写法，如果不写pathRewrite就是默认为空；
                }
            },
            // 第三个代理
            '/api-three': {
                target: 'https://197.32.22.33:9090',
                changeOrigin: true,
                pathRewrite: {
                    '^/api-three': '/api-three' //重写请求，这样本地请求不会有两次/api-three
                }
            }
        }
    }
}
```

## 3.2 新版本

vuecil 3+ 新版本的代理配置–vue.config.js 文件

关于多代理配置：

```js
module.exports = {
  lintOnSave: false,
  devServer: {
    overlay: { // 让浏览器 overlay 同时显示警告和错误
      warnings: true,
      errors: true
    },
    // host: "127.0.0.1",
    port: 8080, // 端口号
    https: false, // https:{type:Boolean}
    open: true, //配置后自动启动浏览器
    hotOnly: true, // 热更新
    // proxy: 'http://localhost:8080'   // 配置跨域处理,只有一个代理
    proxy: { //配置多个跨域
      "/api": {
        target: "http://127.0.0.1:8088",
        changeOrigin: true,
        ws: true, //websocket支持
        secure: false,
        pathRewrite: {
          "^/api": "/"
        }
      },
      "/demo": {
        target: "http://127.0.0.1:8088",
        changeOrigin: true,
        secure: false,
        pathRewrite: {
          "^/demo": "/"
        }
      },
    }
  }

}
```

其中 vue.config.js 详细参考我另一篇博客：[vue 项目升级 (01)：全面解析 vuecil3/vuecil4 的 vue.config.js 等常用配置](https://blog.csdn.net/weixin_43216105/article/details/106763258)

如果有多后台，就可以在 api 文件夹下另外新建一个 elseApi.js ，书写当前 ip 下的接口请求。方法同上，只是 `let resquest = "/elseIp/request/"` 调用的时候把端口更改一下。

## 3.3 代理规则

1、在浏览器或postman中测试接口是否正常访问；（最重要！！不然改半天都没用）
那怎么才是成功的访问呢？
例如：拿第二个代理举例：你要访问的接口为`https://197.32.22.33:8090/api-else/getsomething.json`，在浏览器直接输入有返回值并测试无误后再进行下一步；

2、要确保书写方式完全正确！

- **2.1（参考写法2）** 这时候你本地去请求的接口地址会变成：`http://localhost:8080/api-else/api-else/getsomething.json`才是正常的！
  是不是会好奇为什么会有两个`/api-else`，因为在本地：`http://localhost:8080/api-else`相当于：`https://197.32.22.33:8090`，这才是正常的！

- **2.2 (参考写法3)**
  在按上述写法还是有误的情况下，可以参考写法3的路由去更改测试。例：你要访问的接口为`https://197.32.22.33:9090/api-three/getThreething.json`，本地配置后：`http://localhost:8080/api-three/getThreething.json`。

  多说一句，为什么要提第三种写法？

  这种适用于前后端分离多后台项目，后台项目的包名为：api-three，使用第2中写法，在打包之后部署生产环境会出现请求的问题（我们自己项目踩过的坑，偶发），所以之后规定代理和后台包名统一，并且不能直接写在请求中，而是在配置代理后，重写代理的请求，指向包名；

3、**请修改完config的index.js后，答应我一定重启下项目`npm run dev`；**

4、**也是我这次bug的原因（正经脸，这个超级细微！）**
**在配置多个代理的情况下，代理名称不能相同，也不能出现重叠的情况！**

## 3.4 代理失效

```js
 proxyTable: {
    	// 第一个代理
      '/api-test': {  
        target: 'https://www.example.org:', /// 目标服务器 host
        },
        //第二个代理
      '/api-testAAA': {  
        target: 'https://197.32.22.33:8090', 
}
```

是的！他用的是`indexOf() === 0`来判断的！！！所以如果你的多个代理重叠`/api-testAAA`和`/api-test`这样出现的话，他们是都会返回`true`的！
但是`/api-test`更快判断完，**所以`/api-testAAA`就失效了**！！！

```js
function matchSingleStringPath(context, uri) {
  const pathname = getUrlPathName(uri);
  return pathname.indexOf(context) === 0;
}
```

4 封装axios
===========================

在项目 src 目录下新建 utils 文件夹，然后在其中新建 request.js 文件，这个文件是主要书写 axios 的封装过程。

```js
/****   request.js   ****/
// 导入axios
import axios from 'axios'
// 使用element-ui Message做消息提醒
import {
  Message
} from 'element-ui';
//1. 创建新的axios实例，
const service = axios.create({
  // 公共接口--这里注意后面会讲
  baseURL: process.env.BASE_API,
  // 超时时间 单位是ms，这里设置了3s的超时时间
  timeout: 3 * 1000
})
// 2.请求拦截器
service.interceptors.request.use(config => {
  //发请求前做的一些处理，数据转化，配置请求头，设置token,设置loading等，根据需求去添加
  config.data = JSON.stringify(config.data); //数据转化,也可以使用qs转换
  config.headers = {
    'Content-Type': 'application/x-www-form-urlencoded' //配置请求头
  }
  // 注意使用token的时候需要引入cookie方法或者用本地localStorage等方法，推荐js-cookie
  // const token = getCookie('名称'); 这里取token之前，你肯定需要先拿到token,存一下
  // if (token) {
  //   config.params = {
  //     'token': token
  //   } //如果要求携带在参数中
  //   config.headers.token = token; //如果要求携带在请求头中
  // }
  return config
}, error => {
  Promise.reject(error)
})

// 3.响应拦截器
service.interceptors.response.use(response => {
  //接收到响应数据并成功后的一些共有的处理，关闭loading等

  return response
}, error => {
  /***** 接收到异常响应的处理开始 *****/
  if (error && error.response) {
    // 1.公共错误处理
    // 2.根据响应码具体处理
    switch (error.response.status) {
      case 400:
        error.message = '错误请求'
        break;
      case 401:
        error.message = '未授权，请重新登录'
        break;
      case 403:
        error.message = '拒绝访问'
        break;
      case 404:
        error.message = '请求错误,未找到该资源'
        window.location.href = "/NotFound"
        break;
      case 405:
        error.message = '请求方法未允许'
        break;
      case 408:
        error.message = '请求超时'
        break;
      case 500:
        error.message = '服务器端出错'
        break;
      case 501:
        error.message = '网络未实现'
        break;
      case 502:
        error.message = '网络错误'
        break;
      case 503:
        error.message = '服务不可用'
        break;
      case 504:
        error.message = '网络超时'
        break;
      case 505:
        error.message = 'http版本不支持该请求'
        break;
      default:
        error.message = `连接错误${error.response.status}`
    }
  } else {
    // 超时处理
    if (JSON.stringify(error).includes('timeout')) {
      Message.error('服务器响应超时，请刷新当前页')
    }
    error.message('连接服务器失败')
  }

  Message.error(error.message)
  /***** 处理结束 *****/
  //如果不需要错误处理，以上的处理过程都可省略
  return Promise.resolve(error.response)
})
//4.导入文件
export default service
```

**特殊说明：**

> 鉴于有很多朋友问关于数据转换这块的问题，特在页面中单独回复一下！
> 
> ```
> config.data = JSON.stringify(config.data);
> config.headers = { 'Content-Type':'application/x-www-form-urlencoded'  }
> const token = getCookie('名称')
> if(token){ 
>   config.params = {'token':token} ; 
>   config.headers.token= token; 
> }
> 
> ```
> 
> 上述的代码都是请求的配置项，非必须，也是分情况的，`data/headers /params` 这种本身的参数都有多种，和后台沟通，需要什么就配什么！  
> `config.data = JSON.stringify(config.data);`为什么不用`qs.stringify`，因为我的后台想要的只是 json 类型的传参，而 qs 转换会转换成为键值对拼接的字符串形式。当然你们后台需要传递字符串类型参数，那就换成 qs 或者其他格式方式。  
> `const token = getCookie('名称')`这是 token 的取值，在取之前你肯定需要发请求拿到 token，然后 setCookie 存起来，而名称就是你存的 token 的名称，每个人的不一样；  
> `config.headers = { 'Content-Type':'application/x-www-form-urlencoded' }`请求头内容的配置，也是不同的，`application/x-www-form-urlencoded ：`form 表单数据被编码为 key/value 格式发送到服务器（表单默认的提交数据的格式），你可以根据实际情况去配置自己需要的；  
> 以上  
> 我已经举了很清晰的例子，写代码的过程是自己动脑去搭建工程的，希望能看到我文章的各位，善于搜索，善于思考，善于总结；  
> 当然我很喜欢帮大家解决问题，但是相关的基础问题，还是建议自己去学习掌握。

5 封装请求
===============

在项目 src 目录下的 utils 文件夹中新建 http.js 文件，这个文件是主要书写几种请求的封装过程。

```js
/****   http.js   ****/
// 导入封装好的axios实例
import request from './request'

const http = {
  /**
   * methods: 请求
   * @param url 请求地址 
   * @param params 请求参数
   */
  get(url, params) {
    const config = {
      method: 'get',
      url: url
    }
    if (params) config.params = params
    return request(config)
  },
  post(url, params) {
    const config = {
      method: 'post',
      url: url
    }
    if (params) config.data = params
    return request(config)
  },
  put(url, params) {
    const config = {
      method: 'put',
      url: url
    }
    if (params) config.params = params
    return request(config)
  },
  delete(url, params) {
    const config = {
      method: 'delete',
      url: url
    }
    if (params) config.params = params
    return request(config)
  }
}
//导出
export default http
```

6 封装 API
=========================

用于发送请求——api.js

在项目 src 目录下新建 api 文件夹，然后在其中新建 api.js 文件，这个文件是主要书写 API 的封装过程。

写法一：适合分类导出

```js
import http from '../utils/http'
// 
/**
 *  @parms resquest 请求地址 例如：http://197.82.15.15:8088/request/...
 *  @param '/testIp'代表vue-cil中config，index.js中配置的代理
 */
let resquest = "/demo"

// get请求
export function getListAPI(params) {
  return http.get(`${resquest}/getUser`, params)
}
// post请求
export function postFormAPI(params) {
  return http.post(`${resquest}/postForm.json`, params)
}
// put 请求
export function putSomeAPI(params) {
  return http.put(`${resquest}/putSome.json`, params)
}
// delete 请求
export function deleteListAPI(params) {
  return http.delete(`${resquest}/deleteList.json`, params)
}
```

写法二：使用全部导出

```js
import http from '../utils/http'
// 
/\*\*
 \*  @parms resquest 请求地址 例如：http://197.82.15.15:8088/request/...
 \*  @param '/testIp'代表vue-cil中config，index.js中配置的代理
 \*/
let resquest = "/testIp/request/"

// get请求
export default{
 	getListAPI(params){
    	return http.get(\`${resquest}/getList.json\`,params)
	},
	 postFormAPI(params){
    	return http.post(\`${resquest}/postForm.json\`,params)
	}
}
```

注意：一个项目中如果后台请求不是同一个 ip，而是多个 ip 的时候，可以在 api 文件夹下建立多个 js，用来调用请求。 
我们看下之前遗留的一个问题：

```
// 创建新的axios实例，
const service = axios.create({
  baseURL: process.env.BASE\_API,
  timeout: 3 \* 1000
})
```

在之前封装公共接口的 baseUrl 时候，用了`webpack`中的全局变量`process.env.BASE_API`，而不是直接写死 ip，也是为了适应多个后台或者开发的时候的 api 地址和发布的时候的 api 地址不一样这种情况。

**以上 关于配置环境 和接口 基本搭建完毕，下面看一下调用：**

7 文件中调用
===============

方法一：用到哪个 api 就调用哪个接口——适用于上文接口分类导出；

```
 import {getListAPI,postFormAPI, putSomeAPI, deleteListAPI} from '@/api/api'

  methods: {
      //promise调用，链式调用， getList()括号内只接受参数；
      //   get不传参
      getList() {
        getListAPI().then(res => console.log(res)).catch(err => console.log(err))
      },
		//post传参
      postForm(formData) {
        let data = formData
        postFormAPI(data).then(res => console.log(res)).catch(err => console.log(err))
      },

      //async await同步调用
      async postForm(formData) {
        const postRes = await postFormAPI(formData)
        const putRes = await putSomeAPI({data: 'putTest'})
        const deleteRes = await deleteListAPI(formData.name)
        // 数据处理
        console.log(postRes);
        console.log(putRes);
        console.log(deleteRes);
      },
   }

```

方法二 ：把 api 全部导入，然后用哪个调用哪个 api——适用于全部导出

```
   import api from '@/api/api'
   methods: {
     getList() {
        api.getListAPI(data).then(res => {
          //数据处理
        }).catch(err => console.log(err))
      }
    }  

```

8 结语
==

> 如果你能看到这里，鉴于有很多小白可能会参考我这篇文章来进行前期配置，特说明下配置学习路线：
> 
> 1.  拿到项目及后台接口，首先做的是配置全局代理及**第二点**；
> 2.  全局封装 axios 及第三点 request.js；
> 3.  过滤 axios 请求方式，控制路径及参数的格式及第四点 http.js；
> 4.  正式封装 api 及第五点 api.js；
> 5.  页面调用；

以上就详细介绍了，在 vue-cil 项目中 如何封装 axios，封装请求，封装公共的 api，配置多个接口，页面如何调用请求等问题，都是亲测有用的~ 但是这种封装方法的话，更适合大中型项目，配置比较合理，如果是自己小项目，就直接用 axios 就完事了。。。

**补充**：关于代理的配置及若出现配置代理报错 404 的问题，可以参考我的文章：[代理的配置](https://blog.csdn.net/weixin_43216105/article/details/105844391)来解决；

**如果本文对你有帮助的话，请不要忘记给我点赞打 call 哦~ o(￣▽￣)ｄo**  
**有其他问题留言 over~**
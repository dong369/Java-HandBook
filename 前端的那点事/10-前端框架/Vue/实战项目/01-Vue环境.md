# 1 Vue-cli

技术选项

1、安装Vue脚手架

2、通过Vue脚手架创建项目

3、配置Vue的路由功能

4、配置element-UI组件

5、网络交互Axios

6、初始化Git仓库

7、将项目移交到GitLab或公有Git上

## 1.1 安装升级

```properties
cnpm install -g @vue/cli  // 安装
cnpm install -g vue-cli // 更新
vue -V 或者 vue --version  // 查看版本
```

## 2.2 创建项目

### 2.2.1 交互式命令

```properties
vue create vue-project
```

### 2.2.2 图形界面

```properties
vue ui
```
### 2.2.3 旧版本

```properties
vue init webpack vue
```

# 2  项目依赖

## 2.1 添加依赖

```properties
npm install sass-loader --save;
npm install node-sass --save;
```
如果安装过程报错，可以采用cnpm！！！

## 2.2 卸载依赖

```properties
npm uninstall babel-plugin-transform-runtime --save
```


## 2.3 运行项目
```properties
npm run dev 或 npm run serve
```

# 3 页面开发

## 3.1 第一张页面

### 2.4.1 第一步 

在组件总目录下面新建一个你要写的组件的目录，并新建一个你要写的组件，如test/test.vue

![第一步.png](https://upload-images.jianshu.io/upload_images/8185387-d7c2ec85cc088105.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 2.4.2 第二步

编写组件内容(组件就是把功能拆分出来，然后哪里需要这个组件，就在哪里去引入就行了)
![第二步.png](https://upload-images.jianshu.io/upload_images/8185387-d75dfd75ba2a4f12.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2.2.3 第三步
修改路由，将新写的组件插入到路由里面去(找到src/router/router.js，将页面组件（如刚写的test.vue）添加到appRouter里面去) 
![第三步.png](https://upload-images.jianshu.io/upload_images/8185387-2ad0185e08806ff5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



```vue
<template>
  <div id="app">
    <router-view v-loading="loading"></router-view>
  </div>
</template>
```



## 3.2 框架选型

### 3.1 Ivue
```properties
git clone -b clean https://gitee.com/log4j/pig-ui    // clean分支
```
### 3.2 Element
```properties
npm install
//or # 建议不要用cnpm  安装有各种诡异的bug 可以通过如下操作解决npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```

## 3.3 网络请求

### 4.1 axios接口调用

```properties
cnpm install axios --save;
```

## 3.4 多环境配置

### 5.1 第一种方式

一般有三个环境，一是开发环境，二是测试环境，三是正式环境，每个环境都有一个接口地址。

在config/prod.env.js文件中通过后缀名区分不同的环境，因为prod.env.js定义的常量可以在全局引用，省去了我们再定义全局变量的步骤。

```properties
'use strict'
module.exports = {
  NODE_ENV: '"production"',
  API_PATH_DEV: '"http://127.0.0.1:10403"',
  API_PATH_TEST: '"http://127.0.0.1:9999"',
  API_PATH_PROD: '"http://127.0.0.1:10403"'
}
```

在main.js中，引入axios，并根据当前的域名配置axios的baseURL

```properties
function handleUrl(url) {
  // 环境的切换，BASE_URL是接口的ip前缀，比如http:10.100.1.1:8989/
  var baseUrl;
  if (window.location.hostname === 'localhost' || window.location.hostname === 'dev.com') {
    baseUrl = process.env.API_PATH_DEV
  } else if (window.location.hostname === 'test.com') {
    baseUrl = process.env.API_PATH_TEST
  } else if (window.locatin.hostname === 'prod.com') {
    baseUrl = process.env.API_PATH_PROD
  }
  return baseUrl + url
}
```

## 3.5 页面部署

### 6. 1 克隆前端页面

```properties
git clone -b clean https://gitee.com/log4j/pig-ui    // clean分支
```

### 6.2 安装依赖

```properties
npm install
//or # 建议不要用cnpm  安装有各种诡异的bug 可以通过如下操作解决npm速度慢的问题
npm install --registry=https://registry.npm.taobao.org
```

### 6. 3 开启服务

```properties
npm run dev
```

### 6. 4 打包服务

```
npm run build
```

### 6.5 Nginx部署

将打包后生产的disc文件上传到nginx的html目录下即可！
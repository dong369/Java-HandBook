## 1. 安装配置
### 1.1 下载
> 1.	下载绿色的mychrome安装包。可以到[zdfans](http://www.zdfans.com/)，版本 67.0.3396.99（正式版本） （64 位）
> 2.	对浏览器进行简单的配置。
> 3.	设置Google Chrome为默认浏览器

**注意：鼠标的中间键的作用可以是在新的tab页打开链接**

### 1.2 基础配置

## 2. 安装插件

插件下载地址：[Google插件迷](<https://extfans.com/>)

### 2.1 谷歌访问助手



### 2.2 Vimium





### 2.3 FeHelper





### 2.4 扩展管理





### 2.5 暴力猴





### 2.6 Octotree





### 2.7 Infinity





### 2.8 Adblock Plus





### 2.9 HostAdmin App







> 相应插件。（谷歌访问助手、postman、Postman Interceptor、JSONView、cookie、[baidudl](https://link.jianshu.com/?t=https://chrome.google.com/webstore/detail/baidudl/mccebkegnopjehbdbjbepjkoefnlkhef)、Vimium、[HostAdmin App](https://chrome.google.com/webstore/detail/hostadmin-app/mfoaclfeiefiehgaojbmncmefhdnikeg/related?utm_source=www.crx4chrome.com)）
> ![谷歌访问助手](http://upload-images.jianshu.io/upload_images/8185387-c7751e492bbf55aa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![Postman组合使用](http://upload-images.jianshu.io/upload_images/8185387-13f65d0e76ba1c33.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![EditThisCookie工具](http://upload-images.jianshu.io/upload_images/8185387-c154e37c236a8d78.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![JSONView](http://upload-images.jianshu.io/upload_images/8185387-92ae15b3208922d6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![GitHub源码阅读](https://upload-images.jianshu.io/upload_images/8185387-b09711449e422cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![单词记忆](https://upload-images.jianshu.io/upload_images/8185387-e59f114c546e6d2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
> ![页面](https://upload-images.jianshu.io/upload_images/8185387-fa6258919acc46a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)![键盘操作](https://upload-images.jianshu.io/upload_images/8185387-5c43b4df11b5649e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 3. 前段调试
### 3.1调试技巧
1. debugger
```
除了console.log, debugger是我们最喜欢、快速且肮脏的调试工具。执行代码后，Chrome会在执行时自动停止。你甚至可以把它封装成条件，只在需要时才运行。
if (thisThing) {
    debugger;
}
备注：F10为单步调试；F8为跳到下一个断点处。
```
2. 使用 console.time() 和 console.timeEnd() 测试循环
```
要得知某些代码的执行时间，特别是调试缓慢循环时，非常有用。 甚至可以通过给方法传入不同参数，来设置多个定时器。来看看它是怎么运行的：
console.time('Timer1');
var items = [];
for(var i = 0; i < 100000; i++){
   items.push({index: i});
}
console.timeEnd('Timer1');
```
3. javascript的简写方式
```
1. 三目运算符
const x = 20;
let answer;if (x > 10) {
    answer = 'greater than 10';
} else {
    answer = 'less than 10';
}
const answer = x > 10 ? 'greater than 10' : 'less than 10';
```
2. 循环语句
```
当使用纯 JavaScript（不依赖外部库，如 jQuery 或 lodash）时，下面的简写会非常有用。
for (let i = 0; i < allImgs.length; i++)
简写为：
for (let index of allImgs)
下面是遍历数组 forEach 的简写示例：
function logArrayElements(element, index, array) {
  console.log("a[" + index + "] = " + element);
}
[2, 5, 9].forEach(logArrayElements);// logs:// a[0] = 2// a[1] = 5// a[2] = 9
```
3. 声明变量
```
在函数开始之前，对变量进行赋值是一种很好的习惯。在申明多个变量时：
let x;
let y;
let z = 3;
可以简写为：
let x, y, z=3;
```
4. if 语句
```
在使用 if 进行基本判断时，可以省略赋值运算符。
if (likeJavaScript === true)
简写为：
if (likeJavaScript)
```
5. 变量赋值
```
当将一个变量的值赋给另一个变量时，首先需要确保原值不是 null、未定义的或空值。
可以通过编写一个包含多个条件的判断语句来实现：
if (variable1 !== null || variable1 !== undefined || variable1 !== '') {
     let variable2 = variable1;
}
或者简写为以下的形式：
const variable2 = variable1  || 'new';
```
6. 箭头函数
```
经典函数很容易读写，但是如果把它们嵌套在其它函数中进行调用时，整个函数就会变得有些冗长和混乱。这时候可以使用箭头函数来简写：
function sayHello(name) {
  console.log('Hello', name);
}
 
setTimeout(function() {
  console.log('Loaded')
}, 2000);
 
list.forEach(function(item) {
  console.log(item);
});

简写为：
sayHello = name => console.log('Hello', name);
setTimeout(() => console.log('Loaded'), 2000);
list.forEach(item => console.log(item));
```

## 4. 搜索技巧
### 4.1用 site 命令
> 科学上网 site:zhihu.com OR site:jianshu.com
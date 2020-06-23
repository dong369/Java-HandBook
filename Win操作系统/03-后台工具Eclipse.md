
# 1 基本环境配置
## 1.1 统一项目编码
![Workspace编码](http://upload-images.jianshu.io/upload_images/8185387-e2f8f17905df507d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Spelling](http://upload-images.jianshu.io/upload_images/8185387-d096eff491423615.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![JSP等文件](http://upload-images.jianshu.io/upload_images/8185387-119af52e3ee62b83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![配置属性编码](http://upload-images.jianshu.io/upload_images/8185387-b30384c00e75887e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.2 统一代码格式
![统一代码格式](http://upload-images.jianshu.io/upload_images/8185387-cbf7cf2aff9d9b34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 1.3 统一项目视图
> 切换到自己喜爱且方便的视图模式写，看个人情况。本次我们主要介绍两种视图，一种是Java包视图（package explorer），另一种是Java的项目视图（project explorer）。

### 1.3.1 package explorer
> package explorer一般会在写Java简单项目和JavaWeb项目使用。
> ![package explorer视图](http://upload-images.jianshu.io/upload_images/8185387-3075405ea0f68b85.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.3.2 project explorer
> project explorer一般会在写maven的聚合项目使用，便于查看项目直接的相互依赖关系。
> ![project explorer视图](http://upload-images.jianshu.io/upload_images/8185387-9218a71b3bdace89.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>

## 1.4 错误和警告

### 1.4.1 忽略dubbo的错误信息
![添加dubbo](http://upload-images.jianshu.io/upload_images/8185387-12c7cff55d200c72.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.5.1 忽略JQuery.min.js报错的警告
打开项目下的.project文件夹,把以下内容注释！![注释内容](http://upload-images.jianshu.io/upload_images/8185387-42b599efdb90e066.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 1.4.2 Tomcat的配置
#### 4.6.1 Tomcat服务调优
关于TOMCAT端口的一些事情

一、Tomcat有三个端口号
​        1. 8080 ---- 服务访问端口
​        2. 8005 ---- 8005是远程关闭tomcat
​        3. 8009 ---- Apache代理时用的

二、Tomcat的优化配置
```xml
  <Server port ="800 5" shutdown ="SHUTDOWN">
                改为
  <Server port ="- 1" shutdown ="SHUTDOWN" >

  <Connector port="8080" protocol ="HTTP/ 1.1"  conne ctionT imeout ="20000"  redirectPort="8443" useBodyEncodingForURI="true" URIEncoding="UTF-8" />

  <Connector port="8009" protocol="AJP/ 1.3" redirectPort="8443" />本行删除，这样做原因是端口监听的多，Tomcat服务器越慢！
```


#### 4.6.2 eclipse中Tomcat的配置
![图-07](http://upload-images.jianshu.io/upload_images/8185387-8ac9c587d8046cab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图-08](http://upload-images.jianshu.io/upload_images/8185387-3003fb065692d980.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图-09](http://upload-images.jianshu.io/upload_images/8185387-3f268d5099bcdbd3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图-10](http://upload-images.jianshu.io/upload_images/8185387-1c14c5ea6601e848.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图-11](http://upload-images.jianshu.io/upload_images/8185387-14f84c06bf55f455.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![图-12](http://upload-images.jianshu.io/upload_images/8185387-7bac0605c6455f34.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 1.4.3 快捷键
#### 4.7.1 常用快捷键
```properties
Alt+/：代码提示
Ctrl+Alt+下：复制当前行代码
ctrl+shift+r：打开资源
ctrl+o：快速outline
Ctrl+1：快速修复(最经典的快捷键,就不用多说了)
ctrl+2：L：为本地变量赋值
Alt+Shift+R：快速修改所有的变量名
ctrl + shift+o：搜索一个类中的方法
ctrl + o：搜索一个类中的方法
ctrl + h 打开搜索
alt + F5 更新项目（当项目没有错误，但是仍然出现错误标记时使用）
Ctrl+D: 删除当前行
Ctrl+T：快速显示当前类的继承结构
Ctrl+/：注释当前行,再按则取消注释
Ctrl+Shift+F：格式化当前代码
Alt+shift+S：书写实体类是时候使用（C+O+R+S）
Alt+F5：校对maven的jar包
ctrl + shift + x：大写
ctrl + shift + y：小写
向左：将要移动的代码选中，然后按TAB键
向右：将要移动的代码选中，然后按shift+tab键
更多快捷键组合可在Eclipse按下ctrl+shift+L查看。
调试中：
F5      进入执行的方法中
F6      当前代码的表面执行，单行执行下一步
F7      不再观察，返回进入处
F8      停止调试，直接正常执行完毕 
```
#### 4.7.2 输入@给提示
![输入@自动提示](http://upload-images.jianshu.io/upload_images/8185387-46a6786d0609e9ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 4.8 Eclipse.ini文件的配置

```properties
-startup
plugins/org.eclipse.equinox.launcher_1.3.200.v20160318-1642.jar
--launcher.library
plugins/org.eclipse.equinox.launcher.win32.win32.x86_64_1.1.400.v20160518-1444
-product
org.eclipse.epp.package.jee.product
--launcher.defaultAction
openFile
-showsplash
org.eclipse.platform
--launcher.defaultAction
openFile
--launcher.appendVmargs
// 配置自己的jdk文件路径
-VM
E:\dev\soft\jdk8\bin\javaw.exe
-vmargs
-Dosgi.requiredJavaVersion=1.8
-XX:+UseG1GC
-XX:+UseStringDeduplication
-Dosgi.requiredJavaVersion=1.8
-Xms256m
-Xmx1024m
```



### 4.9 代码注释配置

#### 4.9.1 类注释
```java
/**
 * project - ETC发票系统
 *
 * @author ${user} 
 * @date 日期:${date} 时间:${time}
 * @JDK 1.8 
 * @version 1.0
 * @since 1.0
 * @Description 功能模块： 
 */
```
#### 4.9.2 方法注释
```java
 /**
 * @Title ${enclosing_method}  
 * ${tags} 
 * @date 日期:${date} 时间:${time}
 * @return ${return_type}
 * @Description 功能： 
 */
```

# 2 版本控制

### 4.1 SVN项目提交

#### 4.1.1配置SVN

***步骤一：提交项目到SVN***
![忽略不需要提交的内容](http://upload-images.jianshu.io/upload_images/8185387-0a01b1f66907027f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.1.2 项目提交



#### 4.1.3 项目检出

***步骤一：从SVN下载项目代码***
![图-05](http://upload-images.jianshu.io/upload_images/8185387-fa5c315193092bb7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***步骤二：将项目代码转成maven项目***
![图-06](http://upload-images.jianshu.io/upload_images/8185387-16f1ab5d13674b1e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4.1.4 项目打分支



#### 4.1.5 项目分支合并



#### 4.1.6 项目Tag标签



### 4.2 Git使用

> eclipse和sts都内置有git插件。

#### 4.2.1 配置Git



#### 4.2.2 提交项目



#### 4.2.3 检出项目



#### 4.2.4 项目打分支



#### 4.2.5 项目合并



#### 4.2.6 项目Tag标签



# 3 项目构建

# 4 插件配置

### 6.1 lombok

> 代码简洁工具包。




# React第7天

## RN学习说明
1. ReactNative是基于React这门框架的语法来进行开发的；
2. RN中，提供了 移动端 专用的一些组件，这时候，我们在网页中使用的一些 元素，div, p, img 都不能用了，只能使用RN固有的组件；
3. 最终，开发出来的项目，是要运行到手机上的，那么，如何把一个 RN 的项目，完整的发布到手机上去运行呢，这里，需要结合 安卓的 签名打包步骤，并使用 RN 提供的打包命令，进行完整 apk 文件的发布；最终发布出来的就是 Release 版本的项目，可以上传到应用商店；


## 配置ReactNative基本开发环境
[搭建基本的开发环境 - 英文官网](http://facebook.github.io/react-native/docs/getting-started.html)<br/>
[搭建基本的开发环境 - 中文](http://reactnative.cn/docs/0.42/getting-started.html#content)
这两篇文档对比着进行参考，进行相关的安装；

## 手机的相关配置
1. 使用数据线，把手机链接到电脑上；
2. 运行 `adb devices` 的命令，这个命令，是安卓开发环境提供的；
3. 需要先开启手机的`开发者模式`
4. 如果开启开发者模式之后，还是看不到设备，则尝试安装 `豌豆荚` 这样的工具，让这些工具帮助你在电脑上安装手机的驱动；

## 搭建RN的项目
1. 运行`react-native init 项目名称`来初始化一个react native项目；
![01.项目初始化完毕之后截图并说明](images/01.项目初始化完毕之后截图并说明.png)
2. 打包运行项目，把打包好的项目部署到手机中！
 + 确保手机已经正确的链接到了当前电脑上，同时手机开启了`开发者调试模式`；可以使用`adb devices`来查看当前链接到电脑上的手机设备列表！
 + 当确认手机正确链接到电脑上之后，可以运行`react-native run-android`来打包当前项目，并把打包好的项目以调试的模式安装到手机中！
 + 打包完成之后的截图
![02.打包完成之后的截图](images/02.打包完成之后的截图.png)
 + React Package窗口的作用
![03.React Package窗口的作用](images/03.React Package窗口的作用.png)
 + 04.React  Packager打包编译代码截图
![04.React  Packager打包编译代码截图](images/04.React  Packager打包编译代码截图.png)
 + 当第一打包编译项目部署到手机上之后 - 如何设置开发服务器的地址
![05.当第一打包编译项目部署到手机上之后 - 如何设置开发服务器的地址.png](images/05.当第一打包编译项目部署到手机上之后 - 如何设置开发服务器的地址.png)

## 项目结构介绍以及一些注意事项


## 使用样式


##修改项目首屏页面


## 基本组件的使用介绍
+ View：
+ Text：
+ TextInput：
+ Image：
+ Button：
+ ActivityIndicator：
+ ScrollView：这是一个列表滚动的组件
+ ListView：也是一个列表滚动的组件，但是，这个组件已经过时了，官方推荐使用 FlatList 来代替它

## 判断组件是否被卸载
```
if (this._reactInternalInstance){
	// 组件没有被卸载
}
```

## [配置Tab栏](https://github.com/happypancake/react-native-tab-navigator)

## [配置Tab栏的图标](https://github.com/oblador/react-native-vector-icons)
> 注意：使用图标，需要使用 `Android SDK Manager` 安装 `Android SDK Build-tools 26.0.1` 并接收其 license;

## 案例：豆瓣电影列表
+ 电影列表数据：`https://api.douban.com/v2/movie/in_theaters`
+ 电影详细数据：`https://api.douban.com/v2/movie/subject/26309788`

## 安装路由
1. 运行`npm i react-native-router-flux --save`
2. 路由官网：`https://github.com/aksonov/react-native-router-flux`
3. 路由相关配置：`https://github.com/aksonov/react-native-router-flux/blob/master/docs/API.md`
4. 路由简单的DEMO：`https://github.com/aksonov/react-native-router-flux/blob/v3/docs/MINI_TUTORIAL.md`

## 路由的一些基本使用方法


## 配置首页的轮播图
1. 轮播图官网：`https://github.com/leecade/react-native-swiper?utm_source=tuicool&utm_medium=referral`
2. 运行`npm i react-native-swiper --save`安装轮播图组件
3. 导入轮播图组件`import Swiper from 'react-native-swiper';`
4. 其中，在Swiper身上，`showsPagination={false}`是用来控制页码的；`showsButtons={false}`是用来控制左右箭头显示与隐藏；`height={160}`是用来控制轮播图区域的高度的！
5. 设置轮播图的样式：
```
var styles = StyleSheet.create({
    wrapper: {},
    slide1: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#9DD6EB',
    },
    slide2: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#97CAE5',
    },
    slide3: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        backgroundColor: '#92BBD9',
    },
    image:{
        width:'100%',
        height:'100%'
    }
})
```
6. 将组件的代码结构引入到页面上：
```
<Swiper style={styles.wrapper} showsButtons={true} height={160} autoplay={true}>
                <View style={styles.slide1}>
                    <Image source={{uri:'http://www.itcast.cn/images/slidead/BEIJING/2017410109413000.jpg'}} style={styles.image}></Image>
                </View>
                <View style={styles.slide2}>
                    <Image source={{uri:'http://www.itcast.cn/images/slidead/BEIJING/2017440109442800.jpg'}} style={styles.image}></Image>
                </View>
                <View style={styles.slide3}>
                    <Image source={{uri:'http://www.itcast.cn/images/slidead/BEIJING/2017441409442800.jpg'}} style={styles.image}></Image>
                </View>
            </Swiper>
```

#### 首页轮播图片URL地址：
+ 图片地址1：http://www.itcast.cn/images/slidead/BEIJING/2017410109413000.jpg
+ 图片地址2：http://www.itcast.cn/images/slidead/BEIJING/2017440109442800.jpg
+ 图片地址3：http://www.itcast.cn/images/slidead/BEIJING/2017441409442800.jpg

## 渲染电影列表数据

## 渲染电影详情页面

## 调用摄像头拍照
[react-native-image-picker的github官网](https://github.com/marcshilling/react-native-image-picker)
[react native 之 react-native-image-picke的详细使用图解](http://www.cnblogs.com/shaoting/p/6148085.html)
1.  运行`npm install react-native-image-picker@latest --save`安装到项目运行依赖，此时调试**可能会报错**，如果报错，需要使用下面的步骤解决：
	+ 先删除`node_modules`文件夹
	+ 运行`npm i`
	+ 运行`npm start --reset-cache`
2. 运行`react-native link`自动注册相关的组件到原生配置中
3. 打开项目中的`android`->`app`->`src`->`main`->`AndroidManifest.xml`文件，在第8行添加如下配置：
```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```
4. 打开项目中的`android`->`app`->`src`->`main`->`java`->`com`->`当前项目名称文件夹`->`MainActivity.java`文件，修改配置如下：
    ```
    package com.native_camera;
    import com.facebook.react.ReactActivity;

    // 1. 添加以下两行：
    import com.imagepicker.permissions.OnImagePickerPermissionsCallback; // <- add this import
    import com.facebook.react.modules.core.PermissionListener; // <- add this import

    public class MainActivity extends ReactActivity {
        // 2. 添加如下一行：
        private PermissionListener listener; // <- add this attribute

        /**
         * Returns the name of the main component registered from JavaScript.
         * This is used to schedule rendering of the component.
         */
        @Override
        protected String getMainComponentName() {
            return "native_camera";
        }
    }
    ```
5. 在项目中添加如下代码：
    ```
    // 第1步：
    import {View, Button, Image} from 'react-native'
    import ImagePicker from 'react-native-image-picker'
    var photoOptions = {
      //底部弹出框选项
      title: '请选择',
      cancelButtonTitle: '取消',
      takePhotoButtonTitle: '拍照',
      chooseFromLibraryButtonTitle: '选择相册',
      quality: 0.75,
      allowsEditing: true,
      noData: false,
      storageOptions: {
        skipBackup: true,
        path: 'images'
      }
    }

    // 第2步：
    constructor(props) {
    super(props);
        this.state = {
          imgURL: ''
        }
      }

	// 第3步：
    <Image source={{ uri: this.state.imgURL }} style={{ width: 200, height: 200 }}></Image>
    <Button title="拍照" onPress={this.cameraAction}></Button>

    // 第4步：
    cameraAction = () => {
    ImagePicker.showImagePicker(photoOptions, (response) => {
      console.log('response' + response);
      if (response.didCancel) {
        return
      }
      this.setState({
        imgURL: response.uri
      });
    })
  }
    ```
6. **一定要退出之前调试的App**，并重新运行`react-native run-android`进行打包部署；这次打包期间会下载一些jar的包，需要耐心等待！

## 签名打包发布Release版本的apk安装包
+ 请参考以下两篇文章：
 - [ReactNative之Android打包APK方法（趟坑过程）](http://www.jianshu.com/p/1380d4c8b596)
 - [React Native发布APP之签名打包APK](http://blog.csdn.net/fengyuzhengfan/article/details/51958848)

### 如何发布一个apk
1. 先保证自己正确配置了所有的 RN 环境
2. 在 cmd 命令行中，运行这一句话`keytool -genkey -v -keystore my-release-key2.keystore -alias my-key-alias2 -keyalg RSA -keysize 2048 -validity 10000`
 + 其中： `my-release-key.keystore` 表示你一会儿要生成的那个 签名文件的 名称【很重要，包找个小本本记下来】
 + `-alias` 后面的东西，也很重要，需要找个小本本记下来，这个名称可以根据自己的需求改动`my-key-alias`
 + 当运行找个命令的时候，需要输入一系列的参数，找个口令的密码，【一定要找个小本本记下来】
3. 当生成了签名之后，这个签名，默认保存到了自己的用户目录下`C:\Users\liulongbin\my-release-key2.keystore`
4. 将你的签名证书copy到 android/app目录下。
5. 编辑 `android` -> `gradle.properties`文件，在最后，添加如下代码：
```
MYAPP_RELEASE_STORE_FILE=your keystore filename
MYAPP_RELEASE_KEY_ALIAS=your keystore alias
MYAPP_RELEASE_STORE_PASSWORD=*****
MYAPP_RELEASE_KEY_PASSWORD=*****
```
6. 编辑 android/app/build.gradle文件添加如下代码：
```
...
android {
    ...
    defaultConfig { ... }
    + signingConfigs {
    +    release {
    +        storeFile file(MYAPP_RELEASE_STORE_FILE)
    +        storePassword MYAPP_RELEASE_STORE_PASSWORD
    +        keyAlias MYAPP_RELEASE_KEY_ALIAS
    +        keyPassword MYAPP_RELEASE_KEY_PASSWORD
    +    }
    +}
    buildTypes {
        release {
            ...
    +        signingConfig signingConfigs.release
        }
    }
}
...
```
7. 进入项目根目录下的`android`文件夹，在当前目录打开终端，然后输入`./gradlew assembleRelease`开始发布APK的Release版；
8. 当发行完毕后，进入自己项目的`android\app\build\outputs\apk`目录中，找到`app-release.apk`，这就是我们发布完毕之后的完整安装包；就可以上传到各大应用商店供用户使用啦；

>注意：请记得妥善地保管好你的密钥库文件，不要上传到版本库或者其它的地方。

## 相关文章
+ [React Native 小米（红米）手机安装失败、白屏 Failed to establish session 解决方案](http://blog.csdn.net/u011240877/article/details/51983262)
+ [React Native Android 初次试用遇到的各种坑](http://lib.csdn.net/article/reactnative/48721)
+ [Redux 中文文档](http://www.redux.org.cn/)
+ [react-native 在使用require加载本地图片时报Unexcepted character](http://blog.csdn.net/u014038534/article/details/53943862)
+ [React Native for Android 发布独立的安装包](http://blog.csdn.net/u013531824/article/details/51003775)
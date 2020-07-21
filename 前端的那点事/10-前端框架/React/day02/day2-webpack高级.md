## 移动App第2天

## webpack的发布策略
1. 在实际开发中，一般会有两套项目方案：
 + 一套是开发期间的项目，包含了测试文件、测试数据、开发工具、测试工具等相关配置，有利于项目的开发和测试，但是这些文件仅用于开发，发布项目时候需要剔除；
 + 另一套是部署期间的项目，剔除了那些客户用不到的测试数据测试工具和文件，比较纯净，减少了项目发布后的体积，有利于安装和部署！
2. 为了满足我们的发布策略，需要新建一个配置文件，命名为`webpack.publish.config.js`，将`webpack.config.js`的配置拷贝过去，剔除一些开发配置项即可：
 + 将`devServer`节点删掉：
 ```
 devServer: {
        hot: true,
        open: true,
        port: 4321
    }
 ```
 + 将`plugins`节点下的热更新插件删掉：
 ```
 new webpack.HotModuleReplacementPlugin()
 ```
3. 修改`url-loader`，将图片放入统一的images文件夹之下：
```
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43959&name=images/[name].[ext]' }
```
或者使用`img-`前缀加上`7位的hash名称`：
```
{ test: /\.(png|jpg|gif)$/, use: 'url-loader?limit=43959&name=images/img-[hash:7].[ext]' }
```
4. 在`package.json`中的script节点下新增`dev`命令，通过`--config`指定webpack启动时要读取的配置文件：
```
"pub": "webpack --config webpack.publish.config.js"
```

## 每次重新构建时候删除dist目录
1. 运行`cnpm i clean-webpack-plugin --save-dev`
2. 在头部引入这个插件：
```
var cleanWebpackPlugin = require('clean-webpack-plugin');
```
3. 在`plugins`节点下使用这个插件：
```
new cleanWebpackPlugin(['dist'])
```

## 分离第三方包改造`webpack.publish.config.js`
1. 改造entry节点如下：
```
entry: {
        app: path.resolve(__dirname, 'src/js/main.js'), // 自己代码的入口
        vendors: ['jquery'] // 要分离的第三方包的入口
    }
```
2. 在plugins节点下新增插件：
```
new webpack.optimize.CommonsChunkPlugin({ // 抽离第三方包的插件
        name:'vendors', // 指定抽离第三方包的入口名称
        filename:'vendors.js' // 抽离出的公共模块的名称
})
```
3. `html-webpack-plugin`在生成`index.html`文件的时候，会自动将抽离的第三方包引入进去！

## [优化压缩JS](https://webpack.js.org/configuration/plugins/#plugins)
在plugins数组中添加：
```
new webpack.optimize.UglifyJsPlugin({ // 优化压缩JS
    compress:{
        warnings:false // 移除警告
    }
}),
new webpack.DefinePlugin({ // 设置为产品上线环境，进一步压缩JS代码
    'process.env.NODE_ENV': '"production"'
})
```

## 优化压缩HTML文件
在`plugins`节点下的`htmlWebpackPlugin`插件中，添加`minify`子节点：
```
minify:{// 压缩HTML代码
    collapseWhitespace:true, // 合并空白字符
    removeComments:true, // 移除注释
    removeAttributeQuotes:true // 移除属性上的引号
}
```
其他优化项请参考：[html-minifier - github](https://github.com/kangax/html-minifier#options-quick-reference)

## [抽取CSS文件](https://github.com/webpack-contrib/extract-text-webpack-plugin)
1. 运行`npm install --save-dev extract-text-webpack-plugin`安装抽取CSS文件的插件。
2. 在配置文件中导入插件：
```
const ExtractTextPlugin = require("extract-text-webpack-plugin");
```
3. 修改CSS和Sass的loader如下：
```
{
    test: /\.css$/, use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: ["css-loader"],
        publicPath: '../' // 设置图片路径
    })
},
{
    test: /\.scss$/, use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: ['css-loader', 'sass-loader'],
        publicPath: '../' // 设置图片路径
    })
}
```
4. 在plugins节点下新增插件：
```
new ExtractTextPlugin("css/styles.css"), // 抽取CSS文件的插件
```

## [压缩抽取出来的CSS文件](https://github.com/NMFR/optimize-css-assets-webpack-plugin)
1. 运行`cnpm i optimize-css-assets-webpack-plugin --save-dev`安装插件到开发依赖。
2. 在配置文件头部导入插件：
```
var OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
```
3. 在plugins节点下新增插件：
```
 new OptimizeCssAssetsPlugin() // 压缩CSS文件的插件
```

## 相关文章
1. [Sass 基础教程](http://www.sasschina.com/guide/)
2. [webpack-dev-server](https://github.com/webpack/webpack-dev-server/releases)
3. [You have not accepted the license agreements of the following ](http://majing.io/questions/804)
---
layout: post
title: webpack学习之插件[Plugins](热加载)
subtitle: 笔记
date: 2022-06-12
author: Wason
header-img: img/bg/post-bg-25.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之插件\[Plugins\](热加载) #
***接上文***

3）Hot Module Replacement
Hot Module Replacement（HMR）也是webpack里很有用的一个插件，它允许你在修改组件代码后，自动刷新实时预览修改后的效果。

要使用这个插件需要 在webpack.config.js中引用插件、更改webpack.config.js中的devServer设置、更改.babelrc配置文件引用react-transform-hrm的插件

**补充：**
* HMR是一个webpack插件，它让你能浏览器中实时观察模块修改后的效果，但是如果你想让它工作，需要对模块进行额外的配额；
* Babel有一个叫做react-transform-hrm的插件，可以在不对React模块进行额外的配置的前提下让HMR正常工作；

---

① 更改webpack.config.js，在devServer增加 hot 和 plugins中增加 HotModuleReplacementPlugin 插件
```
devServer: {
   contentBase: "./public",//本地服务器所加载的页面所在的目录
   historyApiFallback: true,//不跳转
   inline: true,
   hot: true
 },

plugins: [
 new webpack.BannerPlugin('版权所有，翻版必究'),
 new HtmlWebpackPlugin({
     template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
 }),
 new webpack.HotModuleReplacementPlugin()//热加载插件
],
```
② 安装babel的插件 react-transform-hmr
```
npm install --save-dev babel-plugin-react-transform react-transform-hmr
```
③ 配置babel
```
// .babelrc
{
  "presets": ["react", "env"],
  "env": {
    "development": {
    "plugins": [["react-transform", {
       "transforms": [{
         "transform": "react-transform-hmr",
         
         "imports": ["react"],
         
         "locals": ["module"]
       }]
     }]]
    }
  }
}
```
④ 运行服务器 npm run start ,直接更改Greeter.js中的内容后保存，页面直接跟着改变。

4）产品阶段的构建  
在产品阶段，可能还需要对打包的文件进行额外的处理，比如说优化，压缩，缓存以及分离CSS和JS。  
① 创建webpack.production.config.js文件  
```
// webpack.production.config.js
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: __dirname + "/app/main.js", //已多次提及的唯一入口文件
    output: {
        path: __dirname + "/build",
        filename: "bundle.js"
    },
    devtool: 'null', //注意修改了这里，这能大大压缩我们的打包代码
    devServer: {
        contentBase: "./public", //本地服务器所加载的页面所在的目录
        historyApiFallback: true, //不跳转
        inline: true,
        hot: true
    },
    module: {
        rules: [
          {
            test: /(\.jsx|\.js)$/,
            use: {
                loader: "babel-loader"
            },
            exclude: /node_modules/
          }, {
            test: /\.css$/,
            // 下面这段代码有问题
            use: ExtractTextPlugin.extract({
                fallback: "style-loader",
                use: [{
                    loader: "css-loader",
                    options: {
                        modules: true
                    }
                }, {
                    loader: "postcss-loader"
                }],
            })
使用上面的方法会报  ExtractTextPlugin is not defined 报错，所以改用下面代码：
            use: [
              {
                loader: 'style-loader'
              },
              {
                loader: 'css-loader',
                options: {
                  modules: true
                } 
              },
              {
                loader: 'postcss-loader'
              }
            ]
        }]
    },
    plugins: [
        new webpack.BannerPlugin('版权所有，翻版必究'),
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html" //new 一个这个插件的实例，并传入相关的参数
        }),
        new webpack.HotModuleReplacementPlugin() //热加载插件
    ],
};
```
② 修改 package.json里的scripts指令  
```
//package.json
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "webpack",
    "server": "webpack-dev-server --open",
    "build": "NODE_ENV=production webpack --config ./webpack.production.config.js --progress"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
...
  },
  "dependencies": {
    "react": "^15.6.1",
    "react-dom": "^15.6.1"
  }
}

注意:如果是window电脑，build需要配置为"build":
 "set NODE_ENV=production && webpack --config ./webpack.production.config.js --progress".  

```
③ 然后运行npm run build 就可以在 build文件夹中生成打包后的文件。

***To Be Continue~***

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050
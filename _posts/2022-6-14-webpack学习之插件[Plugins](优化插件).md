---
layout: post
title: webpack学习之插件[Plugins](优化插件)
subtitle: 笔记
date: 2022-06-14
author: Wason
header-img: img/bg/post-bg-26.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之插件\[Plugins\](优化插件) #
***接上文***

5）优化插件 [OccurenceOrderPlugin、UglifyJsPlugin、ExtractTextPlugin]
webpack提供了一些在发布阶段非常有用的优化插件，它们大多来自于webpack社区，可以通过npm安装，通过以下插件可以完成产品发布阶段所需的功能

* OccurenceOrderPlugin :为组件分配ID，通过这个插件webpack可以分析和优先考虑使用最多的模块，并为它们分配最小的ID；
* UglifyJsPlugin：压缩JS代码；
* ExtractTextPlugin：分离CSS和JS文件.

内置插件：OccurenceOrder、UglifyJS plugins

外置插件：ExtractTextPlugin

① 安装外置插件：npm install --save-dev extract-text-webpack-plugin
```
===版本号===>
+ extract-text-webpack-plugin@3.0.2
added 6 packages from 6 contributors in 6.929s
```
② 更改 webpack.production.config.js配置文件
```
// webpack.production.config.js
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
    entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
        path: __dirname + "/build",
        filename: "bundle.js"
    },
    devtool: 'none',
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
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
            },
            {
                test: /\.css$/,
                use: [
                    {
                        loader: "style-loader"
                    }, {
                        loader: "css-loader",
                        options: {
                            modules: true
                        }
                    }, {
                        loader: "postcss-loader"
                    }
                ]
            }
        ]
    },
    plugins: [
        new webpack.BannerPlugin('版权所有，翻版必究'),
        new HtmlWebpackPlugin({
            template: __dirname + "/app/index.tmpl.html"
        }),
        new webpack.optimize.OccurrenceOrderPlugin(),
        new webpack.optimize.UglifyJsPlugin(),
        new ExtractTextPlugin("style.css")
    ],
};
```
③ 执行打包： npm run build, 会报错  
```
192:webpacktest hao$ npm run build

> webpacktest@1.0.0 build /Users/hao/pro/webpacktest
> NODE_ENV=production webpack --config ./webpack.production.config.js --progress

10% building 0/0 modules 0 active(node:11864) DeprecationWarning: Tapable.plugin is deprecated. Use new API on `.hooks` instead
78% module and chunk tree optimization unnamed compat plugin/Users/hao/node_modules/webpack/lib/Chunk.js:866
        throw new Error(
        ^
Error: Chunk.entrypoints: Use Chunks.groupsIterable and filter by instanceof Entrypoint instead
    at Chunk.get (/Users/hao/node_modules/webpack/lib/Chunk.js:866:9)
    at /Users/hao/pro/webpacktest/node_modules/extract-text-webpack-plugin/dist/index.js:176:48
    at Array.forEach (<anonymous>)
    at /Users/hao/pro/webpacktest/node_modules/extract-text-webpack-plugin/dist/index.js:171:18
    at AsyncSeriesHook.eval [as callAsync] (eval at create (/Users/hao/node_modules/tapable/lib/HookCodeFactory.js:33:10), <anonymous>:24:1)
    at AsyncSeriesHook.lazyCompileHook (/Users/hao/node_modules/tapable/lib/Hook.js:154:20)
... 
——————————————————————————————————————————
解决方案：
改用其他方式安装：npm install --save-dev extract-text-webpack-plugin@next

+ extract-text-webpack-plugin@4.0.0-beta.0
removed 4 packages and updated 2 packages in 5.716s

```
在package.json中的安装版本是:  

![](http://wason419.github.io/img/20200614/2020061401.png)

④ 再次执行 npm run build，即可  

![](http://wason419.github.io/img/20200614/2020061402.png)


***To Be Continue~***

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3] 

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050



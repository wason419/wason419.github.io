---
layout: post
title: webpack学习之插件[Plugins](入门)
subtitle: 笔记
date: 2022-06-10
author: Wason
header-img: img/bg/post-bg-24.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之插件\[Plugins\](入门) #
### 插件（Plugins） ###

1. 插件（Plugins）是用来拓展Webpack功能的，它们会在整个构建过程中生效，执行相关的任务。
2.  Loaders是在打包构建过程中用来处理源文件的（JSX，Scss，Less..），一次处理一个。  
Plugins并不直接操作单个文件，它直接对整个构建过程起作用。
3. 应用：
1）给打包后代码添加版权声明的插件   
```
const webpack = require('webpack');
module.exports = {
...
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
      new webpack.BannerPlugin('版权所有，翻版必究')
    ],
};
```
2）HtmlWebpackPlugin  
这个插件的作用是依据一个简单的index.html模板，生成一个自动引用你打包后的JS文件的新index.html。这在**每次生成的js文件名称不同时非常有用**（比如添加了hash值）

---  
安装：npm install --save-dev html-webpack-plugin

**对原先目录结构做些更改：**  
①  移除public文件夹，利用此插件，index.html文件会自动生成，此外CSS已经通过前面的操作打包到JS中了.  
② 在app目录下，创建一个index.tmpl.html文件模板，这个模板包含title等必须元素，在编译过程中，插件会依据此模板生成最终的html页面，会自动添加所依赖的 css, js，favicon等文件

index.tmpl.html中的模板源代码如下：
```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
  </body>
</html>
```
③ 更新webpack的配置文件，方法同上,新建一个build文件夹用来存放最终的输出文件  
```
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
    entry: __dirname + "/app/main.js",//已多次提及的唯一入口文件
    output: {
        path: __dirname + "/build",
        filename: "bundle.js"
    },
    devtool: 'eval-source-map',
    devServer: {
        contentBase: "./public",//本地服务器所加载的页面所在的目录
        historyApiFallback: true,//不跳转
        inline: true//实时刷新
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
                        options: {    // css-loader的原先写法在没有使用插件HtmlWebpackPlugin可解决报错并正常使用，但如果要加入该插件需要将该 options 的设置改为这种写法
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
            template: __dirname + "/app/index.tmpl.html"//new 一个这个插件的实例，并传入相关的参数
        })
    ],
}; 

```

**执行 npm start ,打包后 即可在build文件夹内看到新文件**

![](http://wason419.github.io/img/20200610/2020061001.png)


***To Be Continue~***

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050


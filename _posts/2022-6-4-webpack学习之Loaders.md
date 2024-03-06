---
layout: post
title: webpack学习之Loaders
subtitle: 笔记
date: 2022-06-04
author: Wason
header-img: img/bg/post-bg-21.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之Loaders #
### 强大的 Loaders ###  

>一切皆模块

Webpack有一个不可不说的优点，它把所有的文件都当做模块处理，JavaScript代码，CSS和fonts以及图片等等通过合适的loader都可以被处理。

1. 通过使用不同的loader，webpack有能力调用外部的脚本或工具，实现对不同格式的文件的处理。  
比如说分析转换scss为css，或者把下一代的JS文件（ES6，ES7)转换为现代浏览器兼容的JS文件，对React的开发而言，合适的Loaders可以把React的中用到的JSX文件转换为JS文件。  
2. Loaders需要单独安装并且需要在webpack.config.js中的modules关键字下进行配置，Loaders的配置包括以下几方面：  
* test：一个用以匹配loaders所处理文件的拓展名的正则表达式（必须）  
* loader：loader的名称（必须）  
* include/exclude:手动添加必须处理的文件（文件夹）或屏蔽不需要处理的文件（文件夹）（可选）
* query：为loaders提供额外的设置选项（可选）
3. 需要用 webpack 处理转换什么内容就安装对应的内容的loaders
4. 增加解析内容，把Greeter.js 的文本内容改为来自 json文件
5. 更改webpack.config.js 配置文件( 增加babel处理 )

```
module.exports = {
  devtool: 'eval-source-map',
  entry: __dirname+'/app/main.js',
  output: {
    path: __dirname+'/public',
    filename: 'bundle.js'
  },
  devServer:{
    contentBase: './public',
    historyApiFallback: false,
    inline: true
  },
  
  module:{
    rules: [
      {
        test: /(\.jsx|\.js)$/,
        use: {
          loader: "babel-loader",
          options: {
            presets:[
              "env", "react"
            ]
          }
        },
        exclude: /node_modules/
      }
    ]
  }
}
```

***To Be Continue~***

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050

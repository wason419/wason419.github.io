---
layout: post
title: webpack学习之插件[Plugins](缓存)--完结
subtitle: 笔记
date: 2022-06-16
author: Wason
header-img: img/bg/post-bg-27.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之插件\[Plugins\](缓存)--完结 #
***接上文***

6）缓存
使用缓存的最好方法是保证你的文件名和文件内容是匹配的（内容改变，名称相应改变）  
webpack可以把一个哈希值添加到打包的文件名中，使用方法如下,添加特殊的字符串混合体（[name], [id] and [hash]）到输出文件名前:  

```
const webpack = require('webpack');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ExtractTextPlugin = require('extract-text-webpack-plugin');

module.exports = {
..
    output: {
        path: __dirname + "/build",
        filename: "bundle-[hash].js"
    },
   ...
};
```
执行 npm run build 后在build文件夹中，打包后的文件便会带hash后缀：

![](http://wason419.github.io/img/20200616/2020061601.png)

**去除build文件中的残余文件**

添加了hash之后，会导致改变文件内容后重新打包时，文件名不同而内容越来越多，生成待不同hash后缀的文件  
`(需要有文件内容更改，build之后的js才会改变。不然所以文件内容不变，多次执行build后不会替换build文件中的js)`  
因此这里介绍另外一个很好用的插件clean-webpack-plugin。  
① 安装：cnpm install clean-webpack-plugin --save-dev  
② 在webpack.production.config.js文件中 加入clean-webpack-plugin  

~~const CleanWebpackPlugin = require("clean-webpack-plugin");~~  
  plugins: [  
    ...// 这里是之前配置的其它各种插件  
    ~~new CleanWebpackPlugin('build/*.*', {~~  
    ~~root: __dirname,~~  
    ~~verbose: true,~~  
    ~~dry: false~~  
    ~~})~~  
  ]  

但是，按以上方法写会报错：  

```
/Users/hao/node_modules/webpack-cli/bin/cli.js:93
                throw err;
                ^
TypeError: CleanWebpackPlugin is not a constructor
    at Object.<anonymous> (/Users/hao/pro/webpacktest/webpack.production.config.js:52:7)
    at Module._compile (/Users/hao/node_modules/v8-compile-cache/v8-compile-cache.js:192:30)
```

解决方案：网址 https://www.cnblogs.com/baby-zuji/p/11540718.html  
修改两点即可，改为以下方式引用插件  
① const {CleanWebpackPlugin} = require("clean-webpack-plugin”);  
② new CleanWebpackPlugin()  

执行 npm run build 后，build文件夹中的众多打包文件会被清空，修改内容后多次打包也只保留一个打包的js文件，只是文件名的hash发生改变。  

***完结~***

### 参考文章 ###  
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050
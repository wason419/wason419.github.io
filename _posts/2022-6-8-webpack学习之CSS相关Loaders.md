---
layout: post
title: webpack学习之CSS相关Loaders
subtitle: 笔记
date: 2022-06-08
author: Wason
header-img: img/bg/post-bg-23.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之CSS相关Loaders #
### CSS ###
webpack提供两个工具处理样式表，css-loader 和 style-loader
**二者处理的任务不同**
* css-loader使你能够使用类似@import 和 url(...)的方法实现 require()的功能
* style-loader将所有的计算后的样式加入页面中，二者组合在一起使你能够把样式表嵌入webpack打包后的JS文件中
1. 安装相应loader:
```
npm install --save-dev style-loader css-loader
=====>
+ css-loader@3.5.1
+ style-loader@1.1.3
updated 2 packages in 6.206s
```
2. 在webpack.config.js 中 增加css、style 的loader
3. 增加 main.css
4. 在main.js中 引入main.css : import ‘./main.css'

### CSS module ###
CSS模块化：Webpack对CSS模块化提供了非常好的支持，只需要在CSS loader中进行简单配置即可，然后就可以直接把CSS的类名传递到组件的代码中，这样做有效避免了全局污染。

1. webpack.config.js 中对 css-loader增加options配置
```
options: {
    modules: true,
    localIdentName: '[name]__[local]--[hash:base64:5]'
}
```
2. 直接执行启动webpack-serve： npm run serve
```
报错=====>：
ERROR in ./app/Greeter.css (./node_modules/css-loader/dist/cjs.js??ref--5-1!./app/Greeter.css)
Module build failed (from ./node_modules/css-loader/dist/cjs.js):
ValidationError: Invalid options object. CSS Loader has been initialized using an options object that does not match the API schema.
- options has an unknown property 'localIdentName'. These properties are valid:
   object { url?, import?, modules?, sourceMap?, importLoaders?, localsConvention?, onlyLocals?, esModule? }
    at validate (/Users/hao/pro/webpacktest/node_modules/css-loader/node_modules/schema-utils/dist/validate.js:85:11)
...

```
---  
解决方案：修改css-loader的 modules规则定义
```
{
  loader: 'css-loader',
  options: {
    modules: {
      localIdentName: '[name]__[local]--[hash:base64:5]'
    }
  }
}
```
3. 运行 npm run serve 成功
---  
补充：删除public 文件夹中的bundle.js 文件，运行npm run serve。发现本地服务器可以启动,页面正常显示.

但是在 public文件夹中 不会生成 bundle.js文件, 再次执行npm start 就可以生成 bundle.js 文件了  

**总结：** 这两种指令作用是分开的，类似vue项目 执行serve运行，并不会有打包文件dist.  
因此，执行npm run serve 会调用 启动 webpack-serve + entry + module   
执行npm start(打包) 会调用 entry + output 

### CSS预处理器 ###
1. 可以类似css-loader增加相关css预处理器的loader  
2. css的处理平台 postcss: 可使用PostCSS来为CSS代码自动添加适应不同浏览器的CSS前缀
安装postcss-loader 和 autoprefixer（css会自动根据Can i use里的数据自动添加不同前缀的插件）
```
npm install --save-dev postcss-loader autoprefixer
=====>
+ postcss-loader@3.0.0
+ autoprefixer@9.7.6
added 31 packages from 23 contributors in 6.478s
```
---  
修改webpack.config.js文件 增加postcss-loader配置，并增加 postcss.config.js配置文件
```
webpack.config.js=====>
...
{
  loader: "postcss-loader"
}
...

postcss.config.js=====>
/*postcss.config.js*/

module.exports = {
  plugins:[
    require('autoprefixer')
  ]
}
```  
执行 npm start (打包后的bundle.js文件里 需要增加浏览器前缀的css属性，会自动增加前缀)  
**`npm run serve`**


***To Be Continue~***

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050
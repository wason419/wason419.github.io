---
layout: post
title: webpack学习之Babel
subtitle: 笔记
date: 2022-06-06
author: Wason
header-img: img/bg/post-bg-22.jpg
catalog: true
tags:
  - webpack
---

# webpack学习之Babel #
### Babel ###
1. Babel其实是一个编译JavaScript的平台，它可以编译代码帮你达到以下目的：
让你能使用最新的JavaScript代码（ES6，ES7...），而不用管新标准是否被当前使用的浏览器完全支持；
让你能使用基于JavaScript进行了拓展的语言，比如React的JSX；

2. Babel 其实是几个模块的包，核心包是：babel-core 
>webpack可以把其不同的包整合在一起使用，对于每一个需要的功能或拓展，需要安装单独的包 ( 用得最多的是解析Es6的 babel-preset-env 包和解析JSX的 babel-preset-react 包)

3. npm一次性安装多个依赖模块，模块之间用空格隔开
```
npm install --save-dev babel-core babel-loader babel-preset-env babel-preset-react

安装后得到的版本是：
+ babel-preset-env@1.7.0
+ babel-loader@8.1.0
+ babel-core@6.26.3
+ babel-preset-react@6.24.1
updated 4 packages in 7.705s
```

4. 这时直接运行 npm run serve，运行本地服务器，会报错：

```
ERROR in ./app/main.js
Module build failed (from ./node_modules/babel-loader/lib/index.js):
Error: Cannot find module '@babel/core'
babel-loader@8 requires Babel 7.x (the package '@babel/core'). If you'd like to use Babel 6.x ('babel-core'), you should install 'babel-loader@7'.
    at Function.Module._resolveFilename (internal/modules/cjs/loader.js:581:15)
    at Function.Module._load (internal/modules/cjs/loader.js:507:25)
    at Module.require (internal/modules/cjs/loader.js:637:17)
    at require (internal/modules/cjs/helpers.js:22:18)
    ... 

```

解决方案：根据提示 更新 babel-loader 版本  
npm uninstall babel-loader -D  
npm i babel-loader@7 -D  

即可 使用babel处理 es6语法

---

5. 改使用React，记得先安装 React 和 React-DOM
安装：npm install --save react react-dom

6. 更改Greeter.js 并返回为组件，更改main.js 为入口文件
运行server： npm run serve

7. 抽离babel配置内容在 .babelrc文件： touch .babelrc
```
//.babelrc
{
  "presets": ["react", "env"]
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
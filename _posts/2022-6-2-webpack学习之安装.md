---
layout: post
title: webpack学习之安装
subtitle: 笔记
date: 2022-06-02
author: Wason
header-img: img/bg/post-bg-20.JPG
catalog: true
tags:
  - webpack
---

# webpack学习之安装 #
### 概念 ###
Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。
### 安装 ###
1.webpack 官网建议不要 全局安装，因此先清空 全局的webpack。

```
npm -g uninstall webpack ：卸载webpack
```
![](http://wason419.github.io/img/20200602/2020060201.png)

2.清完后 ，开始安装并使用webpack

① 新建文件夹：webpacktest  
② 创建package.json文件：npm init  
③ 安装webpack（在node_modules文件夹中，同时会生成package-lock.json文件）：  
```
npm install --save-dev webpack
```
`控制台查询webpack的版本( # 在非全局安装webpack情况下 )   `
`=====>`
`192:webpacktest hao$ node_modules/.bin/webpack -v`
`4.42.1`

---  
④ 创建app和public文件夹，并创建文件：  
* **index.html** --放在public文件夹中;  
* **Greeter.js**-- 放在app文件夹中;  
* **main.js**-- 放在app文件夹中;  
**bundle.js文件由 webpack转换并打包生成，代码内容如下**  

---
```
<!-- index.html -->
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Webpack Sample Project</title>
  </head>
  <body>
    <div id='root'>
    </div>
    <script src="bundle.js"></script>
  </body>
</html>
————————————————————————————————————————————————
// Greeter.js
module.exports = function() {
  var greet = document.createElement('div');
  greet.textContent = "Hi there and greetings!";
  return greet;
};
————————————————————————————————————————————————
//main.js
const greeter = require('./Greeter.js');
document.querySelector("#root").appendChild(greeter());
```

![](http://wason419.github.io/img/20200602/2020060202.png)

⑤ 开始执行webpack 的转换和打包功能：  

**{extry file}出填写入口文件的路径，本文中就是上述main.js的路径，**  
**{destination for bundled file}处填写打包文件的存放路径**  
**填写路径的时候不用添加{}**  
`webpack {entry file} {destination for bundled file}`  

执行命令： # webpack非全局安装的情况下不能直接使用webpack,需要在具体的目录文件夹下执行webpack指令：  

~~node_modules/.bin/webpack app/main.js public/bundle.js~~ 

上面指令是老版本指令，执行的话会报错:  
=====>  

ERROR in multi ./app/main.js public/bundle.js  
Module not found: Error: Can't resolve 'public/bundle.js' in '/Users/hao/pro/webpacktest'  
@ multi ./app/main.js public/bundle.js main  

---

解决方案：  
在4.x的版本下是不行的，要改为：node_modules/.bin/webpack app/main.js -o  public/bundle.js  
成功在public文件夹下生成 bundle.js  

⑥ 执行public文件夹下 index.html文件，可看到‘Hi there and greetings!’内容。   

![安装完成](http://wason419.github.io/img/20200602/2020060203.png)  

如上便安装完成~  
To Be Continue  

### 参考文章 ###
[1. 入门 webpack，看这篇就够了][1]  
[2. webpack 官网][2]  
[3. webpack坑系列--安装webpack-cli][3]  

[1]: https://segmentfault.com/a/1190000006178770
[2]: https://www.webpackjs.com/guides/getting-started/
[3]: https://segmentfault.com/a/1190000013699050




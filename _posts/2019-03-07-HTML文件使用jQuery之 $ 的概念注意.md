---
layout:     post
title:      HTML文件使用jQuery之 $ 的概念注意
subtitle:   
date:       2019-03-07
author:     Wason
header-img: img/bg/post-bg-03.jpg
catalog: 	 true
tags:
  - jQuery
---
# 前言
HTML文件调用 jQuery，要有jQuery的对象概念

# 正文

**`$.xxx.js`**：表示jquery对象调用`xxx.js`文件，且`xxx.js`文件是个函数，形式为
`（function(...)）（jQuery ）`，`xxx.js`可以是自定义js文件。

另外，要实现 `$.xxx.js` 这种调用，则 在HTML的 head 写入引用时，要把jQuery的引用写在调用`($.xxx.js)`的js文件引用之前。

***引用网文***： 

JS是解释型语言，是根据标签引用分块顺序从上往下执行的，$是jQuery中的产生的对象，需要用的话，必须将jquery.js引用 放在使用它的js方法前面。

而若报`jQuery is not defined` 错误的可能原因：

* ①下载的jQuery文件有问题
* ②引用路径问题
* ③如上位置问题
* ④引用网址，网站链接问题

---
layout: post
title: JQuery的on()监听方法使用
subtitle: 笔记
date: 2019-12-05
author: Wason
header-img: img/bg/post-bg-05.jpg
catalog: true
tags:
  - jQuery
---

# JQuery的on()监听方法使用 #
### 参考网文 ###
[jquery.on()超级方法][1]

**归纳**
在jquery的on方法中实现事件委托就更简单了，on方法可以接受三个参数：
第一个参数是事件名，可以只绑定一个事件，如on('click'),也可以绑定多个事件，如on('click dbclick mouseover')等  
```
$('ul').on('click mouseover',eventHandler); // 绑定多个事件  
```
第二个参数是可选参数，接受一个selector,当事件触发元素符合selector时，会调用事件处理函数
```
$('ul').on('click mouseover','li',eventHandler); // 触发元素是li时执行eventandler  
$('ul').on('click mouseover', 'li:even',eventHandler); // 触发元素是li时,而且元素是第偶数个时执行eventandler
 ```
`注：此处用到 li:even 选择器，后面有注解`
第三个参数是自定义事件处理的回调函数。

#### JQuery的ON()方法支持的所有事件罗列 ###
```
blur
focus
focusin
focusout
load
resize
scroll
unload
click
dblclick
mousedown
mouseup
mousemove
mouseover
mouseout
mouseenter
mouseleave
change
select
submit
keydown
keypress
keyup
error
contextmenu
```

### 拓展知识点 ###
1.jQuery :even 选择器
选取每个带有偶数 index 值的元素（比如 2、4、6）  
index 值从 0 开始，所有第一个元素是偶数 (0)  
![](http://wason419.github.io/img/20191205/2019120501.png)

2.jQuery :odd 选择器  
选取每个带有奇数 index 值的元素（比如 1、3、5）  
![](http://wason419.github.io/img/20191205/2019120502.png)



[1]: https://www.bbsmax.com/A/pRdBgO7znx/

---
layout:     post
title:      JavaScript中的迭代器与循环
subtitle:   
date:       2019-01-02
author:     BY
header-img: img/bg/post-bg-01.jpg
catalog: 	 true
tags:
  - JS
---

# 前言
讲到循环操作，大家一般会想到`for(var i=0;i<maxlength;i++){}`，但除此之外，JS还有其他的循环方法，比如`forEach、for-in、for-of`方法，下面我这里就做下记录说明和拓展

# 正文

## forEach

**forEach**： array.forEach( function(){ } )

**for-in**：主要讲下它的几个缺点

* 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等等。如果你使用字符串的 index 去参与某些运算（"2" + 1 == "21"），运算结果可能会不符合预期。
* for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
* 某些情况下，for...in循环会以任意顺序遍历键名。
* 总之，for...in循环主要是为遍历对象而设计的，不适用于遍历数组。

**for-of**

## 总结

for…in与for…of 
语法格式对比：

```
for (var key in arr){
    console.log(arr[key]);
}

for (var value of arr){
    console.log(value);
}
```
推荐在**循环对象属性**的时候，使用for…in,

在**遍历数组**的时候的时候使用for...of。

因为 for...in是遍历键名，for...of是遍历键值。

注意，for...of是ES6新引入的特性。修复了ES5引入的for...in的不足。

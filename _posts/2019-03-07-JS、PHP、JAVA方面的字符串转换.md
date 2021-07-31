---
layout:     post
title:      JS、PHP、JAVA方面的字符串转换
subtitle:   
date:       2019-03-07
author:     Wason
header-img: img/bg/post-bg-04.jpg
catalog: 	 true
tags:
  - 字符串转换
---

# 正文

## JS方面
 
①数组变成字符串：

```
var a, b;

a = new Array(0,1,2,3,4);

b = a.join("-");      //"0-1-2-3-4"

```

②字符串变成数组：

```
var s = "abc,abcd,aaa";

ss = s.split(",");// 在每个逗号(,)处进行分解  ["abc", "abcd", "aaa"]

var s1 = "helloworld";

ss1 = s1.split('');  //["h", "e", "l", "l", "o", "w", "o", "r", "l", "d"]
```

split(”,”,3)第二个参数 3表示 分割后数组中每个元素的内容长度为3！

## PHP方面

①数组变成字符串：

```
$array = array('lastname', 'email', 'phone');
$comma_separated = implode(",", $array);      // lastname,email,phone
```

②字符串变成数组：

```
$pizza  = "piece1 piece2 piece3 piece4 piece5 piece6";

$pieces = explode(" ", $pizza);
```

## JAVA 方面

int 转为 String:

* num+“”
* String.valueOf(num)
* Integer.toString(num)


String 转为 int:

* Integer.parseInt(str)
* Integer.valueOf(str).intValue( );

另外，JAVA里，String 判断 字符是否相等 要用equals( )，不能用"=="，否则会报错

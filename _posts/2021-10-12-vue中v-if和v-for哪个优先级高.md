---
layout: post
title: vue中v-if和v-for哪个优先级高
subtitle: 笔记
date: 2021-10-12
author: Wason
header-img: img/bg/post-bg-14.jpg
catalog: true
tags:
  - Vue
  - v-if/v-for
---

# vue中v-if和v-for哪个优先级高 #

## 前言 ##
1.在vue2中，v-for的优先级高于v-if；在vue3中，v-if的优先级高于v-for   
2.**`在vue中，永远不要把v-if和v-for同时用在同一个元素上，会带来性能方面的浪费`**（每次渲染都会先循环再进行条件判断）；想要避免出现这种情况，可在外层嵌套template（页面渲染不生成dom节点），在这一层进行v-if判断，然后在内部进行v-for循环   

- v-if 指令用于条件性地渲染一块内容。指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 true值的时候被渲染
- v-for 指令基于一个数组来渲染一个列表。v-for指令需要使用item in items形式的特殊语法，其中items是源数据数组或者对象，而item则是被迭代的数组元素的别名

- 在v-for的时候，建议设置key值，并且保证每一个key值都是独一无二的，这便是diff算法进行优化

```
<Modal v-if="isShow" />

<li v-for="item in items" :key="item.id">
    {{ item.label }}
</li>
```

## 优先级 ##

其实在vue2和vue3中的答案是截然相反的。

- 在vue2中，v-for的优先级高于v-if

- 在vue3中，v-if的优先级高于v-for

因此，切记 **永远不要把v-if和v-for同时用在同一个元素上** ！！！


---
layout: post
title: Vue3的VDOM内部是做了什么, 使得比vue2更快
subtitle: 笔记
date: 2022-03-02
author: Wason
header-img: img/bg/post-bg-24.jpg
catalog: true
tags:
  - Vue
---

# Vue3的VDOM内部是做了什么, 使得比vue2更快 #

## 概述 ##
关于Vue3 比 Vue2 加强的内容,  总结来说, 可分4点描述：【与VDOM较为相关的是1、2、3】   

1. [提升了渲染速度](#anc1)
2. [提升了更新速度](#anc2)
3. [减少了内存占用](#anc3)
4. [cacheHandlers 事件侦听器缓存](#anc4)
5. [增加了 typescript 的支持, 增强了代码可读性和可维护性](#anc5)
6. [推出了 新特性：composition API（组合式API）](#anc6)


## 详述 ##  

<a id='anc1'></a>  

### 1. 提升了渲染速度 ###  
- 模板编译器（SSR渲染）：vue3 采用了模板编译器实现了更快的渲染速度, 模板编译器 可将传统基于字符串的模板 编译为一组可复用的JavaScript渲染函数, 从而提高渲染速度  

<a id='anc2'></a>   

### 2. 提升了更新速度 ###  
- 静态标记：vue3 通过标记静态节点, 和对动态节点进行优化, 同时也对update和mounted 等进行钩子的分离, 也从而提高了更新的速度  

<a id='anc3'></a>

### 3. 减少了内存占用 ###
- 静态提升：vue3 引入了静态提升机制, 将静态子树的生成 放到 编译阶段, 在页面更新时跳过不需要更新的内容, 从而减少内存的占用  

<a id='anc4'></a>

### 4. cacheHandlers 事件侦听器缓存 ###
1. vue 2 默认情况下onClick会被视为动态绑定, 所以每次都会去追踪它的变化  
2. vue3 会认为是同一个函数, 所以没有追踪变化, 直接缓存起来复用即可  

<a id='anc5'></a>

### 5. 增加了 typescript 的支持, 增强了代码可读性和可维护性 ###
- 提供了typescript的支持

<a id='anc6'></a>

### 6. 推出了 新特性：composition API（组合式API） ###
1. setup配置  
2. ref 与 reactive  
  `ref和reactive的对比`  
    定义  
    - ref ：定义基本数据类型  
    - reactive：定义对象或数组类型的数据  

    原理  
    - ref：通过Object.defineProperty()的get和set来实现响应式（数据劫持）  
    - reactive：通过Proxy来实现响应式（数据劫持）, 并通过Reflect操作源对象内部数据  

    使用  
    - ref定义的数据：操作数据需要 .value , 读取数据时模板中不需要 .value  
    - reactive定义的数据：操作和读取都不需要 .value  
    - 补: `一般使用 reactive 较多, 可以将基本类型的数据封装在对象里, 然后再使用reactive`  

3. watch与watchEffect `( watchEffect 不同watch需要定义监听的对象, 而是类似computed 只要函数有涉及的对象数据发生变化, 便会主动调用 ) `
4. provide与inject  
  
  
  
### 参考文章 ###
[1. 为什么vue3.0相对于vue2.0来说性能快1.2到1.5倍？][1]  
[2. 详解Vue3中对VDOM的改进][2]  
[3. Vue3新特性——Composition API详解][3]

[1]: https://blog.csdn.net/weixin_65785071/article/details/125293065
[2]: https://pythonjishu.com/vqgnrnblhxskmvz/
[3]: https://blog.csdn.net/ailsa_csdn/article/details/123166534
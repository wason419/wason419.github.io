---
layout: post
title: Vue的data为什么是函数？Vue内部是怎么处理的？
subtitle: 笔记
date: 2022-2-20
author: Wason
header-img: img/bg/post-bg-23.jpg
catalog: true
tags:
  - Vue
---

# Vue的data为什么是函数？Vue内部是怎么处理的？ #

- Vue的data定义需要以【函数形式】定义，其格式为data函数内部 返回一个数据对象。
- 因为vue内部的处理机制是 组件实例化是 判断data是否为 function，如果是function则获取函数内部对象；若不是function 则直接使用data数据(对象)初始化，
- 因此，使用【函数形式】定义的原因是，每次组件实例化时，均会调用 data 函数，其函数会返回一个 data对象，计算机会为这个对象分配单独的内存地址，实例化几次便分配几个内存地址，从而使得每个实例化数据间相互独立，互不干扰，保证了组件的独立性和可复用性。
- 若使用【对象形式】定义，则每个组件实例化时 , 赋值的是同一个内存地址引用，会造成数据的影响/污染。
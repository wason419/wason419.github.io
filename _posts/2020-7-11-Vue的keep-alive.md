---
layout: post
title: Vue的keep-alive
subtitle: 笔记
date: 2020-07-11
author: Wason
header-img: img/bg/post-bg-07.jpg
catalog: true
tags:
  - Vue
---

# Vue的keep-alive #
### keep-alive的应用 ###
作用：keep-alive是Vue内置的一个组件，可以使比包含的组件保留状态，或避免重新渲染，而router-view也是一个组件，如果直接被包在keep-alive里面，所有的路径匹配到的视图组件都会被缓存

### Vue2.X生命周期 ###
· 初次进入时：created > mounted > activated；退出后触发 deactivated
· 再次进入：会触发 activated；事件挂载的方法等，只执行一次的放在 mounted 中；组件每次进去执行的方法放在 activated 中
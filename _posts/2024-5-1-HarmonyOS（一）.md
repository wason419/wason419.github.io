---
layout: post
title: HarmonyOS（一）
subtitle: 笔记
date: 2024-5-1
author: Wason
header-img: img/bg/post-bg-20.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（一）#

项目文件目录指南

* AppScope > app.json5：应用的全局配置信息
* entry：HarmonyOS工程模块，编译构建生成一个HAP包
    * src > main > ets：用于存放ArkTS源码
    * src > main > ets > entryability：应用/服务的入口
    * src > main > ets > pages：应用/服务包含的页面
    * src > main > resources：用于存放应用/服务所用到的资源文件，如图形、多媒体、字符串、布局文件等
    * src > main > module.json5：Stage模型模块配置文件。主要包含HAP包的配置信息、应用/服务在具体设备上的配置信息 以及 应用/服务的全局配置信息
    * build-profile.json5：当前的模块信息、编译信息配置项，包括buildOption、targets配置等。其中targets中可配置当前运行环境，默认为HarmonyOS
    * hvigorfile.ts：模块级编译构建任务脚本，开发者可以自定义相关任务和代码实现。
* oh_modules：用于存放三方库依赖信息
* build-profile.json5：应用级配置信息，包括签名、产品配置等
* hvigorfile.ts：应用级编译构建任务脚本
* package.json：应用的三方包依赖，支持HAR（遵循npm标准规范）和npm包的依赖

页面路由配置文件目录：
src > main > resources > base> profile > main_pages.json

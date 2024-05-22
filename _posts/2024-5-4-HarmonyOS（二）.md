---
layout: post
title: HarmonyOS（二）
subtitle: 笔记
date: 2024-5-4
author: Wason
header-img: img/bg/post-bg-21.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（二） #  

开启HarmonyOS学习之旅   
结合官网开发教学的配置，对HarmonyOS 的前端入门学习，主要在与 DevEco工具的使用、ArkUI框架的熟悉、 ArkTS开发语法的学习  
![](/img/20240504/2024050401.png)   

一、DevEco工具的使用  
1. 在官网下载匹配电脑的最新DevEco   
[DevEco Studio 3.1.1 Release][1]     
2. 安装好对应开发工具后，需先检验系统配置，如存在“ Set it up now” 则可点击 参照相关步骤安装默认配置（在win电脑上存在Node.js 和 ohpm 的资源引用问题，明明本地电脑Node.js 的版本符合要求，但软件却始终无法找到资源。故后期改用mac电脑进行开发，相关资源引用上可正常下载与配置）  
![](/img/20240504/2024050402.png)    

3. 开发环境与开发工具配置完成后，可参考官网文档开启学习  
[入门][2]    
  
二、 ArkUI （方舟开发） 框架的熟悉    
方舟开发框架（简称ArkUI）是一套构建分布式应用界面的声明式UI开发框架。   
① 它使用极简的UI信息语法、丰富的UI组件、以及实时界面预览工具，可提升HarmonyOS应用界面开发效率30%。   
② 为HarmonyOS应用的UI开发提供了完整的基础设施，包括简洁的UI语法、丰富的UI功能（组件、布局、动画以及交互事件），以及实时界面预览工具等，可以支持开发者进行可视化界面开发。   
③ 它同HarmonyOS的应有模型（Stage模型、FA模型）一样，也分为两种开发范式：基于ArkTS的声明式开发范式（简称“声明式开发范式”）和 兼容JS的类Web开发范式（简称“类Web开发范式”）   

声明式开发范式：采用基于TypeScript声明式UI语法扩展而来的ArkTS语言，从组件、动画和状态管理三个维度提供UI绘制能力。   
类Web开发范式：采用经典的HML、CSS、JavaScript三段式开发方式，即使用HML标签文件搭建布局、使用CSS文件描述样式、使用JavaScript文件处理逻辑。该范式更符合于Web前端开发者的使用习惯，便于快速将已有的Web应用改造成方舟开发框架应用。   

官方较为推荐使用“声明式开发范式”   
```
在开发一款新应用时，推荐采用声明式开发范式来构建UI，主要基于以下几点考虑：
开发效率：声明式开发范式更接近自然语义的编程方式，开发者可以直观地描述UI，无需关心如何实现UI绘制和渲染，开发高效简洁。
应用性能：如下图所示，两种开发范式的UI后端引擎和语言运行时是共用的，但是相比类Web开发范式，声明式开发范式无需JS框架进行页面DOM管理，渲染更新链路更为精简，占用内存更少，应用性能更佳。
发展趋势：声明式开发范式后续会作为主推的开发范式持续演进，为开发者提供更丰富、更强大的
```
![](/img/20240504/2024050403.png)    

不同应用类型支持的开发范式   
根据所选用HarmonyOS应用模型（Stage模型、FA模型）和页面形态（应用或服务的普通页面、卡片）的不同，对应支持的UI开发范式也有所差异，详见下表。   
![](/img/20240504/2024050404.png)    

基于后续发展与稳定考虑，关于HarmonyOS学习 将以Stage模型、声明式开发范式 进行。

### 参考文章 ###  
[HarmonyOS 第一课][3]  


[1]: https://developer.harmonyos.com/cn/develop/deveco-studio/#download
[2]: https://developer.huawei.com/consumer/cn/doc/harmonyos-guides-V2/start-overview-0000001478061421-V2
[3]: https://developer.harmonyos.com/cn/documentation/teaching-video/
---
layout: post
title: ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API
subtitle: 笔记
date: 2022-11-10
author: Wason
header-img: img/bg/post-bg-16.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API #
![](/img/20210702/2021070200.jpg)   

## 概述 ##   
> 本节分享下 ArcGIS API for JavaScript, 后续简述为 ArcGIS JS API 。    
其便是 在浏览器端实现GIS功能（WebGIS）的 API 技术支持。  

[1. ArcGIS API for JavaScript的版本区别](#anc1)  
[2. ArcGIS API for JavaScript的引入方式](#anc2)  
[3. ArcGIS API for JavaScript基础概念讲解](#anc3)
[4. require 和 loadModules 方法的区别](#anc4)

## 详述 ##  
<a id='anc1'></a>
### 1. ArcGIS API for JavaScript的版本区别 ###  

**1.1 ArcGIS JS API的不同版本**  

[① ArcGIS API for JavaScript文档 4.22版本首页][1]  
[② ArcGIS API for JavaScript文档 4.22版本 API 参考列表 ][2]   
[③ ArcGIS API for JavaScript文档 3.39版本首页][3]   
[④ ArcGIS API for JavaScript文档 3.39版本 API 参考列表 ][4]    
[⑤ ArcGIS的 中文知乎社区][5]   

>`ArcGIS 官网原文为 英文文档，下面将会有部分官网界面的截图，有些截图里的文字内容为中文，均为浏览器页面翻译。故可能存在内容/文字翻译不恰当问题，一切以官网原文为准`  

如上面网址所示，ArcGIS JS API 目前的大版本有两个，分别是 3.x 版本 和 4.x 版本。3.x 版本是早先发布的版本，里面对二维地图的操控等比较详细，4.x 版本是后来发布的版本，主要增加了三维地图场景的内容，目前这两个版本同时更新，3.x 版本目前最新版是 3.39，4.x 版本目前最新版是 4.22。而两者的区别在官网也有说明：[Choose between version 3.x and 4.x][6]   

![官网的版本说明](/img/20210710/2021071001.png)  

下图是根据官网的版本说明，做的内容整理：  

![自定义版本说明](/img/20210710/2021071003.png)  

**1.2 ArcGIS JS API的版本选择**  

总而言之，对ArcGIS JS API版本的选择标准可以参考如下：   
1. 应用程序是否需要 3D 可视化？如果是，请使用 4.x   
2. 是否正在处理非常大的要素图层？如果是，请使用 4.x    
3. 是否需要使用到 3.x 中已有，但尚未在 4.x 中可用的特定功能，例如分析小部件等，如果是，请使用 3.x  
4. 其他情况可以根据官网的具体区分，进行比较抉择   

这边分享一份 Esri 的官方资料，下面图片为内容截图 : [Esri 对于 ArcGIS API for JavaScript 的技术支持说明][7]  

![](/img/20210710/2021071004.png)![](/img/20210710/2021071005.png)

> 个人觉得，目前 4.x 还在不断的补充 自身尚未实现但在 3.x 中已发布的功能 API ，并且，发展趋势上看，3.x 在不久后会被逐步淘汰，4.x 在功能支持上也必定将超过 3.x ，故 如果项目还未使用过 3.x 的，可以考虑上手 4.x 。   

---

<a id='anc2'></a>
### 2. ArcGIS API for JavaScript的引入方式 ###   

**2.1 多种引入方式**  
关于ArcGIS JS API 的引入方式，在官方文档已说明，详情可见下面两篇：[Install and set up][8], [Introduction to tooling][9]   
由上文可知， 常用的有以下几种：   
1. 通过 ArcGIS CDN 的 AMD 模块  
```
  访问 API 的最常见方法是使用托管版本。从 CDN 中引用 API 和 CSS，在应用程序中使用 API.

  <link rel="stylesheet" href="https://js.arcgis.com/4.22/esri/themes/light/main.css">  
  <script src="https://js.arcgis.com/4.22/"></script>
```
2. 通过 NPM 的 ES 模块  
```
  可通过 JavaScript 包管理器npm作为 ES 模块使用, 因此，它可以结合到 Vue + webpack 框架中一起使用.

  安装:     npm install @arcgis/core
  导入模块： import Map from "@arcgis/core/Map" 
```
3. 通过 CDN 的 ES 模块  
```
  注意：此方法目前仅推荐用于开发和原型设计，与第一种方式在 JS 的引用上不一致。

  <link rel="stylesheet" href="https://js.arcgis.com/4.22/@arcgis/core/assets/esri/themes/light/main.css">
  <script type="module">
    import Map from "https://js.arcgis.com/4.22/@arcgis/core/Map.js";

    // Use the Map class
  </script>  
```
4. 其他方式...   


**2.2 比较 AMD 和 ES 模块方式**  
从上文可知，ArcGIS JS API 可用作 AMD 和 ES 模块来引入   
从 4.0 版本开始，API 是作为 AMD 提供，例如第一种方式的 CDN 使用 AMD 模块   
从 4.18 版本开始，API 也可用作 ES 模块(我便是看到新版本的支持，才决定选择采用 Vue + ArcGIS 方式进行学习研究)     

<p style="color: #cc4125;">AMD 模块实现了异步模块定义格式，它们使用 require() 方法和 第三方脚本加载程序 加载模块及其依赖项</p>
<p style="color: #ff9900;">ES 模块，也称为 ECMAScript 模块或简称 ESM，是一种官方的标准化模块系统，引用方式如上面第二种，它通过import语句在所有现代浏览器中本地工作。ES 模块不需要单独的脚本加载器</p>

> 如果是将 4.18+ 与框架或构建工具一起使用，并且没有使用 Dojo 1 或 RequireJS等，那么应该使用 ES 模块构建。  

**2.3 构建项目**  
目前网上涉及ArcGIS JS API的文章资料，要么就是 3.x 的，要么便是使用 4.17 版本或更早版本的 API, 因而只能参考借鉴，更多的还是要增加对官方文档的理解以及查找相关文章来参考。  

若是使用 4.17 甚至更早的版本的，便是采用 AMD 模块方式，该方式的项目需要引用库 esri-loader 并使用 第三方加载器加载依赖。[此处说明下 require() 和 loadModules() 的区别](#anc4)。并且，由于版本限制，网上大部分项目是采用多页面/原生代码方式开发，因而还需要将 ArcGIS 项目放置在 本地IIS服务器、Tomcat、Nginx等本地服务器环境下运行。以上环境搭建的相关文档 可自行百度。下面分享几篇参考文章:   
① [ArcGIS API for JavaScript 4.13 本地IIS服务器部署][10]
② [Win7安装SMTP服务的方法][11]

而我练习的项目的搭配是：Vue2.x + ArcGIS JS API 4.22 + ESM，在直接命令行指令创建Vue项目后，参照上文第二种方式引用 ArcGIS , 写完代码后直接 npm run serve 运行即可。具体内容将会下一节详细介绍。  


<a id='anc3'></a>
### 3. ArcGIS API for JavaScript基础概念讲解 ###  



<a id='anc4'></a>
### 4. require 和 loadModules 方法的区别 ###  




[1]: https://developers.arcgis.com/javascript/latest/
[2]: https://developers.arcgis.com/javascript/latest/api-reference/
[3]: https://developers.arcgis.com/javascript/3/
[4]: https://developers.arcgis.com/javascript/3/jsapi/
[5]: http://zhihu.geoscene.cn/
[6]: https://developers.arcgis.com/javascript/latest/choose-version/
[7]: https://support.esri.com/en/Products/Developers/web-apis/arcgis-api-for-javascript/#product-support
[8]: https://developers.arcgis.com/javascript/latest/install-and-set-up/
[9]: https://developers.arcgis.com/javascript/latest/tooling-intro/
[10]: https://blog.csdn.net/litong149/article/details/105925479/
[11]: https://blog.csdn.net/weixin_33906657/article/details/93615609





---
layout: post
title: ArcGIS API for JavaScript 4.22学习 - API文档
subtitle: 笔记
date: 2021-07-10
author: Wason
header-img: img/bg/post-bg-16.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习 - API文档 #
![](/img/20210702/2021070200.jpg)   

## 概述 ##  
下面我们开始 ArcGIS API for JavaScript 官网API文档的学习。  

[1. ArcGIS API for JavaScript的版本区别](#anc1)  
[2. ArcGIS API for JavaScript的引入方式](#anc2)  

## 详述 ##  
<a id='anc1'></a>
### 1. ArcGIS API for JavaScript的版本区别 ###  
涉及到的相关网址：  
[① ArcGIS API for JavaScript文档 4.22版本首页][1]  
[② ArcGIS API for JavaScript文档 4.22版本 API 参考列表 ][2]   
[③ ArcGIS API for JavaScript文档 3.39版本首页][3]   
[④ ArcGIS API for JavaScript文档 3.39版本 API 参考列表 ][4]    
[⑤ ArcGIS的 中文知乎社区][5]   

`ArcGIS 官网原文为 英文文档，下面将会有部分官网界面的截图，截图中文字内容为中文的，均为浏览器页面翻译。故可能存在内容/文字翻译不恰当问题，一切以官网原文为准`

如上面网址所示，ArcGIS API for JavaScript目前的大版本有两个，分别是 3.x 版本 和 4.x 版本。3.x 版本是原来最早发布的版本，里面对二维地图的操控等比较详细，4.x 版本是后来发布的版本，主要增加了三维地图场景的内容，目前这两个版本同时更新，3.x 版本目前最新版是 3.39，4.x 版本目前最新版是 4.22。而两者的区别在官网也有说明：[Choose between version 3.x and 4.x][6]   

![官网的版本说明](/img/20210710/2021071001.png)  

下图是根据官网的版本说明，做的内容整理：  

![自定义版本说明](/img/20210710/2021071003.png)  

**ArcGIS JS API的版本选择**  
`ArcGIS JS API 表示的是 ArcGIS API for JavaScript`  

总而言之，对ArcGIS JS API的版本选择可以参考如下：  
1. 应用程序是否需要 3D 可视化？如果是，请使用 4.x  
2. 是否正在处理非常大的要素图层？如果是，请使用 4.x  
3. 是否需要使用到 3.x 中已有，但尚未在 4.x 中可用的特定功能，例如分析小部件等，如果是，请使用 3.x  
4. 其他情况可以根据官网的具体区分，进行比较抉择  

这边分享一份 Esri 的官方资料，下面的图片为内容截图 : [Esri 对于 ArcGIS API for JavaScript 的技术支持说明][7]  

![](/img/20210710/2021071004.png)![](/img/20210710/2021071005.png)

> 个人觉得，目前 4.x 在不断补充跟 3.x 相比还欠缺的功能支持，并且，发展趋势上看，3.x 在不久后会被淘汰，4.x 在功能支持上也必定将超过 3.x ，故 如果项目还未使用过 3.x 的，可以考虑上手 4.x 

---

<a id='anc2'></a>
### 2. ArcGIS API for JavaScript的引入方式 ###   

**多种引入方式**  
关于ArcGIS JS API 的引入方式，在官方文档已说明，详情可见下面两篇：[Install and set up][8], [Introduction to tooling][9]   
由上文可知， 常用的有以下几种：   
1. 通过 ArcGIS CDN 的 AMD 模块  
```
  访问 API 的最常见方法是使用托管版本。从我们的 CDN 中引用 API 和 CSS，开始在您的应用程序中使用 API.

  <link rel="stylesheet" href="https://js.arcgis.com/4.22/esri/themes/light/main.css">  
  <script src="https://js.arcgis.com/4.22/"></script>
```
2. 通过 NPM 的 ES 模块  
```
  可通过JavaScript 包管理器npm作为 ES 模块使用, 因此，它可以结合到Vue + webpack 框架中一起使用.
  安装: npm install @arcgis/core
  导入模块: import Map from "@arcgis/core/Map" 
```
3. 通过 CDN 的 ES 模块  
```
  注意：此方法目前仅推荐用于开发和原型设计。

  <link rel="stylesheet" href="https://js.arcgis.com/4.22/@arcgis/core/assets/esri/themes/light/main.css">
  <script type="module">
    import Map from "https://js.arcgis.com/4.22/@arcgis/core/Map.js";

    // Use the Map class
  </script>  
```
4. 其他方式。   


**比较 AMD 和 ES 模块方式**  
ArcGIS API for JavaScript 可用作 AMD 和 ES 模块。   
从 4.0 版本开始，API 是作为 AMD 提供，例如第一种方式的 CDN 使用 AMD 模块。   
从 4.18 版本开始，API 也可用作 ES 模块。我便是看到新版本的支持，才决定选择采用 Vue + ArcGIS 方式学习研究.     

“AMD 模块实现了异步模块定义格式，它们使用require()方法和第三方脚本加载器来加载模块及其依赖项。   
ES 模块，也称为 ECMAScript 模块或简称 ESM，是一种官方的标准化模块系统，它通过import语句在所有现代浏览器中本地工作。ES 模块不需要单独的脚本加载器。”   

> 如果是将 4.18+ 与框架或构建工具一起使用，并且没有使用 Dojo 1 或 RequireJS等，那么应该使用 ES 模块构建。 


**构建项目**  
目前网上涉及ArcGIS JS API的文章资料，要么就是 3.x 的，不然便是使用 4.17 版本或更早版本的 API, 因而只能参考借鉴，更多的还是要靠自己对官网说明的理解以及在网上搜寻相关资料案例。  

若是采用 4.17 版本甚至更早的版本，基本上便是采用 AMD 模块方式，项目需要引用库 esri-loader 并使用 require() 加载依赖。并且，由于版本限制，网上大部分项目是采用多页面/原生代码方式开发，因而还需要将 ArcGIS 项目放置在 本地IIS服务器、Tomcat、Nginx等本地服务器环境下运行。环境配置的相关文档 可自行百度。   

而我使用的是 Vue + ArcGIS 方式，项目开发起来比较方便，比较符合以往的开发习惯。  
我项目的搭配是：Vue2.x + ArcGIS JS API 4.22；直接命令行指令创建项目后，再参照上文第二种方式引用 ArcGIS , 编写完项目后，直接 npm run serve 即可。    









[1]: https://developers.arcgis.com/javascript/latest/
[2]: https://developers.arcgis.com/javascript/latest/api-reference/
[3]: https://developers.arcgis.com/javascript/3/
[4]: https://developers.arcgis.com/javascript/3/jsapi/
[5]: http://zhihu.geoscene.cn/
[6]: https://developers.arcgis.com/javascript/latest/choose-version/
[7]: https://support.esri.com/en/Products/Developers/web-apis/arcgis-api-for-javascript/#product-support
[8]: https://developers.arcgis.com/javascript/latest/install-and-set-up/
[9]: https://developers.arcgis.com/javascript/latest/tooling-intro/





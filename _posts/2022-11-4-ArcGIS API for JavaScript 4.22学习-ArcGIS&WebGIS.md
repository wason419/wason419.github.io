---
layout: post
title: ArcGIS API for JavaScript 4.22学习 - ArcGIS & WebGIS
subtitle: 笔记
date: 2022-11-4
author: Wason
header-img: img/bg/post-bg-15.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习 - ArcGIS & WebGIS #

对于ArcGIS技术，我个人也是第一次接触，在学习的过程中，遇到了不少问题和疑惑，下面就跟大家分享下。    

**学习路线**  
我的学习路线是： 
> ① GIS基础文档资料 → ② [ArcGIS API for JavaScript 4.22官方英文API文档][1] → ③ 在Demo练习中不断巩固API应用和补充相关知识点。 

并且，由于我之前是从事Vue前端开发的，在看到官网文档中表示ArcGIS 4.18+支持使用ES模式引用库后，便直接决定采用 Vue + ArcGIS API for JavaScript 4.22 的方式进行学习和开发了 ( 而这也导致了在开发过程中，查找问题解答的不方便 )。  
不过，个人认为 对于ArcGIS技术的学习路线，可以分两种情况：  
1. 跟我一样完全是 GIS素人的话，可以先看完下面内容，在对ArcGIS有一定了解后，再参照上面的路线来开展学习，可以帮你减少不少疑惑。  
2. 若是对ArcGIS 已有一定了解或者 本身就是个 GISer 的话，可以在有一定的Web技术基础后，直接对ArcGIS API for JavaScript 4.22官方文档 进一步学习。  

`后面 ArcGIS API for JavaScript 4.22 将简称为 ArcGISJS`  

下面我将结合 Vue + ArcGISJS，分享开发中遇到问题及解决方式，和 对官方英文文档的相关API进行个人“翻译”说明。  

**ArcGIS是什么**  
ArcGIS API for JavaScript 官网文档为英文文档，在学习过程中，时常看到单词`Portal`，简单翻译为`门户`；但一开始自己不太理解其意思，觉得有点生硬，后来通过对 `Esri` 的拓展了解以及看了下面两篇文章，对ArcGIS有了整体的认识后才有些明白 Portal 的含义。    
1. [ArcGIS API for JavaScript开发入门必读][2]   
2. [WebGIS概念][3]  

---

个人总结下，首先要重点知道下 **Esri** 和 **ArcGIS** ，如下图     

![](/img/20210704/2021070405.png)  

Esri: 美国环境系统研究所公司  
目前为世界最大的地理信息系统技术供应商, 其地理信息系统软件目前的全球市场占有率最高，公司最知名产品如ArcGIS.  
我们一般企业业务的 地理信息方面的功能便是用到了 Esri 的技术产品。而我近期所学习的便是 ArcGIS 中的 ArcGIS API for JavaScript 技术。  

![](/img/20210704/2021070404.png)

维基描述的 ArcGIS 是由 ESRI 出品的一个地理信息系统系列软件的总称，指其是一系列软件总称。而参考上面“ArcGIS API for JavaScript开发入门必读”文章所述，“ArcGIS不是一个软件，而是一个平台，我们称之为'ArcGIS平台'”。  
对于GISer们，使用和接触到的软件更多的是ArcGIS Desktop、ArcGIS for Server、ArcGIS Pro这三个软件，这三个软件其实仅仅是ArcGIS软件体系中的其中三个而已。ArcGIS平台的整体结构如下图：  

![](/img/20210704/2021070402.png)   

可见 ArcGIS平台 分为上中下三部分，最上层是应用层，里面包括桌面端、移动端、PC端的一些应用软件，主要是做数据采集、处理、渲染显示的工作；最底层是服务器层，包括常用的ArcGIS Server，还有一些不常用的用于处理大数据的GA Server、处理实时数据的GE Server、处理影像的Image Server、用于科学计算的Notebook Server等，这些Server服务器支撑着整个ArcGIS平台的运行。而Data Store，则是负责平台中的数据存储。最上层和最底层是由Portal for ArcGIS连接，所以Portal其实在整个平台中是起着一个控制中枢的作用，最上层的应用如果要调用最底层server里面的数据服务，就必须要经过Portal，故可看成一个门户、通道。

**WebGIS是什么**  
我所学习的 ArcGIS API for JavaScript 主要是用来做 WebGIS 开发的，即在网页(浏览器)上进行GIS数据处理操作、可视化展示等。WebGIS的理解如下：  

![](/img/20210704/2021070403.png)   

通过上图，可以帮助自己对所学的GIS技术有更加清楚的认识。同时，要想学习ArcGISJS，也至少需要具备一定的Web前端技术基础。


对 ArcGIS & WebGIS 有了一定认识和了解后，接下来，我们就可以开展 ArcGISJS 相关的学习了。


[1]: https://developers.arcgis.com/javascript/latest/api-reference/
[2]: https://www.bilibili.com/read/cv8041647/
[3]: https://www.jianshu.com/p/7ac9aa68750c
---
layout: post
title: ArcGIS API for JavaScript 4.22学习-WebGIS
subtitle: 笔记
date: 2021-07-4
author: Wason
header-img: img/bg/post-bg-13.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习-WebGIS #
## 前言 ##  

对于ArcGIS技术，我个人也是第一次接触，在学习的过程中，遇到了不少问题和疑惑，下面就跟大家分享下。    
**学习路线**  
我的学习路线是： ① GIS基础文档资料 → ② [ArcGIS API for JavaScript 4.22官方英文API文档][1] → ③ 在Demo练习中不断巩固API应用和补充相关知识点。  
并且，由于我之前是从事Vue前端开发的，在看到官网文档中表示ArcGIS 4.18+支持使用ES模式引用库后，便直接决定采用 Vue + ArcGIS API for JavaScript 4.22 的方式进行学习和开发了；而这也导致了在开发过程中，查找问题解答的不方便。  
不过，个人认为 对于ArcGIS技术的学习路线，可以分两种情况：  
1.跟我一样是完全新人的话，可以先看完下面内容，在对ArcGIS有一定了解后，再参照上面路线的开展学习，可以帮你减少不少疑惑。
2.若是对ArcGIS 已有一定了解的话，可以直接对ArcGIS API for JavaScript 4.22官方文档 进一步学习。

`后面 ArcGIS API for JavaScript 4.22 将简称为 ArcGISJS。`  

下面我将结合 Vue + ArcGISJS，分享开发中遇到问题及解决方式，和 对官方英文文档的相关API进行个人“翻译”说明。  

![](/img/20210704/2021070401.png)  

**ArcGIS是什么**  
ArcGIS API for JavaScript 官网文档为英文文档，在学习过程中，时常看到单词`Portal`，简单翻译为`门户`；但一开始自己不太理解其意思，后来百度到下面两篇文章才对ArcGIS有整体的认识。   
1. [ArcGIS API for JavaScript开发入门必读][2]  
2. [WebGIS概念][3]  
---








### 参考文章 ###
1. GIS基础文档v1.2


[1]: https://developers.arcgis.com/javascript/latest/api-reference/
[2]: https://www.bilibili.com/read/cv8041647/
[3]: https://www.jianshu.com/p/7ac9aa68750c
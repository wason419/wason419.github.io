---
layout: post
title: HarmonyOS（六）
subtitle: 笔记
date: 2024-5-18
author: Wason
header-img: img/bg/post-bg-24.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（六） #  

首选项  
首选项的特点是：   
1. 以Key-Value形式存储数据。【Key是不重复的关键字，Value是数据值。】  
2. 非关系型数据库  
区别于关系型数据库，它不保证遵循ACID（Atomicity, Consistency, Isolation and Durability）特性，数据之间无关系。  
进程中每个文件仅存在一个Preferences实例，应用获取到实例后，可以从中读取数据，或者将数据存入实例中。通过调用flush方法可以将实例中的数据回写到文件里  
 
3. 实现形式：应用 — 实例 — 文件（存储）  

![](/img/20240518/2024051801.png)  

与关系数据库的区别  

![](/img/20240518/2024051802.png)     

常用接口有：保存数据（put）、获取数据（get）、是否包含指定的key（has）、删除数据（delete）、数据持久化（flush）等  

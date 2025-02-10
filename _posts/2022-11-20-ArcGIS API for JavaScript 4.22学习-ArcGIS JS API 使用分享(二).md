---
layout: post
title: ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API 使用分享(二)
subtitle: 笔记
date: 2022-11-20
author: Wason
header-img: img/bg/post-bg-19.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API 使用分享(二) #

承接上文，本文继续分享部分 API 的代码实现。  

2.面积/距离测量  
项目目录结构同上文所述，而地图上的 面积/距离测量功能，ArcGIS也有提供特定的API支持    

首先，在init.js文件中，引用相关文件 并初始化  

```
<!-- init.js -->
import Measurement from "@arcgis/core/widgets/Measurement";
......

function ArcGIS() {
  ......
  ArcGIS.prototype.init = function init($el) {
    console.log('===init===')
    try {
      ......
      // 初始化长度/面积测量工具
      this.measurement = new Measurement({
        view: this.myView,
      });
      this.myView.ui.add(this.measurement, {
        position: "bottom-right"
      });
      ......
    } catch (err) {
      console.log('err==>', err)
    }
  }
}

```

ArcGIS的公共index.js文件，定义ArcGIS的全局对象，代码如下：  

```
<!-- index.js -->
import { measurementClose, measurementOpen } from "./modules/measurement.js";
......

// 测量  
ArcGIS.prototype.measurementClose = measurementClose;
ArcGIS.prototype.measurementOpen = measurementOpen;

......

export default ArcGIS;
```

接着，在首页的vue文件中，引用初始化js  

```
<!-- index.vue -->
<template>
  <!-- 工具条组件 -->
    <tool-bar
      @measurement="measurement"
      ......
    >
    </tool-bar>
    ......
</template>

<script>
import ToolBar from "./components/toolBar"; // 工具条组件

// 引入 ArcGIS 模块，并进行实例化
import ArcGIS from "./map/index";
let Map = new ArcGIS();
......

export default {
  name: "mapTools",
  components: {
    ToolBar,
  },
  data() {
    return {
      ......
    };
  },
  mounted() {
    Map.init("mapContainer"); // 初始化地图模块
  },
  methods: {
    // 测量
    measurement(type) {
      ......
      Map.myView.popup.close(); // 关闭弹框
      switch (type) {
        case "0":
          Map.measurementClose();
          break;
        case "1":
          Map.measurementOpen("1");
          break;
        case "2":
          Map.measurementOpen("2");
          break;
      }
    },
    ......
  }
</script>

```

```
<!-- toolBar.vue 子组件代码 -->
<template>
  <div class="toolbar">
    <el-dropdown @command="measurement">
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="1"> 距离测量 </el-dropdown-item>
        <el-dropdown-item command="2"> 面积测量 </el-dropdown-item>
        <el-dropdown-item command="0"> 取消测量 </el-dropdown-item>
      </el-dropdown-menu>
      <el-button type="primary">
        <i class="el-icon-share"></i> 测量
        <i class="el-icon-arrow-down el-icon--right"></i>
      </el-button>
    </el-dropdown>

    ......

  </div>
</template>

<script>
export default {
  name: "toolBar",
  methods: {
    // 开启测量
    measurement(type) {
      this.$emit("measurement", type);
    },

    ......
  },
};
</script>

```

定义面积/距离测量的具体实现，创建逻辑操作文件measurement.js， 相关代码如下：   

```

// 关闭测量工具
const measurementClose = function measurementClose() {
  this.measurement.clear() // 清除测量图案
}

/**
 * 打开所选的测量工具
 * @param {*} type 测量类型 1距离 2面积
 */
function measurementOpen(type='1') {
  if (type === '1') {
    this.measurement.activeTool = 'distance';
  } else {
    this.measurement.activeTool = 'area';
  }
  this.myView.ui.add(this.measurement, "bottom-right");
}

export { measurementClose, measurementOpen }

```

> 学习并使用 ArcGIS API for JavaScript技术，需要做到对官方文档资料的熟悉，可事先根据项目使用的需求，从需要使用到技术入手学习，直到基本过一遍官方文档即可，后续再从技术实践中不断巩固。

> 项目代码：[ArcGIS-GIS学习代码](https://download.csdn.net/download/wenghaoduan/85219256)

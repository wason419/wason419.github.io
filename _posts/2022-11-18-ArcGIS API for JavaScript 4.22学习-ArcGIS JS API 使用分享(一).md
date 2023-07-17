---
layout: post
title: ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API 使用分享(一)
subtitle: 笔记
date: 2022-11-18
author: Wason
header-img: img/bg/post-bg-18.jpg
catalog: true
tags:
  - ArcGIS
---

# ArcGIS API for JavaScript 4.22学习 - ArcGIS JS API 使用分享(一) #
本节主要介绍下 ArcGIS API for JavaScript 的 API 列表内容，以及分享部分 API 的代码实现。  

一. ArcGIS API for JavaScript 的官网文档链接：[ArcGIS API for JavaScript/API Reference][1]  
二. 根据 ArcGIS API for JavaScript 提供的 API Reference 列表数据，对4.22版本所有 API 进行规整如下图：  

![](/img/20210718/2021071801.png)  


---  

三. 部分 API 代码实现   
下面将对部分 API 功能的代码实现进行分享  

1.图层切换
ArcGIS 的界面呈现方式，可以简单理解为是由Map管理数据，View负责视图容器，而视图中则是由不同图层叠加组合来显示的。因此，在实际应用会需要对地图视图进行切换，这时我们所做的操作便是对视图中的图层进行更换。图层分为底图和上层图层。我所实现的方式是更改底图。  

首先，创建初始化文件 init.js，在init.js中 引入arcgis相关Api，引用方式可查阅上一篇文章《构建 Vue+ArcGIS 项目》，并定义初始化方法 init(). 如下：  

```
<!-- init.js -->
import Map from "@arcgis/core/Map";
import MapView from "@arcgis/core/views/MapView";
import WebTileLayer from "@arcgis/core/layers/WebTileLayer";
import TileLayer from "@arcgis/core/layers/TileLayer";
import Basemap from "@arcgis/core/Basemap";
......

function ArcGIS() {
  this.map = null; // 地图
  this.baseMap = null; // 地图底图

  ArcGIS.prototype.init = function init($el) {
    console.log('===init===')
    try {
      this.TileLayer = TileLayer
      this.Basemap = Basemap;
      ......

      // FeatureLayer 图层
      this.gLayer = new this.FeatureLayer({
        id: "gLayer",
        url: "https://203.195.229.114:6443/arcgis/rest/services/1100001/house/MapServer",
        outFields: ["*"],
        spatialReference: {
          wkid: 4326
        },
      });

      // 用于加载测试天地图配置
      var _tileInfo = new TileInfo({
        dpi: 90.71428571427429,
        rows: 256,
        cols: 256,
        compressionQuality: 0,
        origin: {
          x: -180,
          y: 90
        },
        spatialReference: {
          wkid: 4326
        },
        lods: [
          { level: 1, levelValue: 1, resolution: 0.7031250000000002,scale: 2.9549759305875003E8 },
          { level: 2, levelValue: 2, resolution: 0.3515625, scale: 147748796.52937502 },
          { level: 3, levelValue: 3, resolution: 0.17578125, scale: 73874398.264687508 },
          { level: 4, levelValue: 4, resolution: 0.087890625, scale: 36937199.132343754 },
          { level: 5, levelValue: 5, resolution: 0.0439453125, scale: 18468599.566171877 },
          { level: 6, levelValue: 6, resolution: 0.02197265625, scale: 9234299.7830859385 },
          { level: 7, levelValue: 7, resolution: 0.010986328125, scale: 4617149.8915429693 },
          { level: 8, levelValue: 8, resolution: 0.0054931640625, scale: 2308574.9457714846 },
          { level: 9, levelValue: 9, resolution: 0.00274658203125, scale: 1154287.4728857423 },
          { level: 10, levelValue: 10, resolution: 0.001373291015625, scale: 577143.73644287116 },
          { level: 11, levelValue: 11, resolution: 0.0006866455078125, scale: 288571.86822143558 },
          { level: 12, levelValue: 12, resolution: 0.00034332275390625, scale: 144285.93411071779 },
          { level: 13, levelValue: 13, resolution: 0.000171661376953125, scale: 72142.967055358895 },
          { level: 14, levelValue: 14, resolution: 8.58306884765625e-005, scale: 36071.483527679447 },
          { level: 15, levelValue: 15, resolution: 4.291534423828125e-005, scale: 18035.741763839724 },
          { level: 16, levelValue: 16, resolution: 2.1457672119140625e-005, scale: 9017.8708819198619 },
          { level: 17, levelValue: 17, resolution: 1.0728836059570313e-005, scale: 4508.9354409599309 },
          { level: 18, levelValue: 18, resolution: 5.3644180297851563e-006, scale: 2254.4677204799655 },
          { level: 19, levelValue: 19, resolution: 2.68220901489257815e-006, scale: 1127.23386023998275 },
          { level: 20, levelValue: 2, resolution: 1.341104507446289075e-006, scale: 563.616930119991375 }
        ]
      })

      var baseMapAppid = 'xxxxxx';
      var baseMapUrl = 'xxxxxx';
      var baseMapUrlKey = 'xxxxxx';         //天地图geokey密钥

      // 不用使用x/y/z, 而是改用level/row/col
      let _type1 = 'img', _type2 = 'cia';
      let _level = '{level}';
      let _row = '{row}';
      let _col = '{col}';
      let _url1 = baseMapUrl + baseMapAppid + '/wmts' + "?service=wmts&request=gettile&version=1.0.0&layer=" + _type1 + "&style=default" +
        "&tilematrixset=c&format=tiles&tilecol=" + _col + "&tilerow=" + _row + "&tilematrix=" + _level + "&geokey=" + baseMapUrlKey

      let _url2 = baseMapUrl + baseMapAppid + '/wmts' + "?service=wmts&request=gettile&version=1.0.0&layer=" + _type2 + "&style=default" +
        "&tilematrixset=c&format=tiles&tilecol=" + _col + "&tilerow=" + _row + "&tilematrix=" + _level + "&geokey=" + baseMapUrlKey
      // 天地图图层数据0
      this._urlText = baseMapUrl + baseMapAppid + '/wmts' + "?service=wmts&request=gettile&version=1.0.0&layer=" + _type1 + "&style=default" +
      "&tilematrixset=c&format=tiles&tilecol=" + _col + "&tilerow=" + _row + "&tilematrix=" + _level + "&geokey=xxxx"

      // 天地图数据
      const tdt_Layer = {
        // 一般的天地图引用方式
        // vec_tiledLayer: new WebTileLayer({
        //   id: 'vec_tiledLayer',
        //   urlTemplate:
        //     "http://{subDomain}.tianditu.gov.cn/DataServer?T=vec_w&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002",
        //   subDomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
        // }), // 天地图切片底层
        // ras_tiledLayer: new WebTileLayer({
        //   id: 'ras_tiledLayer',
        //   urlTemplate:
        //     "http://{subDomain}.tianditu.gov.cn/DataServer?T=img_w&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002",
        //   subDomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
        // }), // 天地图影像底层
        // cia_tiledLayer: new WebTileLayer({
        //   id: 'cia_tiledLayer',
        //   urlTemplate:
        //     "http://{subDomain}.tianditu.gov.cn/DataServer?T=cia_w&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002",
        //   subDomains: ["t0", "t1", "t2", "t3", "t4", "t5", "t6", "t7"],
        // }), // 天地图影像注记


        // 统一更改坐标系和 类型为经纬度, 因为view 已经固定坐标系
        vec_tiledLayer: new WebTileLayer("http://{subDomain}.tianditu.gov.cn/DataServer?T=vec_c&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002", {
          tileInfo: _tileInfo,
          subDomains: ["t0"],
          spatialReference: {
            wkid: 4326
          },   
        }), // 天地图切片底层
        ras_tiledLayer: new WebTileLayer("http://{subDomain}.tianditu.gov.cn/DataServer?T=img_c&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002", {
          tileInfo: _tileInfo,
          subDomains: ["t0"],
          spatialReference: {
            wkid: 4326
          },   
        }), // 天地图影像底层
        cia_tiledLayer: new WebTileLayer("http://{subDomain}.tianditu.gov.cn/DataServer?T=cia_c&x={col}&y={row}&l={level}&tk=50b970b1fa5cffb38d6a75ee9c3a0002", {
          tileInfo: _tileInfo,
          subDomains: ["t0"],
          spatialReference: {
            wkid: 4326
          },    
        }), // 天地图影像注记
      

        // 指定天地图引用方式, 天地图4490，map图层需要统一设置为4326
        my_tiledLayer: new WebTileLayer(_url1, {
          tileInfo: _tileInfo,
          spatialReference: {
            wkid: 4326
          },
        }), // 指定天地图 影像图层
        my_w_tiledLayer: new WebTileLayer(_url2, {
          tileInfo: _tileInfo,
          spatialReference: {
            wkid: 4326
          },
        }), // 指定天地图 影像标注
      }

      // 设置地图地图图层，全局变量，后续有用
      this.baseMap = {
        vectorMap: tdt_Layer.vec_tiledLayer, // 矢量地图
        rasterMap: tdt_Layer.ras_tiledLayer, // 影像地图
        rasterMapAnnotation: tdt_Layer.cia_tiledLayer, // 影像注记
        myMap: tdt_Layer.my_tiledLayer, // 指定天地图
        myWMap: tdt_Layer.my_w_tiledLayer,
        type: '1', // 1 为矢量 | 2：影像 | 3： 指定
      };

      this.map = new Map({
        // zoom: 5, // 缩放级别
        // logo: false, // esri logo
        // maxZoom: 18, // 最大缩放级别
        // sliderPosition: "bottom-right", // 缩小放大按钮位置
        spatialReference : {
          wkid : 4326
        },
        // basemap: {
        //   baseLayers: [tdt_Layer.my_tiledLayer, tdt_Layer.my_w_tiledLayer]
        // }
      });
      this.map.addMany([tdt_Layer.vec_tiledLayer, tdt_Layer.cia_tiledLayer])  // 图层首次初始化加载

      // 挂载界面
      this.myView = new MapView({
        container: $el,
        map: this.map,
        zoom: 5,
        // center: [113.280637, 23.125178], // 广州
        center: [116.397128, 39.916527], // 北京
        // center: [116.369, 40.086],
        // center: [116.38784749099624, 78.1726697795944],
        spatialReference : {
          wkid : 4326
        },
      });

      ......
    } catch (err) {
      console.log('err==>', err)
    }
  }
}

```

另外，我还对ArcGIS的相关方法做了抽离，创建ArcGIS的index.js文件，定义ArcGIS的全局对象，代码如下： 

```
<!-- index.js -->
import ArcGIS from "./init.js";
import { baseMapChange } from "./modules/BaseMap";
import { addLayer, removeLayer } from "./modules/LayerControl.js";

...

// 图层切换
ArcGIS.prototype.baseMapChange = baseMapChange;

// 图层控制
ArcGIS.prototype.addLayer = addLayer;
ArcGIS.prototype.removeLayer = removeLayer;

......

export default ArcGIS;
```

接着，在首页的vue文件中，引用初始化js

```
<!-- index.vue -->
<template>
  <!-- 工具条组件 -->
    <tool-bar
      @baseMapChange="baseMapChange"
      ......
    >
    </tool-bar>
    <!-- 地图部分 -->
    <div class="main">
      <div id="mapContainer"></div>
      <div id="myTableNode"></div>
    </div>
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
    // 地图切换
    baseMapChange(type) {
      ......
      Map.baseMapChange(type);
    },
    ......
  }
</script>

<style>
/* 使用GIS小组件如弹框、距离/面积测量时，需要引用当前css才能显示 */
@import "https://js.arcgis.com/4.22/@arcgis/core/assets/esri/themes/dark/main.css";
</style>

<style>
.main {
  position: absolute;
  top: 70px;
  bottom: 0;
  width: 100%;

  #mapContainer {
    width: 100%;
    height: 100%;
  }
}
.content-area {
  padding: 10px;
  border: 1px solid #f56c6c;
  position: absolute;
  left: 60px;
  right: 7px;
  bottom: 25px;
  background: white;
  font-size: 22px;
  font-weight: 500;
}
</style>
```

```
<!-- toolBar.vue 子组件代码 -->
<template>
  <div class="toolbar">
    <el-dropdown @command="baseMapChange">
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item command="1"> 矢量(天地图) </el-dropdown-item>
        <el-dropdown-item command="2"> 影像(天地图) </el-dropdown-item>
        <el-dropdown-item command="3"> 加载FeatureLayer要素图层 </el-dropdown-item>
        <el-dropdown-item command="4"> 指定天地图 </el-dropdown-item>
      </el-dropdown-menu>
      <el-button type="primary">
        <i class="el-icon-coin"></i> 底图
        <i class="el-icon-arrow-down el-icon--right"></i>
      </el-button>
    </el-dropdown>
    <!-- 地图切换按钮 END-->

    ......

  </div>
</template>

<script>
export default {
  name: "toolBar",
  methods: {
    // 底图切换
    baseMapChange(type) {
      this.$emit("baseMapChange", type);
    },

    ......
  },
};
</script>

<style lang="scss" scoped>
.toolbar {
  position: absolute;
  top: 80px;
  right: 40px;
  height: 40px;
  width: auto;
  z-index: 99;
}
</style>

<style>
.el-button + .el-button {
  margin-left: 0;
  border-left: 1px solid #ededed;
}
.el-dropdown > .el-button {
  border-right: 1px solid #ededed;
}
.el-button {
  border-radius: 0;
}
</style>
```

其中，还需定义图层切换的具体实现，创建逻辑操作文件BaseMap.js， 相关代码如下：  
```
<!-- BaseMap.js -->
import Vue from "vue";

const name = "baseMapChange";
const _that = Vue.config

const baseMapChange = function baseMapChange(type) {
  if (type === this.baseMap.type) return; // 防止重复加载
  // 添加 矢量
  if (type === '1') {
    console.log('===添加矢量===')
    this.removeLayer();
    this.addLayer(
      [this.baseMap.vectorMap, this.baseMap.rasterMapAnnotation],
      [0, 1]
    );
    this.baseMap.type = '1';
    this.drawInit();
    ......
  }
  // 添加 影像
  else if (type === '2') {
    console.log('===添加影像===')
    this.removeLayer();
    this.addLayer(
      [this.baseMap.rasterMap, this.baseMap.rasterMapAnnotation],
      [0, 1]
    );
    this.baseMap.type = '2';
    this.drawInit();
    ......
  }
  // 添加 指定天地图
  else if (type === '4') {
    console.log('===添加指定天地图===')
    this.removeLayer();
    this.map.addMany([this.baseMap.myMap, this.baseMap.myWMap])
    this.baseMap.type = '4';
    this.drawInit();
    ......
  }
  // 添加 arcgis 服务端
  else {
    console.log('===添加arcgis server FeatureLayer 图层===')
    this.map.add(this.gLayer);
    this.baseMap.type = '3';
    this.drawInit();

    let query = this.gLayer.createQuery();
    query.spatialRelationship = "intersects";  // this is the default
    query.returnGeometry = true;
    query.outFields = ["*"];

    this.gLayer.queryFeatures(query)
      .then(response=>{
        // 查询featureLayer图层 所有数据
        let _arr = response.features.map(item => {
          return item.attributes
        })
        ......
      })
      .catch(err=>{
        console.log(err)
      }) ;
  }
};

export { name, baseMapChange };
```

相关工具类方法:  
```
<!-- LayerControl.js -->
// 图层约定
// 1 2 层为底图层
// 3 层为边界图层

import { DataType } from "@/utils/index"; // 工具函数

/*
 *  description:  添加图层
 *  param {Layer,Array<Layer>} layer  需添加的图层
 *  param {number,Array<number>} lever 添加图层的层数
 */
const addLayer = function addLayer(layer, lever) {
  // 判断是
  if (DataType(layer, "array")) {
    layer.forEach((item, index) => {
      lever ? this.map.add(item, lever[index]) : this.map.add(item);
    });
  } else {
    lever ? this.map.add(layer, lever) : this.map.add(layer);
  }
};

const removeLayer = function removeLayer() {
  this.map.removeAll()
};

export { addLayer, removeLayer };
```

```
<!-- @/utils/index.js -->

//DataType("young"); // "string"
//DataType(20190214); // "number"
//DataType(true); // "boolean"
//DataType([], "array"); // true
//DataType({}, "array"); // false
export function DataType(tgt, type) {
  const dataType = Object.prototype.toString
    .call(tgt)
    .replace(/\[object (\w+)\]/, "$1")
    .toLowerCase();
  return type ? dataType === type : dataType;
}

......
```

### 要点规整 ###
1. 各个图层的 坐标系/经纬度数值 须统一与 View 的定义一致： spatialReference { wkid : 4326 }
2. 天地图的使用有些坑须注意
  ① 一般天地图的引用，可直接在 天地图 官网注册后，依据官网提示进行应用
  ② 天地图一般分为 切片和影像类型，其图层组成为 底图 + 标注图层, 加载天地图需两层组合
  ③ 如上面代码所示，一般天地图的引用方式如上，若是指定的天地图类型，则需采用 _tileInfo 类型的方式进行定义；网路上关于天地图引用方式的描述采用一般的较多，可能是ArcGIS 的新旧版本的区别，故若天地图引用有误，可尝试采用 _tileInfo 方式。
```
this.removeLayer();
this.map.addMany([this.baseMap.myMap, this.baseMap.myWMap])
```
3. 上面代码 表示的是 先清空图层，再加载对应新图层。

> 项目代码：[ArcGIS-GIS学习代码](https://download.csdn.net/download/wenghaoduan/85219256)





[1]: https://developers.arcgis.com/javascript/latest/api-reference/
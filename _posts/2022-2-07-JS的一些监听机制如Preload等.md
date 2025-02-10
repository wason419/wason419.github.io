---
layout: post
title: JS的一些监听机制如Preload等
subtitle: 笔记
date: 2022-02-07
author: Wason
header-img: img/bg/post-bg-21.jpg
catalog: true
tags:
  - JS
  - Preload
  - 预加载
---

# JS的一些监听机制如Preload等 #

## 前言 ##  
首先，由预加载问题引申，可分为 如下2点分析：  
[一、预加载、懒加载是什么，及其作用是什么？](#anc1)    
[二、预加载、懒加载的使用？](#anc2)    

## 详述 ##  
<a id='anc1'></a>  

### `一、预加载、懒加载是什么，及其作用是什么？` ###   
  1. 预加载是指提前加载Web应用程序资源，以便在需要时可以快速访问这些资源，提高用户的体验；   
  2. 懒加载就是当用户需要访问某个组件时才会把该组件的代码加载进来（按需加载），而不是一开始就把所有组件的代码都加载进来，这样可以减少初始加载的时间，提高页面的响应速度。   

<a id='anc2'></a>  

### `二、预加载、懒加载的使用？` ###  

可分为两个情况：JS 和 Vue  
1. 一般的JS框架下，预加载方面，可借助 preload.js 工具库实现，见下【content_1】；  
2. Vue框架下，可通过 Router 或 webpack 配置实现，见下【content_2】；  
3. 补充 vue框架下 使用Vue 插件来实现图片预加载，见下【content_3】;  



**[content_1]**

一、preload.js 简介
1. preload.js 是一个用于预加载资源的JS库。它是由 Grant Skinner 开发的，旨在为Flash开发人员提供工具来管理预加载。但是，它也可以用于HTML5游戏和Web应用程序。preload.js 可以同时处理多个文件，提供回调和事件监听器，可用于跟踪和报告加载进度，并处理加载错误。

2. 安装：npm install preload-js     
或者 直接引用 <script src="path/to/preloadjs.min.js"></script>

3. 使用举例：

```
------------------------start------------------------
创建一个 Preload 实例，并配置要加载的文件
var queue = new createjs.LoadQueue(false);

queue.on("fileload", handleFileLoad);
queue.on("complete", handleComplete, this); 

// 配置要加载的文件
// 为每个文件指定一个唯一的 ID，以便在文件加载完成后进行引用
queue.loadManifest([    
    {id: "image", src:"image.png"},    
    {id: "sound", src:"sound.mp3"},    
    {id: "data", src:"data.json"}
]);

function handleFileLoad(event) {    
    console.log("文件已加载", event.item.id);
}
function handleComplete() {    
    console.log("所有文件已加载完成");
}
------------------------end------------------------
```


4. 监听事件     
preload.js 提供了几个事件，监视资源加载的状态：  
* fileload：每次成功加载完一个文件后触发。  
* progress：每次有文件加载时都会触发。可以使用 event.loaded 属性获取当前加载进度。  
* complete：全部文件加载完毕之后触发。  
* error：加载失败时触发。  

```js
------------------------start------------------------
var queue = new createjs.LoadQueue(false);
queue.on("fileload", handleFileLoad);
queue.on("progress", handleProgress, this);
queue.on("complete", handleComplete, this);
queue.on("error", handleError);
// 配置要加载的文件
queue.loadManifest([     
    // ...
]);
------------------------end------------------------
```

preload.js 也可以单独加载各种类型的资源，例如图片、音频、视频和JSON数据等等。  
[e.g. 加载图片]   
queue.loadFile({id:"logo", src:"logo.png"});   


**[content_2]**  
日常在vue框架下 进行懒加载、预加载处理的情况会比较多些。下面分别描述：  

一、预加载  
1. 普通预加载处理  

```
------------------------start------------------------
<template>  
  <div v-if="showContent" class="content">{{ exContent }}</div>   
</template>  
<script>
import { getContent } from './api.js'  
export default {  
  data() {  
    return {  
      exContent: '',
      showContent: false
    }
  },
  async mounted() {
    // 显示加载指示器
    showLoadingIndicator()
    // 预加载内容
    await this.preloadContent()
    // 隐藏加载指示器
    hideLoadingIndicator()
    // 显示内容
    this.showContent = true
  },
  methods: {
    async preloadContent() {
      this.exContent = await getContent()
    }
  }
}
</script>

借助 mounted 生命周期，模块加载时，先进行loading显示处理，待内部相关数据加载完毕后，再关闭loading，显示页面。   

------------------------end------------------------  
```

2. Vue Router处理实现    

[e.g. 1]   
 
```js
------------------------start------------------------
const router = new VueRouter({
  routes: [
    {
      path: '/profile',
      component: Profile,
      meta: {
        preload: async () => {
          // 组件预加载
          const profileData = await getProfileData()
          store.dispatch('setProfileData', profileData)
        }
      }
    }
  ]
})

通过在路由的meta属性中，配置preload ，可实现 进入路由组件之前 立即调用 preload 所配置的函数。
------------------------end------------------------    

```

[e.g. 2]   

```js
------------------------start------------------------  
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: () => import('./Foo.vue'),
      // 预加载
      meta: { preload: true }    // 可配置状态值 或 直接配置预加载函数
    },
    {
      path: '/bar',
      component: () => import('./Bar.vue'),
      // 懒加载
      meta: { lazyload: true }    // 只是用于后期利用 meta 中的状态值进行判断处理
    }
  ],

  // 手动处理预加载
  scrollBehavior ( to, from, savedPosition ) {
    if (savedPosition && to.meta.preload) {
      return savedPosition;
    } else {
      return { x: 0, y: 0 };
    }
  }
});
------------------------end------------------------   

```

通过在路由meta 属性中配置状态值，在路由守卫或 Vue Router提供的滚动行为监听中   
进行【scrollBehavior】预加载判断并执行相关函数。  

scrollBehavior官方文档：  
https://router.vuejs.org/zh/guide/advanced/scroll-behavior.html   
见下【图1】  


3. 利用webpack处理实现   
webpack提供了两个关键字来实现预取（prefetch）和预加载（preload）   

- 预取是指浏览器在空闲时加载资源，   
- 预加载则是在当前页面加载完毕后立即加载下一个页面需要的资源。  

例如：  

```js
------------------------start------------------------  
const Foo = () => import(/* webpackPrefetch: true */ './Foo.vue')
const Bar = () => import(/* webpackPreload: true */ './Bar.vue')
------------------------end------------------------  
```

二、懒加载
1. 使用ES 的 import()动态导入组件（Vue 2.4.0以上版本支持使用import()方法来动态导入组件）

```
------------------------start------------------------  
e.g. 我们可以定义一个异步组件，这个组件在需要的时候才会被加载进来：  
Vue.component('my-component', () => import('./MyComponent.vue'));  
------------------------end------------------------  
```

2. vue异步组件技术——异步加载
vue-router配置路由，使用vue的异步组件技术，可以实现按需加载。此时，一个组件将生成一个js文件。

```
------------------------start------------------------
/* vue异步组件技术 */   
{ path: '/home', name: 'home', component: resolve => require(['@/components/home'],resolve) }   
------------------------end------------------------
```

**[content_3]**   

```vue
------------------------start------------------------   
// main.js
import Vue from 'vue'
import App from './App.vue'
import vuePicturePreload from 'vue-picture-preload'

Vue.use(vuePicturePreload)

new Vue({
  render: h => h(App)
}).$mount('#app')

在需用加载图片的组件下 
<template>
  <div>
    <img v-preload="src" />
  </div>
</template>

<script>
export default {
  data () {
    return {
      src: '/path/to/image.jpg'
    }
  }
}
</script>  
------------------------end------------------------   
```


![图1](/img/20220205/2022020501.png "图1")  
(图1)  

    
### 参考文章 ###

[了解preload.js：优化Web应用程序的资源加载][1]   
[Vue如何实现组件的懒加载和预加载？][2]  
[Vue路由懒加载][3]  
[vue中的preload][4]  

[1]: https://www.861ppt.com/news/18110.html
[2]: https://www.php.cn/faq/568099.html  
[3]: https://blog.csdn.net/qq_44034384/article/details/127601448  
[4]: https://www.yzktw.com.cn/post/1259950.html

---
layout: post
title: HarmonyOS（四）
subtitle: 笔记
date: 2024-5-10
author: Wason
header-img: img/bg/post-bg-22.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（四） #  

UIAbility【 应用程序框架入口】   
UIAbility是一种包含用户界面的应用组件，主要用于和用户进行交互。UIAbility也是系统调度的单元，为应用提供窗口在其中绘制界面。  
一个应用可以有一个UIAbility，也可以有多个UIAbility【一个UIAbility 便是一个页面模块内容；多个UIAbility相对独立，在任务列表里中可看到多个页面内容，但却是同一个应用的】  
一个UIAbility可以对应于多个页面，建议将一个独立的功能模块放到一个UIAbility中，以多页面的形式呈现。例如新闻应用在浏览内容的时候，可以进行多页面的跳转使用。【即 独立功能抽离到一个UIAbility中，然后点击跳转多个页面显示不同内容】  
   
UIAbility的生命周期  
```
文档：
UIAbility的生命周期包括 Create、Foreground、Background、Destroy四个状态，WindowStageCreate和WindowStageDestroy为窗口管理器（WindowStage）在UIAbility中管理UI界面功能的两个生命周期回调，从而实现UIAbility与窗口之间的弱耦合。
```

**即 UIAbility的生命周期状态 有四个：  Create、Foreground、Background、Destroy；！！！**   
WindowStageCreate、WindowStageDestroy 仅仅只是两个跟窗口管理器（WindowStage）相关的生命周期回调。  

![](/img/20240510/2024051001.png)    


UIAbility的启动模式   
UIAbility的启动模式有3种： singleton（单实例模式）、multiton（多实例模式）和specified（指定实例模式）   
```
singleton（单实例模式）
singleton启动模式为单实例模式，也是默认情况下的启动模式。

multiton（多实例模式）
multiton启动模式为多实例模式，每次调用startAbility()方法时，都会在应用进程中创建一个新的该类型UIAbility实例。即在最近任务列表中可以看到有多个该类型的UIAbility实例。这种情况下可以将UIAbility配置为multiton（多实例模式）。

specified（指定实例模式）
specified启动模式为指定实例模式，针对一些特殊场景使用（例如文档应用中每次新建文档希望都能新建一个文档实例，重复打开一个已保存的文档希望打开的都是同一个文档实例）。
```

修改 UIAbility的启动模式： 修改launchType属性   
```
{
   "module": {
     // ...
     "abilities": [
       {
         "launchType": "singleton",
         // ...
       }
     ]
  }
}
```

页面跳转与参数接收   
在使用页面路由之前，需要先导入router模块   
```
import router from '@ohos.router';
```

页面跳转主要有2个方式  
1. router.pushUrl()  
2. router.replaceUrl()  
```
/** pushUrl **/
方式一：API9及以上，router.pushUrl()方法新增了mode参数，可以将mode参数配置为router.RouterMode.Single单实例模式和router.RouterMode.Standard多实例模式。
在单实例模式下：如果目标页面的url在页面栈中已经存在同url页面，离栈顶最近同url页面会被移动到栈顶，移动后的页面为新建页，原来的页面仍然存在栈中，页面栈的元素数量不变；如果目标页面的url在页面栈中不存在同url页面，按多实例模式跳转，页面栈的元素数量会加1。
注意：当页面栈的元素数量较大或者超过32时，可以通过调用router.clear()方法清除页面栈中的所有历史页面，仅保留当前页面作为栈顶页面。
router.pushUrl({
  url: 'pages/Second',
  params: {
    src: 'Index页面传来的数据',
  }
}, router.RouterMode.Single)


/** replaceUrl**/
方式二：API9及以上，router.replaceUrl()方法新增了mode参数，可以将mode参数配置为router.RouterMode.Single单实例模式和router.RouterMode.Standard多实例模式。
在单实例模式下：如果目标页面的url在页面栈中已经存在同url页面，离栈顶最近同url页面会被移动到栈顶，替换当前页面，并销毁被替换的当前页面，移动后的页面为新建页，页面栈的元素数量会减1；如果目标页面的url在页面栈中不存在同url页面，按多实例模式跳转，页面栈的元素数量不变。
router.replaceUrl({
  url: 'pages/Second',
  params: {
    src: 'Index页面传来的数据',
  }
}, router.RouterMode.Single)
```

其中，最后一个参数为  API9及以上， router新增的mode参数， 可以将mode参数配置为：router.RouterMode.Single（单实例） 和 router.RouterMode.Standard（多实例）  

![](/img/20240510/2024051002.png)    
注意：
1. RouterMode 默认采取的是 Standard （多实例模式）；  
2. 页面栈的元素数量的上限参考值为 32；3. 采用不同的跳转方式时，页面栈的元素数量有可能加1 ，也可能不变，甚至是减1。   


页面接收 是使用 router.getParams()方法获取   
```
import router from '@ohos.router';

@Entry
@Component
struct Second {
  @State src: string = (router.getParams() as Record<string, string>)['src'];
  // 页面刷新展示
  // ...
}
```

页面返回   
可以通过调用router.back()方法实现返回到上一个页面，或者在调用router.back()方法时增加可选的options参数（增加url参数）返回到指定页面。  
```
说明
调用router.back()返回的目标页面需要在页面栈中存在才能正常跳转。 【router.back({ url: 'pages/Index' });】
例如调用router.pushUrl()方法跳转到Second页面，在Second页面可以通过调用router.back()方法返回到上一个页面。
例如调用router.clear()方法清空了页面栈中所有历史页面，仅保留当前页面，此时则无法通过调用router.back()方法返回到上一个页面。
```


页面返回 还可以提供 一个询问对话框。即在调用router.back()方法之前，可以先调用router.enableBackPageAlert()方法开启页面返回询问对话框功能。   
```
说明
router.enableBackPageAlert()方法开启页面返回询问对话框功能，只针对当前页面生效。
例如在调用router.pushUrl()或者router.replaceUrl()方法，跳转后的页面均为新建页面，因此在页面返回之前均需要先调用router.enableBackPageAlert()方法之后，页面返回询问对话框功能才会生效。

如需关闭页面返回询问对话框功能，可以通过调用router.disableAlertBeforeBackPage()方法关闭该功能即可。

router.enableBackPageAlert({
  message: 'Message Info'
});

router.back();
或者
router.back({
  url: 'pages/Index',
  params: {
    src: 'Second页面传来的数据',
  }
})
```



页面返回触发的 页面参数接收   
```
说明
调用router.back()方法，不会新建页面，返回的是原来的页面，在原来页面中@State声明的变量不会重复声明，以及也不会触发页面的aboutToAppear()生命周期回调，因此无法直接在变量声明以及页面的aboutToAppear()生命周期回调中接收和解析router.back()传递过来的自定义参数。

import router from '@ohos.router';
class routerParams {
  src:string
  constructor(str:string) {
    this.src = str
  }
}
@Entry
@Component
struct Index {
  @State src: string = '';
  onPageShow() {
    this.src = (router.getParams() as routerParams).src
  }
  // 页面刷新展示
  // ...
}
```


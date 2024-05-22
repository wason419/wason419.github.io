---
layout: post
title: HarmonyOS（五）
subtitle: 笔记
date: 2024-5-15
author: Wason
header-img: img/bg/post-bg-23.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（五） #  
![](/img/20240515/2024051501.png)    
上图中，Components部分的装饰器为组件级别的状态管理，Application部分为应用的状态管理。  
开发者可以通过@StorageLink/@LocalStorageLink实现应用和组件状态的双向同步，通过@StorageProp/@LocalStorageProp实现应用和组件状态的单向同步。   

装饰器总览  
ArkUI提供了多种装饰器，通过使用这些装饰器，状态变量不仅可以观察在组件内的改变，还可以在不同组件层级间传递，比如父子组件、跨组件层级，也可以观察全局范围内的变化。  
根据状态变量的影响范围，将所有的装饰器可以大致分为：  
1. 管理组件拥有状态的装饰器：组件级别的状态管理，可以观察组件内变化，和不同组件层级的变化，但需要唯一观察同一个组件树上，即同一个页面内。  
2. 管理应用拥有状态的装饰器：应用级别的状态管理，可以观察不同页面，甚至不同UIAbility的状态变化，是应用内全局的状态管理。  

从数据的传递形式和同步类型层面看，装饰器也可分为：  
1. 只读的单向传递；  
2. 可变更的双向传递。  


管理组件拥有的状态，即Components级别的状态管理：  
@State：@State装饰的变量拥有其所属组件的状态，可以作为其子组件单向和双向同步的数据源。当其数值改变时，会引起相关组件的渲染刷新。  
@Prop：@Prop装饰的变量可以和父组件建立单向同步关系，@Prop装饰的变量是可变的，但修改不会同步回父组件。  
@Link： @Link装饰的变量和父组件构建双向同步关系的状态变量，父组件会接受来自@Link装饰的变量的修改的同步，父组件的更新也会同步给@Link装饰的变量。  
@Provide/@Consume：@Provide/@Consume装饰的变量用于跨组件层级（多层组件）同步状态变量，可以不需要通过参数命名机制传递，通过alias（别名）或者属性名绑定。  
@Observed：@Observed装饰class，需要观察多层嵌套场景的class需要被@Observed装饰。单独使用@Observed没有任何作用，需要和@ObjectLink、@Prop连用。  
@ObjectLink：@ObjectLink装饰的变量接收@Observed装饰的class的实例，应用于观察多层嵌套场景，和父组件的数据源构建双向同步。  
```
说明
仅@Observed/@ObjectLink可以观察嵌套场景，其他的状态变量仅能观察第一层！！！
```

不同装饰器使用注意点：  
@State :  ① 被装饰变量的初始值：必须本地初始化；② 是否支持组件外访问： 不支持，只能在组件内访问。   
@Prop： ① 被装饰变量的初始值：允许本地初始化。② 是否支持组件外访问： 私有，只能在组件内访问。   
@Link：  ① 被装饰变量的初始值： 无，禁止本地初始化。② 是否支持组件外访问： 私有，只能在所属组件内访问。  

@Provide：① 被装饰变量的初始值：必须指定。 ② 是否支持组件外访问：私有，仅可以在所属组件内访问。   
@Consume：① 被装饰变量的初始值：无，禁止本地初始化。② 是否支持组件外访问：私有，仅可以在所属组件内访问。   
@ObjectLink：被装饰变量的初始值：不允许。  

```
@Provide/@Consume装饰的状态变量有以下特性：

@Provide装饰的状态变量自动对其所有后代组件可用，即该变量被“provide”给他的后代组件。由此可见，@Provide的方便之处在于，开发者不需要多次在组件之间传递变量。
后代通过使用@Consume去获取@Provide提供的变量，建立在@Provide和@Consume之间的双向数据同步，与@State/@Link不同的是，前者可以在多层级的父子组件之间传递。
@Provide和@Consume可以通过相同的变量名或者相同的变量别名绑定，变量类型必须相同。

// 通过相同的变量名绑定
@Provide a: number = 0;
@Consume a: number;
// 通过相同的变量别名绑定
@Provide('a') b: number = 0;
@Consume('a') c: number;

@Provide和@Consume通过相同的变量名或者相同的变量别名绑定时，@Provide修饰的变量和@Consume修饰的变量是一对多的关系。不允许在同一个自定义组件内，包括其子组件中声明多个同名或者同别名的@Provide装饰的变量。
```

```
限制条件：

@Prop装饰器不能在@Entry装饰的自定义组件中使用。
@Link装饰器不能在@Entry装饰的自定义组件中使用。

使用@Observed装饰class会改变class原始的原型链，@Observed和其他类装饰器装饰同一个class可能会带来问题。
@ObjectLink装饰器不能在@Entry装饰的自定义组件中使用。
```

管理应用拥有的状态，即Application级别的状态管理：   
AppStorage是应用程序中的一个特殊的单例LocalStorage对象，是应用级的数据库，和进程绑定，通过@StorageProp和@StorageLink装饰器可以和组件联动。    
AppStorage是应用状态的“中枢”，将需要与组件（UI）交互的数据存入AppStorage，比如持久化数据PersistentStorage和环境变量Environment。UI再通过AppStorage提供的装饰器或者API接口，访问这些数据。    

框架还提供了LocalStorage，AppStorage是LocalStorage特殊的单例。LocalStorage是应用程序声明的应用状态的内存“数据库”，通常用于页面级的状态共享，通过@LocalStorageProp和@LocalStorageLink装饰器可以和UI联动。   
```
LocalStorage：页面级UI状态存储，通常用于UIAbility内、页面间的状态共享。
AppStorage：特殊的单例LocalStorage对象，由UI框架在应用程序启动时创建，为应用程序UI状态属性提供中央存储；
PersistentStorage：持久化存储UI状态，通常和AppStorage配合使用，选择AppStorage存储的数据写入磁盘，以确保这些属性在应用程序重新启动时的值与应用程序关闭时的值相同；
Environment：应用程序运行的设备的环境参数，环境参数会同步到AppStorage中，可以和AppStorage搭配使用。
```



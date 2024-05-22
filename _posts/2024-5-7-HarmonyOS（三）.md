---
layout: post
title: HarmonyOS（三）
subtitle: 笔记
date: 2024-5-7
author: Wason
header-img: img/bg/post-bg-22.jpg
catalog: true
tags:
  - HarmonyOS
---

# HarmonyOS（三） #  

一、自定义组件的基本结构（涉及的关键字、装饰器）  
struct：自定义组件基于struct实现，struct + 自定义组件名 + {...}的组合构成自定义组件，不能有继承关系。（自定义组件名、类名、函数名不能和系统组件名相同。）   
build()函数：build()函数用于定义自定义组件的声明式UI描述，自定义组件必须定义build()函数。   
@Component：@Component装饰器 仅能装饰 struct关键字 声明的数据结构。struct被@Component装饰后 具备组件化的能力，需要实现build方法来描述UI，一个struct只能被一个@Component装饰。  
@Entry：@Entry装饰的自定义组件将作为UI页面的入口。在单个UI页面中，最多可以使用@Entry装饰一个自定义组件。@Entry可以接受一个可选的 LocalStorage 的参数。   
```
自定义父子组件的传参方式：
@Component
struct MyComponent {
  private countDownFrom: number = 0;
  private color: Color = Color.Blue;

  build() {
  }
}

@Entry
@Component
struct ParentComponent {
  private someColor: Color = Color.Pink;

  build() {
    Column() {
      // 创建MyComponent实例，并将创建MyComponent成员变量countDownFrom初始化为10，将成员变量color初始化为this.someColor
      MyComponent({ countDownFrom: 10, color: this.someColor })
    }
  }
}
```

build()函数定义 注意要点：   
@Entry装饰的自定义组件，其build()函数下的根节点唯一且必要，且必须为容器组件，其中ForEach禁止作为根节点。   
@Component装饰的自定义组件，其build()函数下的根节点唯一且必要，可以为非容器组件，其中ForEach禁止作为根节点。   
```
@Entry
@Component
struct MyComponent {
  build() {
    // 根节点唯一且必要，必须为容器组件
    Row() {
      ChildComponent()
    }
  }
}

@Component
struct ChildComponent {
  build() {
    // 根节点唯一且必要，可为非容器组件
    Image('test.jpg')
  }
}
```

不允许在UI描述里直接使用console.info，但允许在方法或者函数里使用   

```
build() {
  // 反例：不允许console.info
  console.info('print debug log');
}
```

不允许调用没有用@Builder装饰的方法，允许系统组件的参数是TS方法的返回值（即非@Builder 装饰函数的返回值 可以用作参数使用）。  
```
@Component
struct ParentComponent {
  doSomeCalculations() {
  }

  calcTextValue(): string {
    return 'Hello World';
  }

  @Builder doSomeRender() {
    Text(`Hello World`)
  }

  build() {
    Column() {
      // 反例：不能调用没有用@Builder装饰的方法
      this.doSomeCalculations();

      // 正例：可以调用
      this.doSomeRender();

      // 正例：参数可以为调用TS方法的返回值
      Text(this.calcTextValue())
    }
  }
}
```

不允许switch语法，如果需要使用条件判断，请使用if。   
不允许使用表达式   

```
build() {
  Column() {
    // 反例：不允许使用switch语法
    switch (expression) {
      case 1:
        Text('...')
        break;
      case 2:
        Image('...')
        break;
      default:
        Text('...')
        break;
    }
  }
}

build() {
  Column() {
    // 反例：不允许使用表达式
    (this.aVar > 10) ? Text('...') : Image('...')
  }
}


/** if 方式用法 */
@Entry
@Component
struct MyComponent {

  build() {
    Column() {
      if (this.showChild) {
        Child()
      }

      Button('delete Child').onClick(() => {
        this.showChild = false;
      })
    }
  }
}

@Component
struct Child {
    build() {}
}
```

二、页面级别 与 自定义组件的 生命周期   
```
自定义组件和页面的关系：
自定义组件：@Component装饰的UI单元，可以组合多个系统组件实现UI的复用，可以调用组件的生命周期。
页面：即应用的UI页面。可以由一个或者多个自定义组件组成，@Entry装饰的自定义组件为页面的入口组件，即页面的根节点，一个页面有且仅能有一个@Entry。只有被@Entry装饰的组件才可以调用页面的生命周期。
```

1. 页面（由@Entry装饰）：  
onPageShow：页面每次显示时触发一次，包括路由过程、应用进入前台等场景。  
onPageHide：页面每次隐藏时触发一次，包括路由过程、应用进入后台等场景。  
onBackPress：当用户点击返回按钮时触发。  

2. 自定义组件（由@Component装饰）：     
aboutToAppear：组件即将出现时回调该接口，具体时机为在创建自定义组件的新实例后，在执行其build()函数之前执行。   
aboutToDisappear：在自定义组件析构销毁之前执行。不允许在aboutToDisappear函数中改变状态变量，特别是@Link变量的修改可能会导致应用程序行为不稳定。   


生命周期如图所示：   
![](/img/20240507/2024050701.png)   

自定义组件的创建和渲染流程   
① 在首次渲染的时候，执行build方法渲染系统组件，如果子组件为自定义组件，则创建自定义组件的实例。在首次渲染的过程中，框架会记录状态变量和组件的映射关系，当状态变量改变时，驱动其相关的组件刷新。   
② 当应用在后台启动时，此时应用进程并没有销毁，所以仅需要执行onPageShow。   


自定义组件的删除   
1. 如果if组件的分支改变，或者ForEach循环渲染中数组的个数改变，组件将被删除：  
    在删除组件之前，将调用其aboutToDisappear生命周期函数，标记着该节点将要被销毁。  
    ArkUI的节点删除机制是：后端节点直接从组件树上摘下，后端节点被销毁，对前端节点解引用，前端节点已经没有引用时，将被JS虚拟机垃圾回收。  
    自定义组件和它的变量将被删除，如果其有同步的变量，比如@Link、@Prop、@StorageLink，将从同步源上取消注册。  
2. 不建议在生命周期aboutToDisappear内使用async await，如果在生命周期的aboutToDisappear使用异步操作（Promise或者回调方法），自定义组件将被保留在Promise的闭包中，直到回调方法被执行完，这个行为阻止了自定义组件的垃圾回收。  

自定义父子组件 不同操作下 生命周期调用：  
1. 应用冷启动的初始化流程为：父组件 aboutToAppear --> 父组件 build --> 子组件 aboutToAppear --> 子组件 build --> 子组件 build执行完毕 --> 父组件 build执行完毕 --> Index onPageShow。  
2. 删除 子组件，会执行 子组件 aboutToDisappear方法。  
3. 页面跳转：   
① 调用router.pushUrl接口，跳转到另外一个页面，当前Index页面隐藏，并没有销毁，执行页面生命周期Index onPageHide。跳转到新页面后，执行初始化新页面的生命周期的流程。   
② 调用router.replaceUrl，则当前Index页面被销毁，执行的生命周期流程将变为：    Index onPageHide --> 父组件 aboutToDisappear --> 子组件 aboutToDisappear。   
    【组件的销毁是从组件树上直接摘下子树，故先调用父组件的aboutToDisappear，再调用子组件的aboutToDisappear，然后执行初始化新页面的生命周期流程。】   
4. 点击返回按钮，触发页面生命周期Index onBackPress，且触发返回一个页面后会导致当前Index页面被销毁。   
5. 最小化应用或者应用进入后台，触发Index onPageHide。当前Index页面没有被销毁，所以并不会执行组件的aboutToDisappear。应用回到前台，执行Index onPageShow。   
6. 退出应用，执行Index onPageHide --> 父组件 aboutToDisappear --> 子组件 aboutToDisappear。   






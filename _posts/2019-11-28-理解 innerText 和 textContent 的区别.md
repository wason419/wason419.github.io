---
layout: post
title: 理解 innerText 和 textContent 的区别
subtitle: 笔记
date: 2019-11-28
author: Wason
header-img: img/bg/post-bg-04.jpg
catalog: true
tags:
  - innerText/textContent
---

# innerText 和 textContent 的区别 #
### 区分
相同点：innerText和textContent的作用都是获取元素之间的文本内容

---
不同点：返回文本内容的样式、回流和兼容性不同
1. innerText的值依赖于浏览器的显示，textContent依赖于代码的显示（即innerText有丶联系到innerHTML，是面对HTML结构的，以界面显示样式返回。而textContent是获取代码层面的文本，已代码中结构样式返回）
2. 如果一个元素之间包含了script标签或者style标签，innerText是获取不到这两个元素之间的文本的，而textContent可以
3. textContent会把空标签解析成换行（几个空标签就是几行），innerText只会把block元素类型的空标签解析换行，并且如果是多个的话仍看成是一个，而inline类型的原素则解析成空格
4. innerText的设置一定会引发浏览器的回流操作(比较耗性能)，而textContent则当所赋值的内容超出了容器尺寸，影响到了页面整体布局时，才会触发回流，一般只是触发浏览器的重绘
5. 兼容性：innerText对IE兼容性较好（IE6+），textContent则兼容IE9+。
不同浏览器对两者兼容性不同，最好用前做下判断
```
if(el.textContent){
    el.textContent='666';
}else{
    el.innerText='666';
}
```

参考网文： [innerText 和 textContent 的区别][1]

[1]: https://www.jianshu.com/p/13096ec76ad2

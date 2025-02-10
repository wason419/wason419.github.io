---
layout: post
title: JS的reduce函数
subtitle: 笔记
date: 2021-09-16
author: Wason
header-img: img/bg/post-bg-12.jpg
catalog: true
tags:
  - reduce
  - JS
---

# JS的reduce函数 #
## 前言 ##
reduce() 方法对数组中的每个元素执行一个由您提供的reduce函数(升序执行)，将其结果汇总为单个返回值。reduce方法可做的事情特别多，就是循环遍历能做的，reduce都可以做，比如数组求和、数组求积、数组中元素出现的次数、数组去重等等。

## 详述 ##  

**`语法`**
```js
  arr.reduce(function(prev,cur,index,arr){
  ...
  }, init);

  参数：
  - prev 必需。累计器累计回调的返回值; 表示上一次调用回调时的返回值，或者初始值 init;
  - cur 必需。表示当前正在处理的数组元素；
  - index 可选。表示当前正在处理的数组元素的索引，若提供 init 值，则起始索引为 0，否则起始索引为 1；
  - arr 可选。表示原数组；
  - init 可选。表示初始值。prev的初始值
```


**`举例`**  
**1. 没有设置函数的初始迭代值**
```js
const arr = [1,2,3,4,5];
const sum = arr.reduce(function(prev,cur,index,arr){
    console.log(prev,cur,index);
    return prev + cur;
});
console.log('arr:',arr,'sum:',sum);

VM538:3     1 2 1
VM538:3     3 3 2
VM538:3     6 4 3
VM538:3     10 5 4
VM538:6     arr: (5) [1, 2, 3, 4, 5] sum: 15
```
分析：在这里reduce的作用就是对这个数组进行求和，迭代了4次，函数迭代的初始值是1，也就是默认值（数组的第一项），prev的值是每次计算后的值。

**2. 设置函数的初始迭代值**
```js
const arr = [1,2,3,4,5];
const sum = arr.reduce((prev,cur,index,arr) => {
    console.log(prev,cur,index);
    return prev + cur;
},5);
console.log('arr:',arr,'sum:',sum);

VM542:3     5 1 0
VM542:3     6 2 1
VM542:3     8 3 2
VM542:3     11 4 3
VM542:3     15 5 4
VM542:6     arr: (5) [1, 2, 3, 4, 5] sum: 20
```
运行结果：
分析：这里我们添加了一个初始的迭代值，也就是让prev从5开始计算(以5为初始值求和)，可以看到，这里迭代了5次，结果也加上了初始值。同时，这里使用箭头函数来代替完整的函数。 运行结果相同。

**`reduce的应用`**  
**1. 数组求和**
```js
let arr = [1,2,3,4,5]
console.log(arr.reduce((a,b) => a + b))         // 15
console.log(arr.reduce((a,b) => a * b))         // 120
```


**2. 计算数组中每个元素出现的次数**
```js
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice', 'Bob', 'Bob'];
var countedNames = names.reduce(function (allNames, name) {
  console.log(allNames,name);
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
console.log(countedNames);

VM550:3     {} 'Alice'
VM550:3     {Alice: 1} 'Bob'
VM550:3     {Alice: 1, Bob: 1} 'Tiff'
VM550:3     {Alice: 1, Bob: 1, Tiff: 1} 'Bruce'
VM550:3     {Alice: 1, Bob: 1, Tiff: 1, Bruce: 1} 'Alice'
VM550:3     {Alice: 2, Bob: 1, Tiff: 1, Bruce: 1} 'Bob'
VM550:3     {Alice: 2, Bob: 2, Tiff: 1, Bruce: 1} 'Bob'
VM550:12     {Alice: 2, Bob: 3, Tiff: 1, Bruce: 1}

```
由于设置了迭代初始值，allNames的第一个值是一个空对象，此时name为Alice，然后进行判断，发现在allNames中没有Alice属性，所以就将Alice对应的属性值赋为1
后面没有重复的是一样的道理，如果碰到重复值，就会将该属性值加1，这样就能计算元素重复的次数了。


**3. 去除数组中重复的元素**
```js
let myArray = ['a', 'b', 'a', 'b', 'c', 'e', 'e', 'c', 'd', 'd', 'd', 'd'];
let myOrderedArray = myArray.reduce(function (accumulator, currentValue) {
  if (accumulator.indexOf(currentValue) === -1) {
    accumulator.push(currentValue)
  }
  return accumulator;
}, []);
console.log(myOrderedArray);      // ['a','b','c','d']

VM554:8     (5) ['a', 'b', 'c', 'e', 'd']

------------------------------------------------------------------------------------------------

let arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
let result = arr.sort().reduce((init, current) => {
    if(init.length === 0 || init[init.length-1] !== current) {
        init.push(current);
    }
    return init;
}, []);
console.log(result);     // [1,2,3,4,5]

VM119:8     (5) [1, 2, 3, 4, 5]
```
分析：这里主要是借助迭代功能实现数组的扩展，判断当前元素是否已经添加到数组中，如果不存在就从尾部添加，这两个方法在去重方法中应该算比较简单高效的。


**4. 按属性对Object分类**
```js
var person = [
    {
        name: 'xiaoming',
        age: 18
    },{
        name: 'xiaohong',
        age: 17
    },{
        name: 'xiaogang',
        age: 17
    }
];
function groupBy(objectArray, property) {
  return objectArray.reduce(function (acc, obj) {
    var key = obj[property];
    if (!acc[key]) {
      acc[key] = [];
    }
    acc[key].push(obj);
    return acc;
  }, {});
}
var groupedPerson = groupBy(person, 'age');
console.log(JSON.stringify(groupedPerson));

VM198:24     {"17":[{"name":"xiaohong","age":17},{"name":"xiaogang","age":17}],"18":[{"name":"xiaoming","age":18}]}
```

**5. 对对象的属性求和**
```js
let people = [
  { name: 'Alice', age: 21 },
  { name: 'Max', age: 20 },
  { name: 'Jane', age: 20 }
];
let result = people.reduce((a,b) =>{
    a = a + b.age;
    return a;
},0)

console.log(result) // - 结果：61

VM202:11     61
```
分析：  
 这里主要就是利用reduce第一个参数是迭代，可以通过初始化这个参数的数据类型，达到想实现的效果。

     
**`总结`**  
reduce() 是数组的归并方法，与 forEach()、map()、filter()等迭代方法一样都会对数组每一项进行遍历，但是reduce() 可同时将前面数组项遍历产生的结果与当前遍历项进行运算，这一点是其他迭代方法无法企及的。

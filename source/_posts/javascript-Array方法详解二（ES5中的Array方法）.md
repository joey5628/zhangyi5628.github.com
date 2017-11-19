---
title: javascript-Array的方法详解二（ES5中的Array方法）
date: 2016-04-22 18:43:17
categories: javascript
tags: [javascript, 数组]
---
### 一、前言
ES5中新增了许多Array的方法，巧用这些方法能够很好的提示代码质量和性能，自己之前也经常忘记这些方法的用法，所以写一写巩固一下。
### 二、索引
ES5中新增的数组方法如下：
1.[isArray](#isArray)
2.[indexOf](#indexOf)
3.[lastIndexOf](#lastIndexOf)
4.[every](#every)
5.[some](#some)
6.[forEach](#forEach)
7.[map](#map)
8.[filter](#filter)
9.[reduce](#reduce)
10.[reduceRight](#reduceRight)
### 三、详解
#### <span id="isArray">1.Array.isArray()</span>
判断一个元素是否为数组，不多解释。
低版本浏览器可以如下扩展：
```js
if(!Array.isArray){
    Array.isArray = function(arg){
        return Object.prototype.toString.call(arg) === '[object Array]';
    }
}
```
#### <span id="indexOf">2.indexOf</span>
和string.indexOf(searchString, position)中的indexOf方法类似。
```js
array.indexOf(searchElement[, fromIndex = 0])
```
返回第一个匹配到的元素整数索引，没有匹配到就返回-1（注意使用的时严格匹配模式）。fromIndex可选，表示从这个位置开始匹配，默认值是0。
```js
[1,2,3,4].indexOf('2'); //注意使用的时严格匹配模式
//-1
[1,2,3,4].indexOf(2);
//1
```
兼容处理如下：
```js
if (!Array.prototype.indexOf) {
  Array.prototype.indexOf = function(searchElement, fromIndex) {
    var k;
    if (this == null) {
      throw new TypeError('"this" is null or not defined');
    }
    var o = Object(this);

    var len = o.length >>> 0;
    if (len === 0) {
      return -1;
    }
    var n = +fromIndex || 0;
    if (Math.abs(n) === Infinity) {
      n = 0;
    }
    if (n >= len) {
      return -1;
    }
    k = Math.max(n >= 0 ? n : len - Math.abs(n), 0);
    while (k < len) {
      if (k in o && o[k] === searchElement) {
        return k;
      }
      k++;
    }
    return -1;
  };
}
```
#### <span id="lastIndexOf">3.lastIndexOf</span>
```js
array.lastIndexOf(searchElement[, fromIndex])
```
lastIndexOf和indexOf方法类似，只是lastIndexOf是从数组的末尾开始查询，fromIndex的默认值是array.length - 1.
#### <span id="every">4.every</span>
```js
array.every(callback,[ thisObject]);
```
every和some 都是用来测试数组中的项是否满足某一条件。every只有当所有项全部满足时才返回true，some只要有一个满足就返回true。
```js
[12, 5, 8, 130, 44].every(function(element){
    return element > 10
});
//false
```
兼容写法如下：
```js
if (typeof Array.prototype.every != "function") {
  Array.prototype.every = function (fn, context) {
    var passed = true;
    if (typeof fn === "function") {
       for (var k = 0, length = this.length; k < length; k++) {
          if (passed === false) break;
          passed = !!fn.call(context, this[k], k, this);
      }
    }
    return passed;
  };
}
```
#### <span id="some">5.some</span>
```js
array.some(callback,[ thisObject]);
```
some是判断数组有没有符合条件的元素，只要有一项满足就返回true，和every正好相反。
```js
[12, 5, 8, 130, 44].some(function(element){
    return element > 10
});
//true
```
#### <span id="forEach">6.forEach</span>
```js
array.forEach(callback[, thisArg])
```
forEach是循环遍历数组中的每一个元素，等同于for循环。
forEach的callback支持3个参数：
currentValue：遍历到得当前元素  
index：对应的数组索引  
array：数组本身  
```js
[1, 2 ,3].forEach(function(){console.log(arguments));
// 1, 0, [1, 2, 3]
// 2, 1, [1, 2, 3]
// 3, 2, [1, 2, 3]
```
forEach不会去遍历数组中空的元素。
```js
var array = [1, 2, 3];
delete array[1]; // 移除 2
alert(array); // "1,,3"
alert(array.length); // but the length is still 3
array.forEach(alert); // 弹出的仅仅是1和3
```
兼容老版浏览器的写法如下：
```js
if (typeof Array.prototype.forEach != "function") {
  Array.prototype.forEach = function (fn, context) {
    for (var k = 0, length = this.length; k < length; k++) {
      if (typeof fn === "function" && Object.prototype.hasOwnProperty.call(this, k)) {
        fn.call(context, this[k], k, this);
      }
    }
  };
}
```
#### <span id="map">7.map</span>
```js
array.map(callback[, thisArg])
```
map是指通过callback方法把原数组“映射”成对应的新数组，用法和forEach类似。
callback同样支持三个参数：
currentValue：遍历到得当前元素  
index：对应的数组索引  
array：数组本身
```js
var arr1 = [1, 2, 3, 4];
var arr2 = arr1.map(function(item){return item*2});
console.log(arr2);
//[2, 4, 6, 8]

var arr3 = arr1.map(function(item){item*2});
console.log(arr3);
//[undefined, undefined, undefined, undefined]
```
callback中如果return，就会返回undefined。
兼容老版浏览器的写法如下：
```js
if (typeof Array.prototype.map != "function") {
  Array.prototype.map = function (fn, context) {
    var arr = [];
    if (typeof fn === "function") {
      for (var k = 0, length = this.length; k < length; k++) {      
         arr.push(fn.call(context, this[k], k, this));
      }
    }
    return arr;
  };
}
```
#### <span id="filter">8.filter</span>
```js
array.filter(callback[, thisArg])
```
filter为“过滤”、“筛选”之意。指数组filter后，返回过滤后的新数组。
filter的callback函数需要返回布尔值true或false. 如果为true则表示通过添加到新数组中，如果是false这不通过。
```js
var data = [0, 1, 2, 3];
var arrayFilter = data.filter(function(item) {
    return item;
});
console.log(arrayFilter);
// [1, 2, 3]
```
返回值只要是弱等于== true/false就可以了，而非非得返回 === true/false
兼容老版浏览器的写法如下：
```js
if (!Array.prototype.filter) {
  Array.prototype.filter = function(fun/*, thisArg*/) {
    'use strict';

    if (this === void 0 || this === null) {
      throw new TypeError();
    }

    var t = Object(this);
    var len = t.length >>> 0;
    if (typeof fun !== 'function') {
      throw new TypeError();
    }
    var res = [];
    var thisArg = arguments.length >= 2 ? arguments[1] : void 0;
    for (var i = 0; i < len; i++) {
      if (i in t) {
        var val = t[i];
        if (fun.call(thisArg, val, i, t)) {
          res.push(val);
        }
      }
    }

    return res;
  };
}
```
#### <span id="reduce">9.reduce</span>
```js
array.reduce(callback[, initialValue])
```
reduce中callback函数接受4个参数：之前值(上一次的处理结果 - 通过上一次调用回调函数获得的值)、当前值、索引值以及数组本身。initialValue参数可选，表示初始值。若指定，则当作最初使用的previous值；如果缺省，则使用数组的第一个元素作为previous初始值，同时current往后排一位，相比有initialValue值少一次迭代。
```js
var sum = [1, 2, 3, 4].reduce(function (previous, current, index, array) {
  return previous + current;
});

console.log(sum); // 10
```
有了reduce，我们可以轻松实现二维数组的扁平化：
```js
var matrix = [
  [1, 2],
  [3, 4],
  [5, 6]
];

// 二维数组扁平化
var flatten = matrix.reduce(function (previous, current) {
  return previous.concat(current);
});

console.log(flatten); // [1, 2, 3, 4, 5, 6]
```
#### <span id="reduceRight">10.reduceRight</span>
reduceRight跟reduce相比，用法类似：
```js
array.reduceRight(callback[, initialValue])
```
实现上差异在于reduceRight是从数组的末尾开始实现。看下面这个例子：
```js
var data = [1, 2, 3, 4];
var specialDiff = data.reduceRight(function (previous, current, index) {
  if (index == 0) {
    return previous + current;
  }
  return previous - current;
});

console.log(specialDiff); // 0
```

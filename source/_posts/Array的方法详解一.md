---
title: Array的方法详解一
date: 2016-05-08 16:20:25
categories: javascript
tags: [javascript, 数组]
---
### 一、前言
闲来无事，把js中数组的方法给整理下，发现自己很多方法不常用，容易忘记这些方法具体是做什么的。另外也给自己的博客充实点内容。
js中的Array比较奇怪，和java等其他语言相比，js中的数组每一项可以保存任何类型的数据，而且数组的发现是可以动态调整的，即可以随着数据的添加自动增长以容纳新增数据。数组的length属性也很特别，它不是只读的，因此通过设置length属性，可以做到向数组的末尾移除或添加新元素。
js中所有的对象都具有toLocaleString()、toString()、valueOf()三个方法，数组也不例外。
### 二、索引
1.[push](#push)
2.[pop](#pop)
3.[shift](#shift)
4.[unshift](#unshift)
5.[reverse](#reverse)
6.[sort](#sort)
7.[concat](#concat)
8.[slice](#slice)
9.[splice](#splice)
10.[join](#join)
### 三、详解
#### <span id="push">1.Array.push()</span>
js中数组有push()和pop()方法，可以用这两个方法实现类似于栈的行为，即最新添加的最早被移除。栈是一种（Last-In-First-out)后进先出的数据结构。  
push()方法可以接收任意数量的参数，把它们逐个添加到数组的末尾，并返回添加后的数组长度。
```js
var arr = [],
    count = arr.push(1,2);
console.log(count); //2
console.log(arr);   //[1,2]
```
#### <span id="pop">2.Array.pop()</span>
pop()方法则从数组的末尾移除最后一项元素，减少数组的length值，并返回移除的元素。
```js
var arr = [1,2,3],
    item = arr.pop();
console.log(item);          //3
console.log(arr.length);    //2
console.log(arr);           //[1,2]
```
#### <span id="shift">3.Array.shift()</span>
shift方法可以移除数组中得第一个元素并返回该元素，结合使用shift()和push()就可以想使用队列一样使用数组。栈数据结构的访问规则是后进先出，而队列数据结构的访问规则是先进先出。
```js
var arr = [1,2,3],
    item = arr.shift();
console.log(item);          //3
console.log(arr.length);    //2
console.log(arr);           //[2,3]
```
#### <span id="unshift">4.Array.unshift()</span>
unshift()方法和shift()的用途正好相反，它能在数组的前端添加任意个元素并返回新数组的长度。和push()方法用法类似，不同点是push是向数组的末尾添加元素。同时使用unshift()和pop()方法，就可以从相反的方向模拟队列，即在数组的前端添加元素，从数组的末尾移除元素。
```js
var arr = [1,2,3],
    count = arr.unshift(-1,0);
console.log(arr.length);    //5
console.log(arr);           //[-1,0,1,2,3]
```
#### <span id="reverse">5.Array.reverse()</span>
数组中又两个可以直接用来重排序的方法：reverse()和sort()。reverse()方法为反转数组元素的顺序。
```js
var arr = [1,2,3];
arr.reverse();
console.log(arr);   //[3, 2, 1]
```
#### <span id="sort">6.Array.sort()</span>
默认情况下，sort()方法调用每个数组元素的toString()方法来转型，然后比较得到的字符串，按升序排列数组元素。即使数组的每一项都是number类型，sort()方法比较的也是字符串。
```js
var arr = [0,1,5,10,15];
arr.sort();
console.log(arr);   //[0,1,10,15,5]
```
sort()方法可以接收一个函数作为参数，函数接收相邻的两个数组元素作为参数。如果第一个参数应该位于第二个参数之前就返回一个负数，如果两个参数相等则返回0，如果第一个参数应该位于第二个参数之后在返回一个正数。
```js
var arr = [0,1,5,10,15];
arr.sort(function(val1, val2){
    if(val1 < val2){
        return 1;
    }else if(val1 > val2){
        return -1;
    }else{
        return 0;
    }
});
console.log(arr);   //[15, 10, 5, 1, 0] 倒序
```
#### <span id="concat">7.Array.concat()</span>
concat()方法会先创建当前数组的一个副本，然后将接收到的参数添加到这个副本的末尾，最后返回新构建的数组。如果没有传参数，就会只返回当前数组的副本。
```js
var arr = [1,2],
    arr2 = arr.concat(3,[4,5]);
console.log(arr);   //[1,2]
console.log(arr2);  //[1,2,3,4,5]
```
#### <span id="slice">8.Array.slice()</span>
slice()方法可以基于当前数组中的一个或多个元素创建一个新数组。slice()方法可以接受一或两个参数，即要返回元素的起始和结束位置。当只有一个参数时，slice()方法会返回从该参数指定的位置开始到当前数组末尾所有元素。如果是两个参数，则返回起始和结束位置之前的元素，但不包括结束位置的元素。
```js
var arr = [1,2,3,4,5],
    arr2 = arr.slice(3),
    arr3 = arr.slice(3,4);
console.log(arr);   //[1, 2, 3, 4, 5]
console.log(arr2);  //[4, 5]
console.log(arr3);  //[4]
```
如果slice()方法的参数中有一个负数，则用数组长度加上该数来确定相应的位置。对于长度是5的数组，slice(-2,-1)与slice(3,4)得到的结果相同。如果结束位置小于起始位置，则返回空数组。
#### <span id="splice">9.Array.splice()</span>
splice()方法很强大，它有很多种用法，splice()方法主要用来向数组中部插入元素，使用这个方法的方式有3种。
删除：可以删除任意数量的元素，只需指定2个参数：要删除的第一个元素的位置和要删除的元素数量。例如splice(0,2)会删除数组中的前两项。
插入：可以向指定位置插入任意数量的元素，需提供3个参数：起始位置，0（要删除的元素数量）和要插入的元素。
替换：可以向指定位置插入任意数量的元素，且同事删除任意数量的元素，需要指定3个参数：起始位置，要删除的元素数量和要插入的任意数量的元素。插入的项数不必与删除的项数相等。
```js
//删除
var arr = [1,2,3,4,5],
    arr1 = arr.splice(0,2);
console.log(arr);   //[3, 4, 5]
console.log(arr1);  //[1, 2]

//插入
var arr = [1,2,3,4,5],
    arr1 = arr.splice(1, 0, [-1,0]);
console.log(arr);   //[1, -1, 0, 2, 3, 4, 5]

//替换
var arr = [1,2,3,4,5],
    arr1 = arr.splice(0, 2, 0, 0);
console.log(arr);   //[0, 0, 3, 4, 5]
```
最容易忘记的就是splice()方法，反正我是经常忘记该怎么用这个方法。
#### <span id="join">10.Array.join()</span>
join()方法只接收一个参数，即用作分隔符的字符串，然后返回包含所有数组项的字符串。
```js
var arr = [1,2,3];
console.log(arr.join('|'));     //1|2|3
```




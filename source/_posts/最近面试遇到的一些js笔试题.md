---
title: 最近面试遇到的一些js笔试题
date: 2016-04-06 18:28:10
categories: javascript
tags: [javascript, 笔试题]
---
#### 1、随机给出一个字符串，例如：adadfdfseffserfefsefsee，找出出现次数最多的字母。
这道题有两种做法，
第一种方法是创建一个object，循环字符以字符为object的key，初始的value为1，循环的过程中，遇到同样的字符，就+1，最后得到的时类似于这样的对象：
```js
{
    a:2,
    b:10,
    c: 2
    ...
}
```
在对这个对象的每个key和value做排序。当然这种方法不好，要做几遍循环，比较消耗性能。  

第二种方法是循环字符，把字符串中相同的字母都替换为空，再用本次循环中最初的字符串长度减去替换后的字符串长度，就得到了本次循环的字符在字符串中的总数，而且每次循环都会吧相同的字符替换为空，循环需要的次数越来越少，能提高性能。代码如下：
```js
var str ="adadfdfseffserfefsefseeffffftsdg"; //命名一个变量放置给出的字符串
var maxLength = 0; //命名一个变量放置字母出现的最高次数并初始化为0
var result = ''; //命名一个变量放置结果输入  

while( str != '' ){ //循环迭代开始，并判断字符串是否为空
    oldStr = str; //将原始的字符串变量赋值给新变量
    getStr = str.substr(0,1); //用字符串的substr的方法得到第一个字符（首字母）
    str = str.replace(new RegExp(getStr, "g"), "");

    if( oldStr.length-str.length > maxLength ) { //判断原始的字符串的长度减去替代后字符串长度是否大于之前出现的最大的字符串长度
        maxLength = oldStr.length-str.length; //两字符串长度相减得到最大的字符串长度
        result = getStr + "=" + maxLength //返回最大的字符串结果（字母、出现次数）
    }
}  

alert(result) //弹出结果 
```

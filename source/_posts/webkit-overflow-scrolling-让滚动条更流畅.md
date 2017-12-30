---
title: webkit-overflow-scrolling 让滚动条更流畅
date: 2017-10-10 11:14:24
tags:
---
Html5页面一般滚动效果和iOS原生的效果比都会比较生涩，没有回弹效果。  
如果我们想要iOS小的滚动效果，一般有两种方法，一是使用iscroll来模拟，但是这样代价太大。还有一种方法就比较简单，可以加上iOS独有的属性：
```css
-webkit-overflow-scrolling: touch;
```
详见：https://developer.mozilla.org/zh-CN/docs/Web/CSS/-webkit-overflow-scrolling

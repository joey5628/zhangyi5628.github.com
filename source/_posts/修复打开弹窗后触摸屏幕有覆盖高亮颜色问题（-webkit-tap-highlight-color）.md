---
title: 修复打开弹窗后触摸屏幕有覆盖高亮颜色问题（-webkit-tap-highlight-color）
date: 2016-10-19 09:08:49
tags:
---
### 问题
最近在做Html5弹窗组件（Modal）的时候，发现在弹窗打开后触摸屏幕时全屏会被覆盖半透明的灰色背景，着实让我摸不着头脑，后来查看Antd Design Mobile组件库源码时候发现了解决方法。原来是需要设置-webkit-tap-highlight-color 属性
### -webkit-tap-highlight-color
这个属性只用于iOS (iPhone和iPad)。当你点击一个链接或者通过Javascript定义的可点击元素的时候，它就会出现一个半透明的灰色背景。要重设这个表现，你可以设置-webkit-tap-highlight-color为任何颜色。
想要禁用这个高亮，设置颜色的alpha值为0即可  
大部分android手机也是支持的，只是显示效果有所不同。
#### 全局禁用
```css
*,
*:before,
*:after {
  -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
}
```
### 其他鲜为人知的属性
#### （一）css3中-webkit-text-size-adjust详解：
1、当样式表里font-size<12px时，中文版chrome浏览器里字体显示仍为12px，这时可以用  

```css
html{-webkit-text-size-adjust:none;}  
```
2、-webkit-text-size-adjust放在body上会导致页面缩放失效  
3、body会继承定义在html的样式  
4、用-webkit-text-size-adjust不要定义成可继承的或全局的  
#### （二）webkit-appearance
-webkit-appearance: none;  
//消除输入框和按钮的原生外观，在iOS上加上这个属性才能给按钮和输入框自定义样式。    
注意：不同type的input使用这个属性之后表现不一。text、button无样式，radio、checkbox直接消失  
#### （三）-webkit-user-select
-webkit-user-select: none;  
// 禁止页面文字选择 ，此属性不继承，一般加在body上规定整个body的文字都不会自动调整
#### （四）-webkit-touch-callout
-webkit-touch-callout:none;  
// 禁用长按页面时的弹出菜单(iOS下有效) ,img和a标签都要加
#### （五）-webkit-appearance
-webkit-appearance:button;  
使元素看上去像一个按钮

layout: safari
title: Safari active伪类失效问题
date: 2016-10-28 09:13:55
tags:
---
最近在做移动端项目时，发现通过伪类:active给按钮添加点击效果时，iOS下无效，PC下是正常的。
### 1.失效原因
我们可以从Safari Developer Library中找到答案,描述如下:
> “You can also use the -webkit-tap-highlight-color CSS property in combination with setting a touch event to configure buttons to behave similar to the desktop. On iOS, mouse events are sent so quickly that the down or active state is never received. Therefore, the :active pseudo state is triggered only when there is a touch event set on the HTML element—for example, when ontouchstart is set on the element as follows:”
```
<button class="action" ontouchstart=""  
style="-webkit-tap-highlight-color: rgba(0,0,0,0);">  
    Testing Touch on iOS
</button>
```
> “Now when the button is tapped and held on iOS, the button changes to the specified color without the surrounding transparent gray color appearing.”

其中的描述,在iOS上,鼠标事件是太快了,按钮及active状态无法触发,因此,:active状态只能在设置了touch 事件的元素上触发,具体可详见上文中标红色部分.

### 2.解决方案
#### 1) 像上面那样:
```html
<button ontouchstart="" >Testing Touch on iOS</button>
```
#### 2)另一种就是在docuemnt上绑定touchstart事件
```
document.addEventListener("touchstart", function() {
   // do nothing
},false);
```

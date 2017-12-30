---
title: touch-action控制触摸操作
date: 2017-11-24 21:45:31
tags: [HTML5]
---
#### touch-action
CSS属性 touch-action 用于指定某个给定的区域是否允许用户操作，以及如何响应用户操作，用于控制元素的默认触摸行为。  

常用的 touch-action 值：
|  触摸操作参数 | 描述
| -------- | ------------- |
| touch-action: none | 浏览器将不处理触摸交互。 |
| touch-action: pinch-zoom | 像 `touch-action: none` 一样停用所有浏览器交互（ `pinch-zoom` 除外，该交互仍由浏览器处理。 |
| touch-action: pan-y pinch-zoom | touch-action: pan-y pinch-zoom |
| touch-action: manipulation |  停用点按两次手势，可避免浏览器的任何点按延迟。 将滚动和双指张合缩放交由浏览器处理。 |

touch-action: none 来防止浏览器在用户触摸时执行任何操作，从而拦截所有触摸事件。  
使用 touch-action: none 的影响颇为巨大，因为它会阻止所有默认的浏览器行为。 在许多情况下，采用下面其中一个解决方案是更好的选择。

touch-action 可停用浏览器实现的手势。例如，IE10 以上版本支持点按两次执行缩放手势。 将 touch-action 设置为 manipulation 可以阻止点按两次的默认行为。

这样您就可以自行实现点按两次手势。

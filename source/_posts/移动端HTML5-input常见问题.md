---
title: 移动端HTML5 input常见问题
date: 2016-11-22 21:41:38
tags: [HTML5]
---

#### 1. 去掉input 在iOS中的默认圆角和内阴影
iOS下 input会有自带的圆角和内阴影，去掉方法如下：
```css
input{
	-webkit-appearance: none;
	border-radius: 0;
}
```
#### 2. 焦点在 input 时，placeholder 没有隐藏
```css
input:focus::-webkit-input-placeholder{
	opacity: 0;
}
```
#### 3. input 输入框调出数字键盘
单独使用type="number"时，iOS调起的并不是九宫格样式的数字键盘，如果需要调起九宫格的数字键盘需要加上 pattern="[0-9]*" 属性
```html
<!-- 数字键盘 带有符号，非九宫格样式 -->
<input type="number"/>

<!-- 九宫格数字键盘 -->
<input type="number" pattern="[0-9]*"/>

<!-- 电话号码键盘 -->
<input type="tel"/>
```

#### 4. 搜索时，键盘的回车按钮文字设定为“搜索”
解决： input 使用 type="search"，放在 form 表单内。两者结合就能使输入法中的回车按钮文字变为“搜索”  
```html
<form action="">
	<input type="search" />
</form>
```

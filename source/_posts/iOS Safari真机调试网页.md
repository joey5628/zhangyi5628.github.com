layout: iphone
title: iOS Safari 真机调试网页
date: 2017-11-24 21:29:15
tags:
---
#### 概述
平时开发移动端页面时，我们一般也都先选择再chrome上模拟手机浏览器来调试，这样很方便。但是有些时候电脑上运行的好好的，在手机上还是有出现莫名其妙的问题，让人捉不到头脑，这个时候如果能在真机上运行，在电脑上的浏览器中调试就再好不过了。  

对于iPhone来说，iOS5.0开始，可以通过Safari的Web Inspector工具连接设备对应用进行调试。

#### 真机调试
##### 一、开启iOS设备的调试功能
打开“设置 - Safari - 高级”页面，开启“Web检查器”  

<img src="http://owtrjd7fu.bkt.clouddn.com/Safari%E7%9C%9F%E6%9C%BA%E8%B0%83%E8%AF%95-iOS%E5%BC%80%E5%90%AF%E8%B0%83%E8%AF%95.jpeg" width = "330" alt="图片名称" align=center />  

其他手机上安装的需求调试的应用，打开对应的WebView页面或者是浏览器，并把手机连接到电脑。

##### 二、开启Safari的调试功能
运行Safari,点击Safari的“偏好设置”，切换到“高级”  

<img src="http://owtrjd7fu.bkt.clouddn.com/Safari%E7%9C%9F%E6%9C%BA%E8%B0%83%E8%AF%95-%E6%89%93%E5%BC%80%E5%BC%80%E5%8F%91%E8%8F%9C%E5%8D%95.jpg" width = "620" alt="开启Safari的调试功能" align=center />  

勾选“在菜单中显示‘开发’菜单”，关闭偏好设置，这是在Safari的工具栏上就能看到“开发”菜单。

##### 三、连接真机
点击“开发”这时能看到连接的设备，hover到对应的设备上，选择连接要调试的页面  

<img src="http://owtrjd7fu.bkt.clouddn.com/Safari%E7%9C%9F%E6%9C%BA%E8%B0%83%E8%AF%95-%E8%BF%9E%E6%8E%A5%E8%B0%83%E8%AF%95.jpg" width = "620" alt="连接调试" align=center />  

#### 四、调试
打开Web Inspector界面后，即可调试JavaScript、检查HTML页面DOM结构、实时同步更新元素CSS样式、加断点调试等操作  

<img src="http://owtrjd7fu.bkt.clouddn.com/Safari%E7%9C%9F%E6%9C%BA%E8%B0%83%E8%AF%95-%E8%B0%83%E8%AF%95.jpg" width = "620" alt="调试" align=center />

很 easy!

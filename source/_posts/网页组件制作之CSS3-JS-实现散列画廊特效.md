---
title: 网页组件制作之CSS3+JS 实现散列画廊特效
date: 2016-11-22 14:41:03
categories: coding
tags:
  - HTML
  - CSS
  - JavaScript
---


**在该笔记中将学到列表展示中散列画廊特效的方法与展示效果**

本系列的是参考[慕课网 Lyn](http://www.imooc.com/u/104593/courses?sort=publish)老师的视频，仅供个人查阅使用，具体讲解推荐参考视频。

## VCD 分解

1.`View`：视觉，是 HTML + CSS，基本界面模版
2. `Controller`：控制，JavaScript，内容处理，事件处理
3. `Data`：数据，data.js，非必须，助于理解

### 视觉部分

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img66.PNG)

### 数据部分

**有一个 data.js 来处理所有的海报图片链接、标题、描述**

### 控制部分

控制海报的批处理，分配海报的位置，设置控制条的动作，海报的翻面控制。

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img68.PNG)

**使用 `-webkit-perspective:800px` 设置子元素对 3D 效果的支持。这里设置的是子元素距离视图的位置**

**使用 `-webkit-backface-visibility:hidden` 设置元素不面向屏幕的时候进行隐藏**

**使用 `-webkit-transform-style:preserve-3d` 设置支持子元素的 3D 效果**

** `-webkit-transform: rotateY(0deg)` 定义元素旋转的角度**

**使用 `transition` 设置过渡动画**



	<html>
	    <head>
	        <meta charset="utf-8">
	        <title>海报画廊</title>
	        <link rel="stylesheet" type="text/css" href="test.css">
	        <script src="test.js">
	            
	        </script>
	    </head>
	    <body>
	        <div class="wrap">
	            <!-- photo 负责平移和旋转-->            
	            <div class="photo photo-center">
	                <!-- photo-wrap 负责照片的翻转-->
	                <div class="photo-wrap">
	                    <div class="side-front">
	                        <img src="./images/1.jpg" alt="">
	                        <p>蜀南竹海</p>
	                    </div>
	                    <div class="side-back">
	                        <p>照片1描述</p>
	                    </div>                                   
	                </div>
	            </div>
	        </div>
	    </body>
	</html>



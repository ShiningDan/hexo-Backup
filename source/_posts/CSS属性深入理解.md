---
title: CSS属性深入理解
date: 2016-11-09 22:07:03
categories: coding
tags:
  - CSS
---

# CSS 属性深入理解

**在这个属性中，会对一些 CSS 进行深入的介绍。**

## CSS 深入理解之 float

**float 设计的初衷，是为了实现文字的环绕效果：**也就是为了不显示为图片一行，文字一行的行为，而是文字对图片的包裹，如下图所示。

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img38.PNG)

### float 的行为特点

<!--more-->

#### 包裹性

**BFC(Block Formatting Context)：块级格式化上下文**：具有 BFC 特点的元素块，将有一下特性：

1. 收缩：包住元素的块的 height 和 width 的大小将自动包裹住浮动的元素，并且紧紧包裹，不会在宽和高上有多余的空间
2. 隔绝：在具有 float 属性的元素内部发生的任何事情，都不会对外部的元素有任何影响，对外部元素来说，使用了 float 的元素，就是具有一定 height 和 width 的块

同样具有包裹特性的属性有：

1. display: inline-block/table-cell/..
2. position: absolute/fixed/sticky
3. overflow: hidden/scroll

#### 破坏性

**设置了 float 属性的元素，父元素会出现高度塌陷的情况：**如果父元素没有设置 width 和 height，则表现特点为父元素的无法被该元素撑开，或者父元素的高度等于其他子元素的高度，表现如下图。

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img39.PNG)

#### 元素 block 化

使用了 float 属性的元素，其 display 属性会被自动设置为 block，也就是自动变成块状元素在文档流中占据空间。

#### 元素去空格化

在正常文档流中，如果在 HTML 代码中，标签之间有回车，Tab，空格等符号，在页面显示中会有一个空格占位，但是**设置了 float 属性的元素，不会有空格来占位，显示的效果就是浮动元素紧密排列**。因为这是为了实现文字环绕所需要的特性。

如果是使用 &nbsp (空格)来分割元素，则网页在渲染的时候，会将 float 元素排列在一起，然后所有的 &nbsp 都会被视为文字元素，与其他文字进行重排，然后环绕浮动元素。

### float 让父元素塌陷

因为浮动 float 的**设计目的就是为了实现文字的环绕效果**，所以对添加了浮动的特性，不会对父元素的高度和宽度进行撑开，其他的文字将在不考虑浮动元素的存在的条件下进行文字顺序的重排。因为要实现文字环绕效果，所以**具有浮动属性的元素还在文档流中占有空间，重排的文字将包裹该元素依次排列显示。**

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img38.PNG)

### 清除浮动的影响

为了消除上一个元素的浮动属性对下一个元素造成影响的情况出现，我们需要下一个元素中设置属性来清除浮动。**下面有两种清除浮动的方法**：

1. **可以设置clear属性**：通过 clear:both 命令可以清除受到影响的元素的浮动，如果明确地知道浮动受到影响的方向，也可以使用 clear:left 或者 clear:right
2. 可以同时设置 **width:100%(或者固定宽度) + overflow:hidden**

下面演示的是使用 clear:both 来清除浮动：

	<html>
	    <head>
	        <title>float测试</title>
	        <meta charset="utf-8">
	        <style type="text/css">
	            #box1{
	                background-color:red;
	                height:50px;
	                float:left;
	            }
	            #box2{
	                background-color:green;
	                height:50px;
	                float:right;
	            }
	            #secondp{
	                clear:both;
	            }
	        </style>
	    </head>
	    <body>
	        <p>this is a para</p>
	        <div id="box1">box1box1box1</div>
	        <div id="box2">box2box2</div>
	        <p id="secondp">this is a para</p>
	    </body>
	</html>

显示效果如下：

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img6.PNG)

### 两种清除浮动方法的选择

**如果浮动元素没有紧邻的下一个元素，则需要对包裹该浮动元素的父元素清除浮动，否则会发生父元素崩塌的情况出现(父元素的范围无法包裹子元素)。**

如果在父元素上使用 clear:both 清除浮动，结果无效！：

	<html>
	    <head>
	        <title>横向两列布局</title>
	        <meta charset="utf-8">
	        <style type="text/css">
	            *{
	                margin:0;
	                padding:0;
	            }
	            #wrap{
	                margin:0 auto;
	                width:540px;
	                border:2px solid black;
	            }
	            #header{
	                height:100px;
	                background-color:red;
	            }
	            #footer{
	                height:100px;
	                background-color:green;
	            }
	            #mainbody{
	                clear:both;
	                border:2px solid blue;
	            }
	            #mainleft{
	                height:200px;
	                width:400px;
	                float:left;
	                background-color:red;
	            }
	            #mainright{
	                height:200px;
	                width:100px;
	                float:left;
	                background-color:green;
	            }
	        </style>
	    </head>
	    <body>
	        <div id="wrap">
	            <div id="header"></div>
	            <div id="mainbody">
	                <div id="mainleft"></div>
	                <div id="mainright"></div>            
	            </div>
	            <div id="footer"></div>
	        </div>
	    </body>
	</html>

显示效果如下：

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img7.PNG)

如图所示，父元素并没有成功地包裹住子元素。

所以在这时，只能使用 overflow:hidden 来对父元素清除浮动：

	<html>
	    <head>
	        <title>横向两列布局</title>
	        <meta charset="utf-8">
	        <style type="text/css">
	            *{
	                margin:0;
	                padding:0;
	            }
	            #wrap{
	                margin:0 auto;
	                width:540px;
	                border:2px solid black;
	            }
	            #header{
	                height:100px;
	                background-color:red;
	            }
	            #footer{
	                height:100px;
	                background-color:green;
	            }
	            #mainbody{
	                clear:both;
	                border:2px solid blue;
	                overflow:hidden;
	            }
	            #mainleft{
	                height:200px;
	                width:400px;
	                float:left;
	                background-color:red;
	            }
	            #mainright{
	                height:200px;
	                width:100px;
	                float:left;
	                background-color:green;
	            }
	        </style>
	    </head>
	    <body>
	        <div id="wrap">
	            <div id="header"></div>
	            <div id="mainbody">
	                <div id="mainleft"></div>
	                <div id="mainright"></div>            
	            </div>
	            <div id="footer"></div>
	        </div>
	    </body>
	</html>

显示效果如下：

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img8.PNG)

如图所示，父元素 mainbody 成功地包裹住了子元素 mainleft 和 mainright。


## CSS 深入理解之 absolute

**position:relative 和 float 有很多相似的特性，以至于有一些使用 float 的地方使用 position:relative 也不会出问题。**

### positition 的行为特点

#### 包裹性

position:absolute 的元素也具有包裹性，包裹性的性质可以查看 float 部分的介绍

#### 破坏性

 position:absolute 也会使得父元素塌陷，其性质可以查看 float 部分的介绍

**但是 position:absolute 的值无法被 overflow 所限制**

#### 去浮动 float

**绝对定位 position:absolute 与 浮动 float 不能同时使用**，如果同时使用，不同浏览器的渲染会产生区别，统一浏览器中多次打开，渲染的效果也会有所区别，所以这两个属性不能同时使用。

#### 定位的跟随性

**设置了 position:absolute 属性的元素会保持在原有的位置，但是脱离了文档流。**

使用 position:absolute 的时候，一般是与 top left right bottom 等属性配合，相对于 relative 元素进行定位。不过，如果使用 position:absolute 与 margin 的配合，实现精确定位，这样的可扩展性更好。

### 绝对定位与动画

**因为绝对定位的标签脱离了文档流，所以动画尽量作用在绝对定位的元素上，这样不会对正常的文档流显示造成影响。**
























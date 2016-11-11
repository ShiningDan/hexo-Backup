---
title: CSS属性深入理解
date: 2016-11-09 22:07:03
categories: coding
tags:
  - CSS
---

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

**因为使用 top、right、left、bottom 属性设置定位位置，位置不会随着页面大小的变化而自动改变，自适应性不大好。除非使用 js 来根据网页的 height 和 width 来自动调整位置。**

如果绝对定位的位置设置同时使用了对立的元素如： top 和 bottom 同时使用，left 和 right 同时使用，则会导致元素的 height 或 width 的拉伸或压缩的效果。(IE7+ 的浏览器支持) 如果 height/width 和 left/right/top/bottom 同时存在，则 height/width 的显示优先级较高。


### 绝对定位与动画

**因为绝对定位的标签脱离了文档流，所以动画尽量作用在绝对定位的元素上，这样不会对正常的文档流显示造成影响。**

## CSS 深入理解之 overflow

overflow 的基本属性

1. visible：默认值，超出范围的部分仍旧会显示
2. hidden：超出的部分会被隐藏
3. scroll：超出的部分会自动添加滚动条，如果没有超出，也会添加滚动条，不过滚动条不可选
4. auto：如果内容超出，则会自动添加滚动条，如果内容不超出，则不会添加滚动条。
5. inherit：从父元素继承

overflow-x 和 overflow-y 这两个属性会控制一个方向上的溢出表现。如果 overflow-x 和 overflow-y 的值相同，则效果等同于 overflow；如果值不相同，且其中一个值为 visible，而另一个值是 hidden/scroll/auto，则 visible 会被重置为 auto。

 ### overflow 与滚动条

**无论什么浏览器，默认滚动条来自 html 标签，而不是 body 标签( body 标签默认有 0.5em 的 margin 值)。**所以，如果想去掉页面默认的滚动条，可以使用

	html { overflow: hidden}

在 chrome 中，滚动条的高度是：

	document.body.scorllTop

在其他浏览器中，滚动条的高度是：

	document.documentElement.scrollTop

所以，跨浏览器的获得滚动高度的方法是：

	var st = document.body.scrollTop || document.documentElement.scrollTop;

***滚动条的自定义样式：**

1. -webkit：webkit 内核的 CSS 属性为：-webkit-scrollbar 等一系列，可以自行学习
2. IE：scrollbar 一系列属性
3. jq 也有自定义滚动条的插件 [github地址](https://github.com/malihu/malihu-custom-scrollbar-plugin)

### overflow 与 BFC

**BFC(Block Formatting Context)：块级格式化上下文**

设置了非 overflow:visible 的元素，会转换为 BFC 布局方法，使用 BFC 布局方法的时候，一定要注意不要和流体布局 float 相互影响。

overflow 与 BFC 的应用一般分为三种：1. 清除浮动 2. 避免 margin 穿透问题 3. 两栏自适应布局

#### 清除浮动

overflow:visible 无法清除浮动，而 scroll auto hidden 可以。

但是 overflow:hidden 会让超出块的元素不可见，所以更通用的写法为：

	.clearfix {*zoom:1;}
	.clearfix:after {conten:''; display:table; clear:both;}


### overflow 与绝对定位

如果一个元素添加了 overflow:hidden 和 position:absolute，则 overflow:hidden 的隐藏效果会失效。

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img40.PNG)
	
	<html>
	    <head>
	        <title>test</title>
	    </head>
	    <body>
	        <div style="width: 200px; height: 200px; overflow: hidden; border: 2px solid #000;">
	            <img style="width: 300px; height: 300px; position: absolute" src="http://ofjjubwp5.bkt.clouddn.com/image/jpg/avatar.jpg" alt="">
	        </div>
	    </body>
	</html>

同样 position:absolute 也会让 overflow:auto 的滚动失效。



**失效原因**：绝对定位元素不总是被父级 overflow 属性剪裁，尤其 overflow 在绝度定位元素及其包含快之间的时候。

**避免方法**：

**包含块**：含有 position:relative/absolute/fixed 声明的父级元素，没有则为 body 元素

1.  overflow 元素自身为包含块(例如给该元素添加 position:relative 的属性)
2.  overlfow 元素的子元素为包含块

### 依赖 overflow 的样式表现

#### resize 属性

CSS3 的 resize 属性，可以**支持元素的尺寸被鼠标拉伸**，例如 resize:both、resize:horizontal、resize:vertical。但是想要实现 resize 的作用，元素的 overflow 不能设置为 visible。

#### ellipsis 文字溢出使用省略号表示

text-overflow: ellipsis 的意思是，溢出的文本使用省略号表示。但是这个属性有效果，需要设置 overflow:hidden

### overflow 与锚点技术

使用**锚点定位技术(url 后面的 # 跟着页面上元素的 id，点击 url 可以跳到该 id 所在的元素)**，需要容器可滚动，而且锚点元素(id 对应的元素) 在容器内。

 **锚点定位的本质是**：改变容器的滚动高度(scrollTop 高度的改变)

#### 锚点定位的作用

1. 快速定位，例如立刻定位到用户评论
2. 锚点定位与 overflow 选项卡技术：通过 a 标签的 href，定位到该 id 对应在页面的元素，具体可以查看 overflow 选项卡实现。

		<html>
		    <head>
		        <title>test</title>
		    </head>
		    <body>
		        <div class="box">
		            <div class="list" id="one">1</div>
		            <div class="list" id="two">2</div>
		            <div class="list" id="three">3</div>
		            <div class="list" id="four">4</div>
		        </div>
		        <div class="link">
		            <a class="click" href="#one">1</a>
		            <a class="click" href="#two">2</a>
		            <a class="click" href="#three">3</a>
		            <a class="click" href="#four">4</a>
		        </div>
		    </body>
		</html>


































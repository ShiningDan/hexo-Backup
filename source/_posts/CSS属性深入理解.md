---
title: CSS属性深入理解
date: 2016-11-09 22:07:03
categories: coding
tags:
  - CSS
---

本系列的是参考[慕课网 张鑫旭](http://www.imooc.com/u/197450/courses?sort=publish)老师的视频，仅供个人查阅使用，具体讲解推荐参考视频。

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


## CSS 深入理解之 line-height

### line-height 的定义

**行高，指的是两行文字基线之间的距离。**

### line-height 与行内框盒子模型

**所有内联元素的样式表现都与行内框盒子模型有关。**

一般行内框盒子模型由四部分组成，这四部分是依次包含关系：

1. 内容区域(content area)：是一种围绕文字看不见的盒子，内容区域的大小和 font-size 有关
2. 内联盒子(inline box)：不让内容成块显示，而是排成一行。虚线是匿名内联盒子，实线也属于内联盒子

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img41.PNG)

3. 行框盒子(line box)：，每一行就是一个行框盒子，每个行框盒子都是由一个一个内联盒子组成

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img42.PNG)

4. 包含盒子(containing box)：此盒子由一行一行的行框盒子组成

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img43.PNG)

### line-height 高度机制

**内联盒子的高度是 line-height 决定的**，不是由文字撑开的(不是 font-size 决定的)，**但是行高不是直接决定高度的属性，高度的表现不是行高，而是内容区域和行间距**，只不过正好 内同区域高度(content area) + 行间距(vertical spacing) = 行高(line-height)

**内容区域(content area)**的高度和字号(font-size)和字体(font-family)有关。

**行间距**：可大可小，甚至可以是负值。

### line-height 的各类属性值

line-height 有多种不同的属性值：

1. normal：默认属性值，由元素字体和浏览器决定。不同的浏览器和不同的字体，可以计算出其对应的像素高度。一般都要对字体大小进行重设，以保证各个浏览器显示的一致性
2. length：1.5 em/rem/px/pt
3. percent：150% 相对于该元素的 font-size 进行计算。

**一般相对于 font-size 计算行高的时候，业界普遍推荐是用 1.5，而不是 150% 或者 1.5em 来计算行高。**

一般 body 的全局属性使用：

	body{
		font: 14px/20px; "microsoft yahei";
	}

### line-height 与图片的表现

如果图片在行内，则默认情况下，图片的最下边缘和行的**基线**保持一致。所以图片和行的最下部有一定的高度差。

如何消除行内图片底部的间隙：

1. 图片块状化，此时就没有基线对其：

		img { display:blcok;}

2. 图片底线对其：

		img{ vertical-align:bottom;}

3. 行高足够小，导致基线和行的底部没有距离

		.box {line-height:0;}

### line-height 的实际应用

#### 行内图片垂直居中

设置行高大于图片高度，然后图片设置 vertical-align。

	.box { line-height: 300px;}
	.box img { vertical-align:middle;}

#### 多行文本垂直居中

把多行文字用容器封装起来，然后设置 display: inline-block (设置成与图片一样)，最后按照图片的方法进行设置。

**line-height 可以让单行文本垂直居中：**其实并没有垂直居中，和中线有一点距离。

## CSS 深入理解之 vertical-align

### vertical-align 的属性值

1. 线类：baseline/top/middle/bottom
2. 文本类：text-top/text-bpttom
3. 上标下标：sup/super
4. 数值百分比：2px/2em/20%

#### 属性百分比

**都支持负值**。

vertical-align 的百分比是相对于 line-height 来计算的。

### vertical-align 起作用的前提

**只能用于 inline 水平以及 table-cell 元素。**

1. inline水平: img/span/strong/em
2. inline-block: input/button
3. table-cell: td

**如果 vertical-align 没有起作用，有可能是没有设置 line-height，所以 vertical-align 的计算值与预想的不一样。**

### vertical-align 的 top/bottom/middle 理解

#### bottom

1. inline/inline-block元素：元素底部和整行的底部对齐
2. table-cell元素：单元格底 padding 边缘和表格行的底部对齐

#### top

1. inline/inline-block元素：元素底部和整行的顶部对齐
2. table-cell元素：单元格底 padding 边缘和表格行的顶部对齐

#### middle

1. inline/inline-block元素：元素的垂直中心点和父元素基线上 1/2 x 高度处对齐
2. table-cell元素：单元格底 padding 边缘和表格行居中对齐

### vertical-align 的 text-top/text-bottom 理解

使用 text-top/text-bottom，常常用于图片或表情和文字对齐的情况，好处是不会受到其他内联元素的影响(比如有比较大的内联元素，把行的高撑的特别高。此时使用 text-top/bottom 也可以和文字对齐)

1. text-top：盒子的顶部和父级 content area 的顶部对齐
2. text-bottom：盒子的底部和父级 content area 的底部对齐

content area 与 font-size 的大小相关。

### vertical-align 的 sub/super 理解

HTML 中有上标 sup 和下标 sub 标签。

1. super：提高盒子的基线到父级合适的上标基线位置
2. sub：提高盒子的基线到父级合适的下标基线位置

### vertical-align 的使用

除了定义的常用 top/bottom/middle ，还可以使用 vertical-align:-10px; 等精确值，可以达到比较好的对齐效果，而且没有兼容性差异。

#### 不定尺寸图片或多行文字的垂直居中

1. 需要居中的元素和 i 标签 inline-block 化
2. 在需要居中的元素后添加一个 0 宽度 100% 高度的辅助元素
3. 对居中元素和 i 元素使用 vertical-align:middle

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img44.PNG)

	<html>
	    <head>
	        <title>test</title>
	        <style type="text/css">
	            *{
	                margin:0;
	                padding:0;
	            }
	            .box{
	                height:200px;
	                width:500px;
	                margin: 0 auto;
	                background-color:#ccc;
	                text-align:center;
	            }
	            .box span{
	                display:inline-block;
	                vertical-align:middle;
	            }
	            .box i{
	                display:inline-block;
	                width:0;
	                height:100%;
	                vertical-align:middle;
	            }
	        </style>
	    </head>
	    <body>
	        <div class="box">
	            <span>第4行</span><i></i>
	        </div>
	    </body>
	</html>


## CSS 深入理解之 relative

### 限制性

1. 可以显示 position:absolute 的 top/left/bottom/right 位置
2. relative 的子元素的 z-index 的层级不能比 relative 元素的 z-index 层级要高
3. 普通元素的 overflow:hidden 不能显示 position:absolute 的显示，但是如果父元素设置了 relative，此时它的 overflow 属性可以限制 absolute 子元素的显示

**relative无法限制 fix 定位的显示**

### relative 和定位

**relative 的定位是相对于自身原有的位置。并且 relative 的元素还在原来的文档流中占据位置。**

无侵略行定位的使用场景：自定义拖拽效果，不会影响原有的文档流。

**如果同时设置 top/bottom 或者 left/right，则只有 top 和 left 起作用**，而不是类似于绝对定位的拉伸效果。

## CSS深入理解之z-index

**z-index指定了元素机器子元素的 z 顺序，通常较大 z-index 值的元素会覆盖较低的那个**

 z-index的可选值：

1. 数值(可以是负值)
2. auto

### z-index 与 CSS 的定位属性

**如果不考虑 CSS3，只有定位元素(position 的值不是 static(默认) 的元素)的 z-index 才有作用。**

如果没有指定 z-index 的值，则遵循后来居上的准则。如果指定了 z-index 值，则哪个大哪个就覆盖在上面。

如果定位元素 z-index 发生了嵌套，则遵循祖先原则，比较的是最外层元素的 z-index 值。如果祖先元素的 z-index 值是 auto，则子元素的 z-index 生效

### 理解 CSS 中的层叠上下文和层叠水平

层叠上下文中的每一个元素都有一个层叠水平，决定了同一个层叠上下文中元素在 z 轴的显示顺序。

 **层叠水平和 z-index 不是一个东西。普通元素也有层叠水平**

每个层叠上下文和兄弟元素独立：当进行层叠变化或渲染时候，只需要考虑后代元素。

**层叠顺序：**元素发生层叠时候的特定显示顺序

**重要：7阶层叠水平：**

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img45.PNG)

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img46.PNG)

### z-index 与层叠上下文的关系

1. 定位元素默认 z-index:auto 可以看成是 z-index:0
2. z-index 不为 auto 的定位元素会创建层叠上下文
3. z-index 层叠顺序的比较止步于父级层叠上下文

### 其他 CSS 属性与层叠上下文

还有其他的 CSS 属性可以创建层叠上下文，常用的有：

**这些 CSS 的层叠属性都是 z-index:auto**

1. opacity 的值不是1
2. transform 的值不是none
3. filter 的值不是none
4. position:fix声明
5. mix-blend-mode 的值不是 normal

**为了避免 z-index 导致互相的影响，设置 z-index 的元素要保证在最小化范围内。并且基本上不会遇见 z-index 超过 2 的情况出现。如果出现了，应该是某个地方的设置可以进行优化**

## CSS深入理解之margin

![](http://ofjjubwp5.bkt.clouddn.com/image/gif/%E7%9B%92%E6%A8%A1%E5%9E%8B.gif)

### 元素尺寸

1. 可视尺寸 (clientWidth )：border 之内的部分( width + padding + border)
2. 占据尺寸 ( outerWidth )：包含 margin 的部分( margin + border + padding + width)

### CSS margin 与百分比单位

**普通元素的 margin 百分比都是相对于容器(该元素所在的页面包裹元素)的宽度计算的**。

**绝对定位元素的 margin 百分比是相对于第一个定位祖先元素(relative/absolute/fixed)的宽度来计算的**

利用百分比单位可以实现根据容器宽度自适应的显示效果。

### CSS 的 margin 重叠

**margin重叠的意义**

1. 连续段落或列表之类，如果没有 margin 重叠，收尾间距会和其他兄弟标签 1:2 的关系，排版不自然
2. 遗落的 p 标签不能影响原来的阅读排版
3. web 中任意嵌套或直接放置的裸 div 不能影响原来的布局



#### 特性

1. margin 重叠发生在 block 水平元素(不考虑 float 和 absolute 元素)
2. 不考虑 writing-mode(文字的显示方向，比如说显示**古文**的时候，是从上到下显示文字)，重叠只发生在垂直方向上(margin-top/margin-bottom)

#### 重叠情况

1. 相邻兄弟元素 margin 发生重叠
2. 父级和第一个/最后一个元素发生重叠

### CSS 中的 margin:auto

有时候，就算没有设置 width 和 height，也会自动填充。但是 BFC 属性的元素会自动包裹住自己。

但是如果设置了 width 和 height，则元素的大小就会被指定，不会自动填充，此时 margin:auto 就是为了填充这个多出来的空间而设计的(**前提条件下是，元素的左右有多余的空间。如果元素是 inline水平的，左右没有多余的空间，则此时 margin:auto 不会起作用**)。

**如果元素不会自动填充，则使用 margin:auto 也不会自动分配剩余的空间**

#### 使用绝对定位进行强制填充

如果使用绝对定位，并且使用 left/right/top/bottom 强制拉伸元素，然后使用 height/width 强制设置元素的大小，则此时使用 margin:auto，又会根据自动填充的大小设置位置。**此方法经常使用在弹窗的定位中。**

显示效果如下：

![](http://ofjjubwp5.bkt.clouddn.com/image/png/img47.PNG)

代码如下：

	<html>
	    <head>
	        <title>test</title>
	        <style type="text/css">
	            .father{
	                height: 500px; width: 500px;
	                background-color: #ccc;
	                position: relative
	            }
	            .son{
	                position: absolute;
	                left: 0;right: 0;top: 0;bottom: 0;
	                height: 200px;width: 200px;
	                margin:auto;
	                background-color: #777;
	            }
	        </style>
	    </head>
	    <body>
	        <div class="father">
	            <div class="son">
	                
	            </div>
	        </div>
	    </body>
	</html>

### margin 无效的情况

1. inline 水平元素的垂直 margin 无效
2. 形成了 margin 重叠
3. display:table-cell/table-row 等声明的 margin 无效
4. 绝对定位 position:absolute 非定位方向的 margin 无效，但是存在，只是因为脱离了文档流，所以看不出效果

## CSS深入理解之padding

### padding 和元素的尺寸

**padding会影响元素的尺寸，会增加元素的尺寸。**

如果 width 值是 auto，或者 box-sizing:border-box，此时 padding 值如果不是太大，则不会影响元素的尺寸。

对于 inline 元素，水平 padding 会影响尺寸，垂直 padding 不会影响尺寸，但是会影响背景色，**因为背景色的部分包括 padding 的部分**。

### padding 负值和百分比值

**padding 不支持负值**

padding 的百分比是相对宽度计算的。










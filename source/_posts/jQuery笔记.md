---
title: jQuery笔记
date: 2016-12-07 21:55:18
categories: coding
tags:
  - jQuery
---
 jQuery笔记

## jQuery 介绍

**jQuery 的库分为两种类型，一种是生产版，一种是开发版**：生产版的名字是：`jqurey.min.js`，是经过工具压缩或者经过服务器开启 Gzip 压缩的，主要用于产品和项目。开发版的名字是：`jquery.js`，是完整无压缩版，用于测试，学习和开发。

## jQuery 对象

**为了不污染顶级变量，并且能够与其他库共存，jQuery 只创建一个名为 jQuery 的对象**。所有的函数和方法都在这个对象之下。在 jQuery 库中，`$` 就是  jQuery 的一个简写形式，**它们都是一个类型是 function 对象**，但是 `$` 符号可以随时交出控制权。

jQuery 对象是 jQuery 通过包装 DOM 对象后产生的对象，jQuery 对象中无法调用 DOM 对象的任何方法，同样 DOM 也无法使用 jQuery 的方法。

<!--more-->

### jQuery 和 DOM 对象的转换

**jQuery 对象是一个类似于数组的对象**，通过 [index] 和 get(index) 可以得到相应的 DOM 对象。

	let $d = $(document);  // jQuery 对象
	let d = $d[0] // DOM 对象
	let d = $d.get(0) // DOM 对象

使用 $() 把 DOM 对象包装起来，就可以获得一个 jQuery 对象。

	let d = document; // DOM 对象
	let $d = $(d); // jQuery 对象 

## jQuery 选择器

### jQuery 选择器的优势

1. 支持 CSS1 到 CSS3 选择器(废弃了不常用的 XPath，但是在引入相关的插件后，依然可以支持 XPath)
2. 在写JavaScript 的时候，经常需要判断选择得到的对象是否为空，如果没有得到元素，此时使用相关属性会报错。但是 jQuery 对象获取网页中不存在的元素也不会报错，不过可以可以通过 jQuery  对象的 length 长度来判断是否得到元素(length 为获得元素的个数)。

### jQuery 选择器的类型

**选择器都是用在 `$()` 中来定位需要的元素**

1. **基本选择器**(id, class, tag...)
2. **层次选择器**(子元素，后代元素)
3. **基本过滤选择器**(第一个元素，最后一个，奇数个元素，偶数个元素，索引值大于某个数，索引值小于某个数，去除某个元素)
4. **内容过滤选择器**(根据文本内容匹配，文本为空，含有子元素或文本，含有**选择器**所匹配元素的元素)
5. **属性过滤选择器**(含有这个属性，匹配属性值，不匹配属性值，属性值含有文本，属性值中的一个值匹配)
6. **子元素过滤器**(第几个子元素，奇数、偶数个子元素，第一个、最后一个子元素，唯一一个子元素)
7. **表单对象属性过滤器**(可用元素，不可用元素，checked元素，selected 元素)
8. **表单选择器**(选择 type 为不同类型的表单元素)

**上面的方法是通过选择器选择出合适的 jQuery 对象，如果要在 jQuery 对象数组中再选取与指定表达式匹配的元素数组，要使用 `filter()` 或者 `find()` **

* `filter(selector)`：从元素集合中**筛选**出匹配表达式的元素

		$("ul li").filter(":contains('text')") //从 li 中找到含有 text 文字的 li

* `find(selector)`：从元素中找到符合要求的子元素

		$("ul li").find("span") // 从 li 中找到其中的 span 标签

* `is(selector)`：根据选择器、元素或 jQuery 对象来检测匹配元素集合，如果这些元素中至少有一个元素匹配给定的参数，则返回true。

## jQuery 的 DOM 操作

**在 jQuery 中，很多的函数既可以用来获得数据，又可以用来设置数据。**

* `attr()`：获得和设置属性，还有`html()、text()、height()、width()、val()、vss()`等
* `$("<li>列表项</li>")`：获得 DOM 和创建 DOM 对象
* `html()`：相当于 innerHTML 修改。
* `val()`：获得和设置 value 值

### 删除节点

删除节点有 `remove()、detach()、empty()`，区别如下：

* `remove()`：将节点和后代节点都删除，返回值是指向被删除节点的引用，以后可以再使用这些节点，**但是如果再使用这些节点，节点上绑定的事件将失效**
* `detach()`：将节点和后代节点都删除，返回值是指向被删除节点的引用，以后可以再使用这些节点，**如果再使用这些节点，节点上绑定的事件不会失效**
* `empty()`：清空**元素里**所有的后代节点

### 复制节点

`clone()`函数可以复制节点，

**如果在 clone() 方法中传入参数 true，在复制元素的时候还可以复制元素所绑定的事件。**

### 包裹节点

* `wrap()`：匹配到的每一个元素都会被一个元素所包裹
* `wrapAll()`：匹配到的所有元素被一个元素所包裹，**如果包裹的元素间有其他元素，其他元素会被放在包裹元素后。**
* `wrapInner()`：每一个匹配的子内容(包括文本)，用结构化标记包裹起来。

### 获取 CSS 样式

**`css()`方法，无论是外部 CSS 导入，还是直接在 HTML 元素里， css() 方法都可以获得属性值。**

### 添加 class

添加 class 可以通过两个函数来实现，

* `addClass()`：**如果这个元素已经有了 class，调用这个函数不会覆盖掉原有的 class。**
* `attr()`：**调用这个函数会覆盖原有的 class**。

可以使用 `toggleClass("className")`来控制 class 上的重复切换。

**hasClass**也可以用来判断元素中是否包含某个 class。

### 查找父节点和祖先节点

* `parent()`：获取当前元素的父节点
* `parents()`：获得祖先节点，从父节点向上遍历

### 获取元素的宽度和高度

获得宽度和高度的方法，可以是 `css()`，也可以是 `width()、height()`，这两个函数的返回值有所差别：

* `css()`获得的高度与样式的设置有关，可能获得`auto`，也可以获得`10px`之类的字符串
* `height()、width()`获得的高度是元素在页面中的实际高度，和页面的设置无关，并且不带单位

### 获取元素的位置

* `offset()`：获取相对当前视窗的偏移
* `position()`：获取元素的 position 便宜
* `scrollTop、scrollLeft`：获取元素滚动条的距离

## jQuery 事件

### $(document).ready()

#### $(document).ready 与 window.onload 区别

**window.onload 在网页所有的元素(包括所有关联文件，图片等)都下载完成后才执行，$(document).ready 在 DOM 完全就绪的时候就可以被调用，此时不意味着所有的关联文件都已经下载完成。**

window.onload   等价与 $(window).load(function(){//...})

如果多次调用 $(document).ready() 传入行为函数，则**这些行为函数会多次执行**

	$(document).reay(function(){
		//...
	})

等价于：

	$(function(){
		//...
	})

等价于：

	$().ready(function(){
		//...
	})

### 事件绑定

jQuery 中一般使用 `bind()`进行事件绑定，但是也会有 `click()、mouseover()`等一些简写的事件。

使用 `unbind()`方法来移除事件。

* 如果没有参数，则移除所有的绑定事件
* 如果提供事件类型，则只删除该类型的绑定事件
* 如果第二个参数是处理函数，则该处理函数会被删除

`one()`：**只需要触发一次，然后就立即解除绑定**

### 合成事件

#### hover() 方法

**hover() 方法用于模拟光标悬停事件。**用来代替 mouseenter 和 mouseleave，**而不是替代 mouseover 和 mouseout**

**mouseenter 和 mouseover 的区别在于：**

* mouseenter：鼠标穿过被选元素的时候触发
* mouseover：鼠标穿过被选元素或其子元素

#### toggle() 方法

**`toggle()` 方法用于模拟鼠标连续单击事件**

toggle(fn1, fn2, fn3...)，每一次单击都会重复对这几个函数轮番调用。

在元素上调用 toggle() 函数还可以切换元素的可见状态。相当于 hide() & show()

### 事件冒泡和事件默认行为

* stopPropagation()：阻止事件冒泡
* preventDefault()：阻止事件默认行为
* **return false;**：同时阻止事件冒泡和事件默认行为

jQuery 不支持事件捕获。

## jQuery 动画

**show() 和 hide()**：通过设置 display:none 来实现隐藏元素。如果使用 hide() 方法，会记录原有 display 的值，在调用 show 方法的时候再设置回去。这两个函数可以设置参数为 slow、normal、fast、或者数值(毫秒)来设置动画。**显示效果是同时减少元素的高度，宽度和不透明度**

**fadeIn() 和 fadeOut()**：只改变元素的不透明度，直到元素完全消失(display:none)

**slideUp() 和 slideDown()**：只改变元素的高度，直到元素完全消失(display:none)

**fadeTo(time, opa)**：把元素的不透明度以渐进的方式调整到指定的值

**animate()**：设置自定义动画，可以设置累加累减动画，设置同时执行多个动画。

如果在 `animate()`外又调用了其他的函数，其他的函数不会被加入 animate 的队列中，则在开始执行动画的时候，其他的函数就会执行了。为了保证其他函数在动画之后执行，**可以使用 `animate()` 第三个参数，传入回调函数，确保执行顺序。**

用`stop([clearQueue],[gotoEnd])`方法可以停止动画，通过设置参数，可以选择是否删除之后的动画队列，是否将动画跳转到末状态

**当用户快速执行多个动画的时候，可能会出现动画积累，所以在添加动画到队列之前最好使用 is(":animated") 来判断元素是否处于动画状态。**

用 `delay()` 方法来对动画进行延迟操作。






















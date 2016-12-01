---
title: JavaScript笔记
date: 2016-11-29 22:38:50
categories: coding
tags:
  - JavaScript
---
JavaScript 笔记

## DOM 

### DOM 级别

**DOM0**是不存在的，DOM0 只是 DOM 历史中一个参照点而已，一般是指最初支持的 DHTML。

**DOM1**是 1998 年被推荐称为 W3C 的推荐标准，DOM1 被分为两个模块组成的：DOM Core 和 DOM HTML。

1. DOM Core：如何映射基于 XML 的文档结构，以便简化为文档中任意部分的访问和操作。DOM Core 有几个相关的方法，如：getElementById、getElementsByTagName、getAttribute、setAttribute，它们并不属于 JavaScript。
2. DOM HTML：在 DOM Core 的基础上加以扩展，增加了针对 HTML 的对象和方法。比如 onclick 属性，比如 src 属性等。


**DOM2**在 DOM1 的基础上扩展了鼠标和用户界面事件、范围、遍历迭代 DOM 文档的方法，也增加了对 CSS 的支持。DOM2 引入下列新模块：

1. DOM 视图(DOM View)：定义了跟踪不同文档(例如：应用了 CSS 之前和之后的文档)视图的接口
2. DOM 事件(DOM Event)：定义了事件和事件处理的接口
3. DOM 样式(DOM Style)：定义了基于 CSS 为元素应用样式的接口
4. DOM 遍历和范围(DOM Traversal and Range)：定义了遍历和操作文档树的接口

**DOM3**进一步扩展了 DOM，引入了以统一方式加载和保存文档的方法，也对 DOM Core 进行了扩展，开始支持 XML 1.0，设计 XPath 和 XML Base：

1. DOM 加载和保存(DOM Load and Save)
2. DOM 验证(DOM Validation)：DOM 验证

<!--more-->

### Node 类型

**cloneNode()**函数可以复制节点，后面的参数如果为 true 就执行深复制，深复制是复制节点和整个子节点树。

#### NodeList 对象

NodeList 对象不是某一个时间保存下来的快照，DOM 的变化能自动反应到 NodeList 对象上。


### Document 类型

**nodeName**是 "#document"

**nodeValue**是 null

**document.documentElement**指向的是`<html>`元素

**document.body**指向的是`<body>`元素

**查找元素的方法：**

1. getElementById
2. getElementByTaagName：返回一个 HTMLCollection(与 NodeList 相似)类型的元素，如果传入的参数是 `"*"`，可以获得所有的元素
3. getElementsByName：获得给定 name 特性的元素，一般用在获得单选按钮
4. getElementsByClassName

### Element 类型

Node类型的元素，即node.nodeType == 1 为 true 的元素，**nodeName**的值是 tagName，**nodeValue**的值是 null。

**tagName**的返回值都是大写的，如"DIV"

与特性相关的函数是：`getAttribute()、setAttribute()、removeAttribute()`，自定义的特性应该加上 `data-` 前缀以便验证。

**attributes**属性经常用于遍历元素的属性，返回值是 NamedNodeMap，与 NodeList 类似。

**classList**属性可以比 `className`属性更加方便与操作元素的类，classList 包含很有用的方法，比如：`add、remove、item(按照集合中的索引返回类值)、contains、toggle("className")(如果类名存在，则删除类名；如果类名不存在，则添加类名)`
**使用 document.createElement() 可以创建一个新元素，其他的示例没有这个函数**

**查找元素的方法：**

1. getElementsByTagName
2. getElementsByClassName

### DocumentFragment

如果我们要更新文档中的结构，如果要逐条修改结构，会导致浏览器反复渲染。为了避免这个问题，可以先创建一个文档片段(document fragment)，**将结构预先修改到文档片段中，最后一并添加到文档内**

	let fragment = document.createDocumentFragment();

## BOM

BOM 提供的相关功能如下：

1. 弹出浏览器窗口的功能：window.open(**关于弹出窗口有安全限制**)
2. 移动，缩放和关闭浏览器窗口的功能。**但是有的方法可能被浏览器禁用**
3. `window`对象：window 既是 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 Global 对象
3. `navigator`对象：提供浏览器的详细信息
4. `location`对象：提供浏览器所加载的文档的详细信息，比如 URI、端口相关信息。还提供导航功能
5. `screen`对象：提供用户显示器分辨率详细信息
6. `history`对象：保存用户的上网记录，进行跳转等
6. 对`cookie`的支持
7. 对`XMLHttpRequest`和`ActiveXObject`等自定义对象的支持

### 窗口位置和大小

1. **窗口位置**：窗口相对于屏幕左边和上边的位置。(screenLeft、screenTop、screenX、screenY)、
2. **窗口大小**：(outerWidth、outerHeight、innerWidth、innerHeight)

### 系统对话框

1. 确定按键：alter()
2. 确定、取消按键：confirm()
3. 提示用户输入文本：prompt()

## 元素标签

### script标签

在使用`<script>`标签添加 JavaScript 代码的时候，不要在代码的任何地方出现 "`</script>`"字符串，否则当浏览器遇到 "`</script>`"字符串的时候，会被会被解析成 `</script>`标签。不过可以使用转义字符 "\" 来解决这个问题。例如：

	<script type="text/javascript">
		function say(){
			alert("<\/script>");
			}
	</script>

#### script 标签属性

1. defer：表示脚本被延迟到整个文档完全被解析和显示之后再执行，只对外部脚本有效。**相当于告诉浏览器，立即下载，但是延迟执行**
2. async：表示痢疾下载脚本，但是不妨碍页面中的其他操作。**告诉浏览器，不让页面等待脚本的下载和执行，而是异步加载页面的其他内容**

## JavaScript 语法

### 操作符

#### 逻辑非

逻辑非操作符遵循下列规则：

1. 非空对象，则返回 false
2. 空字符串，则返回 true，否则为 false
3. 如果是 0 或者 NaN，则返回 true，否则返回 false
4. 如果是 null 或者 undefined，则返回 ture，否则返回 false

**使用场景**：

	var a;
	var b = !!a;

使用 `!!`在变量的前面，如果 a 是 undefined，`!a`是true，`!!a`是 false，这样方便后续判断。

#### 逻辑与

逻辑与`&&`是短路操作，如果第一个操作数能够决定结果，就不会对第二个操作数进行求值。

**非常重要！：逻辑与操作的返回值不一定是布尔类型！**有一个操作数不是布尔值，则返回值也不一定是布尔值


遵循的规则：

1. 如果第一个操作数是对象，则返回第二个操作数
2. 如果第二个操作数是对象，则只有在第一个操作数的求值结果是 true 的情况下，才返回第二个对象，否则返回第一个对象(**第一个操作数是 undefined、null、NaN、0、""**)
3. 如果两个操作数都是对象，则返回第二个操作数

#### 逻辑或

逻辑与`||`是短路操作，如果第一个操作数能够决定结果，就不会对第二个操作数进行求值。

**非常重要！：逻辑或操作的返回值不一定是布尔类型！**有一个操作数不是布尔值，则返回值也不一定是布尔值

遵循的规则：

1. 如果第一个操作数是对象，则返回第一个操作数
2. 如果第一个操作数为 false(**第一个操作数是 undefined、null、NaN、0、""**)，则返回第二个操作数
3. 如果两个操作数都是对象，则返回第一个操作数
4. 如果两个操作数都是 null、undefined、NaN，则返回 null、undefined、NaN

#### 相等操作符(`==`和`!=`)

**相等与不相等操作符，要先进行数据类型转换，再进行比较**

转换规则如下：

1. Boolean 类型的都转换为 Number 类型
2. 如果一个操作数是 String，另一个是 Number，则使用 Number() 将字符串转换为数值
3. 如果一个是 object，另一个不是，则调用对象的 valueOf() 方法，用得到的基本数据类型进行比较
4. null 和 undefined 是相等的，因为 undefined 派生自 null，他们也可以等于自身
5. null 和 undefined 不能转换为其他值
6. 如果一个操作数是 NaN，则它永远不和任何操作数相等，即使 `NaN == NaN` 的返回值也是false
7. 如果两个操作数是对象，则比较他们是不是指向同一个对象

#### 全等操作符(`===`和`!==`)

**全等与不全等操作符，不进行数据类型转换，直接进行比较**，所以不仅要值相等，数据的类型也要相等

null == undefined 的返回值是 true，但是**null === undefined 的值是 false，因为它们属于不同的数据类型** 

#### 乘法和除法操作符

如果参与计算的操作数不是数值，将先使用 Number() 转换类型函数将其转换为数值类型进行计算。

#### 加法操作符

如果操作数不是数值，而是字符串、对象、数值、布尔值、null、undefined，则会调用 String() 方法将其先转换为字符串，然后进行拼接。

#### 减法操作符

如果一个操作数是对象，则调用 valueOf() 方法来获取值，如果没有 valueOf 方法，则调用 String() 方法获取值。

如果有一个操作数是字符串、布尔值、null、undefined，则在后台调用 Number() 方法进行类型强制转换，如果转换结果是 NaN，则计算结果是 NaN。

#### 大小比较符(`<`，`>`)

1. 如果都是数值，则进行数值比较
2. 如果都是字符串，则分别比较字符串对应的字符编码值(**大写字母的字符编码全部小于小写字母的字符编码**)
3. 如果有一个操作数是数值，则将另一个操作数转换为数值进行比较
4. 如果有一个操作数是对象，则调用这个对象的 valueOf() 方法，如果没有，则调用 toString() 方法
5. 如果有一个操作数是布尔值，则转换为数值进行比较

### 变量

如果在创建变量的时候**省略 var 操作符，则会创建一个全局变量**，不过这样做会在局部作用域中定义全局变量，会变得很难维护

### 数据类型

ECMAScript 中有 **5 种简单的数据类型(基本数据类型)：Undefined、Null、Boolean、Number、String，和一种复杂数据模型：Object。**

使用 typeof 操作符来检测特定变量的数据类型，而不是使用 "==" 等操作符来检测数据类型，例如**undefined 派生自 null 值，null == undefined 返回值是 true**。

**基本数据类型在内存中占据固定大小的空间，所以被保存在栈内存中。引用类型的值是对象，所以被保存在堆内存中**

#### String 类型

**转义字符**是 String 类型包含的一些特殊的字符变量，只有在字符串里才有转义字符。

数值，布尔值，对象和字符串这四个数据类型有一个 toString() 方法，但是**null 和 undefined 值没有这个方法。**

#### Object 类型

**constractor**保存的是用于创建当前对象的函数，在一定程度上可以用来判断数据类型，但是不推荐。

**如果想要检测对象的类型，使用 instanceof 操作符**

##### Array

数组的 length 不是只读的，通过设置 length 的值，可以对数组的末尾删除项目或者添加新的项目，**如果数组的某一些项没有设置值，默认为 undefined。**

1. concat 方法可以创建一个当前数组的副本，并且将参数添加到这个副本的末尾，得到一个新的数组
2. 迭代方法：数组有 5 中迭代方法，**对数组的每一项都运行给定的函数**，它们是 every()、filter()、forEach()、map()、some()
3. 并归方法：数组有 2 中并归方法，**都会迭代数组的所有项，然后构建一个最终的返回值。**

##### RegExp

在正则表达式之中，需要对某些特殊的字符进行转义，如果要对正则表达式的内容转换为字符串的正则表达式，则需要进行双重转义

#### Function

函数名实际上是指向函数对象的一个指针，**注意JavaScript 的函数没有重载**

函数的定义有**两种方式，一种是函数声明，一种是函数表达式**

函数声明：

	function sum(arg1, arg2){
		return arg1 + arg2;
	}

函数表达式：

	var sum = function(arg1, arg2) {
		return arg1 + arg2;
	}

**函数声明和函数表达式的区别：**

函数声明的好处是，如果在函数声明之前调用了函数，代码会成功执行，因为 JavaScript 的解析器有一个函数声明提升的过程。但是函数表达式如果在使用之前没有定义，则会出现问题。

**因为函数也是一个对象，所以可以将函数作为参数进行传递，返回值也可以 return 一个函数。**

	function createSum(arg1, arg2){
		return function(arg1, arg2){
			return arg1 + arg2;
		}
	}

##### 函数的内部属性

`arguments`：保存函数的参数，arguments 还有一个 callee 的属性，该属性是一个指针，指向拥有这个 arguments 对象的函数

**this**：引用的是函数执行的环境对象，即调用了这个函数的对象。

**length**：函数想要接收的参数个数

**apply() 和 call()**：在固定的作用域中调用函数，实际上等于设置函数体内 this 的值。这两个函数的区别只不过是接收参数的方式不同。

##### 函数的闭包

闭包是指有权访问另一个函数作用域中的变量的函数，通常是在一个函数内部创建另一个函数。

**因为作用域链的原因。闭包只能获得包含函数中任何变量的最后一个值，闭包所保存的是整个变量对象，而不是某个特殊变量。**

	function test(){
		var result = new Array();
		for(var i=0;i<10;i++){
			result[i] = function(){
				return i;
			};
		}
		return result;
	}

这个时候所有输出的值都是 10，此时有多种方法来解决这个问题：

可以使用一个**立即执行的匿名函数**，这样可以获得每一个 i 的值。

	function test(){
		var result = new Array();
		for(var i=0;i<10;i++){
			result[i] = function(num){
				return function(){
					return num;
				}
			}(i);
		}
		return result;
	}

也可以使用 ES6 的 let 操作符创建变量 i ，使用 let 操作符创建的变量 i 具有块级作用域，它的值不会发生变化

	function test(){
		var result = new Array();
		for(let i=0;i<10;i++){
			result[i] = function(){
				return i;
			};
		}
		return result;
	}

##### 模仿块级作用域

`(function(){//这里是块级作用域})();`可以用来模仿块级作用域。

可以利用块级作用域定义静态私有变量。

#### 数值转换

**Number()**函数将任何数据类型转化为数值

1. 如果是 Boolean 值，true 会被转化成 1，false 会被转化称为0
2. 如果是数字值，只是简单的传入和返回
3. 如果是 null 值，返回 0
4. 如果是 undefined 值，返回 NaN
5. 如果是字符串，则分为多种情况：要是只包含数字，则会被转换成十进制的数值，如"123"；要是字符串是浮点的格式，则会转换为对应的浮点数，如"1.1"；如果字符串包含有效的十六进制的格式，则转换为相同大小的十进制数。如"0xf"；如果字符串为空，则转换为0，如果不为上述所有的格式，则转换成 NaN

isNaN()函数，**先将传进去的参数转换为数值，然后判断转换的结果是不是 NaN**，所有不能被转换为数值的参数使用 isNaN() 函数的返回值都是 true，而并非 NaN 才返回 true。

**parseInt() 和 parseFloat()**函数将字符串转化为数值

**String()**方法将任何数据类型的值转换为字符串：

1. 如果有 toString() 方法，则调用该方法并返回结果
2. 如果值是 null，则返回 "null"
3. 如果值是 undefined，则返回 "undefined"

**Boolean()**方法将任何数据类型转换为布尔类型

1. String：如果是空字符串，则为 false，否则为 true
2. Number：如果是 0 或者 NaN，则为 false，否则为 true(包括无穷大)
3. Object：如果是 null，则为 false，否则为 true
4. Undefined：被转换为 false

### 语句

#### for-in 语句

`for (property in expression) statement`

可以用来枚举对象的属性。

如果要迭代的对象是 null，则不执行循环体，如果是 undefined，则会报错。**推荐在执行 for-in 之前先确认对象是不是 null 或者 undefiend**

#### break & continue

break 用来跳出循环，不再执行循环。continue 跳出本次循环，但是会继续执行接下来的循环。

**如果在多层循环中 break 和 continue 只能跳出一层循环，如果想跳出多层循环，需要使用 label 和 continue 或者 break 合作**

#### switch

switch 语句中可以使用任何数据类型进行判断，**switch 语句在比较的时候使用的是全等操作符，所以需要考虑数据类型。**

### 函数

**ECMAScript 没有重载，如果定义了两个相同名字的函数，则后定义的函数生效。**

#### arguments

函数体内可以通过 arguments 获得所有参数

arguments 和函数参数的内存空间不同，但是他们的值会同步，对 arguments 的值进行修改，会导致实参的值也被修改。

#### 执行环境及作用域

全局的执行环境是 window 对象。每一个函数都会有自己的执行环境

**在 JavaScript 中，花括号所包裹的范围没有自己的作用域(执行环境)**

#### 内存管理

JavaScript 具有自动的内存管理机制，但是为了确保占用最少的内存，就需要对数据进行处理。**当数据不再使用的时候，最好将其设置为 null，这样内存处理机制就会将其释放**

### ECMAscript 内置对象

#### URI 编码方法

浏览器对于 URI 的理解中，有一些字符无法解释，所以，需要使用 encodeURI() 方法可以对 URI 进行编码，用特殊的 UTF-8 编码替换所有无效的字符串，让浏览器能够理解。与其对应的方法是 decodeURI()。**对于整个 URI ，使用 encodeURI() 进行编码，它不会对属于 URI 的特殊字符进行编码，比如冒号，正斜线，问号，井字号，这样会导致有一些需要进行编码的特殊字符被漏掉，所以使用 encodeURI() 的时候，最好先确定 URI 中其他的部分没有属于 URI 的特殊字符**

encodeURI() 主要用于整个 URI，而 encodeURIComponent() 主要用于 URI 中的某一段进行编码，与之对应的是 decodeURIComponent()。**因为 encodeURIComponent() 会对所有的非字母数字字符都进行编码，如果对整个 URI 使用这个方法，会将属于 URI 的特殊字符进行编码，导致浏览器解析发生错误。**

###  面向对象的程序设计

#### 数据属性

1. [[configurable]]：能否通过 delete 来删除属性从而重新定义，默认为 true
2. [[enumerable]]：能否通过 for-in 循环返回属性，默认为 true
3. [[writable]]：能否修改属性值，默认为 true
4. [[value]]：包含这个属性的数据值，读取值的时候，从这个位置读；写入属性的时候，把值写到这个位置。默认为 undefined

#### 访问器属性

访问器属性包含一对 getter 和 setter，当读取该属性时，会自动调用 getter 函数，这个函数负责返回有效的值。在写入数据属性时，会调用 setter 函数并传入新的值，这个函数决定如何处理数据：

1. [[configurable]]：能否通过 delete 来删除属性从而重新定义，默认为 true
2. [[enumerable]]：能否通过 for-in 循环返回属性，默认为 true
3. [[get]]：读取属性时调用的函数，默认是 undefined
4. [[set]]：写入属性时调用的函数，默认是 undefined

#### 原型链

**在构造函数中， prototype指向的是构造函数的原型，当通过构造函数生成一个对象以后，对象拥有的属性是 `__proto__` ，prototype 在原型链中起到的是一个辅助作用，用来将原型赋值给 `__proto__`，而`__proto__`代表了原型链的本质。**

**使用原型链的时候，如果原型中的属性使用了引用类型，而不是基本数据类型，则不同的实例会共享属性，一个实例修改了引用类型的属性，可能会影响到其他实例。**

为了向超类的构造函数中传递数据，要使用 call() 或者 apply() 方法来传递参数，设置运行环境。

























---
title: ' Sass 笔记'
date: 2016-12-29 22:30:32
categories: coding
tags:
  - Sass
---


#  Sass 笔记

本系列的是参考[慕课网 大漠](http://www.imooc.com/u/104044/courses?sort=publish)老师的视频，仅供个人查阅使用，具体讲解推荐参考视频。

## Sass 简介

**CSS 预处理器**定义了一种新的语言，其基本思想是，用一种专门的编程语言，为 CSS 增加了一些编程的特性

### Sass 和 SCSS 的区别

Sass 和 SCSS 其实是同一种东西，我们平时都称之为 Sass，两者之间不同之处有以下两点：

1. 文件扩展名不同，Sass 是以“.sass”后缀为扩展名，而 SCSS 是以“.scss”后缀为扩展名
2. 语法书写方式不同，**Sass 是以严格的缩进式语法规则来书写，不带大括号({})和分号(;)，而 SCSS 的语法书写和我们的 CSS 语法书写方式非常类似**。

**Sass 是基于 Ruby 环境运行的，如要要运行 Sass，首先要安装 Ruby。**

<!--more-->

## Sass 语法以及编译调试

**Sass 的语法格式和 SCSS 的语法格式，他们的功能都是一样的，不同的是其书写格式和文件扩展名不同**

### 命令编译

#### 单文件编译

	sass <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css

例如：
	
	sass test.scss:test.css

#### 多文件编译

针对文件夹的编译：

	sass sass/:css/

#### 监控自动编译

**重新保存了修改的文件，命令终端就能监测，并重新编译出文件**

	sass --watch <要编译的Sass文件路径>/style.scss:<要输出CSS文件路径>/style.css

**但是这种方法，如果文件夹或文件的名称里面有中文，会出现问题。**

### 输出样式风格

#### 嵌套输出方式 nested

在编译的时候带上参数“ --style nested”

	sass --watch test.scss:test.css --style nested

风格为：

	nav ul {
	  margin: 0;
	  padding: 0;
	  list-style: none; }
	nav li {
	  display: inline-block; }
	nav a {
	  display: block;
	  padding: 6px 12px;
	  text-decoration: none; }

#### 展开输出方式 expanded  

在编译的时候带上参数“ --style expanded”

	sass --watch test.scss:test.css --style expanded

风格为：

	nav ul {
	  margin: 0;
	  padding: 0;
	  list-style: none;
	}
	nav li {
	  display: inline-block;
	}
	nav a {
	  display: block;
	  padding: 6px 12px;
	  text-decoration: none;
	}

#### 紧凑输出方式 compact 

在编译的时候带上参数“ --style compact”

	sass --watch test.scss:test.css --style compact

风格为：

	nav ul { margin: 0; padding: 0; list-style: none; }
	nav li { display: inline-block; }
	nav a { display: block; padding: 6px 12px; text-decoration: none; }

#### 压缩输出方式 compressed

在编译的时候带上参数“ --style compressed”

	sass --watch test.scss:test.css --style compressed

风格为：

	nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}

### Sass 的调试

**可以使用流量器的 sourcemap 功能**

## Sass 基础

**普通变量：**普通变量定义以后可以在全局范围内使用

**默认变量：**默认变量一般是用来设置默认值，然后根据需求来覆盖的，**默认变量在组件化开发的时候非常有用。**

	$baseLineHeight: 2;
	$baseLineHeight: 1.5 !default;
	body{
	    line-height: $baseLineHeight; 
	}

编译后为：

	body{
	    line-height:2;
	}

### 局部变量与全局变量

**在选择器、函数、混合宏...的外面定义的变量为全局变量**

**定义在元素内部的变量，比如 $color:red; 是一个局部变量。**

例如：

	//SCSS
	$color: orange !default;//定义全局变量(在选择器、函数、混合宏...的外面定义的变量为全局变量)
	.block {
	  color: $color;//调用全局变量
	}
	em {
	  $color: red;//定义局部变量
	  a {
	    color: $color;//调用局部变量
	  }
	}
	span {
	  color: $color;//调用全局变量
	}

编译的结果为：

	//CSS
	.block {
	  color: orange;
	}
	em a {
	  color: red;
	}
	span {
	  color: orange;
	}

### 什么时候声明变量

创建变量只适用于感觉确有必要的情况下。不要为了某些骇客行为而声明新变量，这丝毫没有作用。只有满足所有下述标准时方可创建新变量：

1. 该值至少重复出现了两次；
2. 该值至少可能会被更新一次；
3. 该值所有的表现都与变量有关（非巧合）。

### 选择器嵌套

Sass 的嵌套分为三种：

1. 选择器嵌套
2. 属性嵌套
3. 伪类嵌套

#### 选择器嵌套的代词(`&`)

`&`：在选择器嵌套中代表当前所有祖先嵌套关系，如：

	nav {
	    a{
	        color:red;
	        header & {
	            color: green;
	        }
	    }
	}

编译结果是：

	nav a {
	   color: red;
	}
	
	header nav a {
	   color: green;
	}

#### 属性嵌套

CSS 有一些**属性前缀相同，只是后缀不一样**，比如：border-top/border-right，与这个类似的还有 margin、padding、font 等属性

	.box {
	    border-top: 1px solid red;
	    border-bottom: 1px solid green;
	}

编译出来是：

	.box {
	  border: {
	   top: 1px solid red;
	   bottom: 1px solid green;
	  }
	}

#### 伪类嵌套

其实伪类嵌套和属性嵌套非常类似，只不过他**需要借助`&`符号一起配合使用**

	.clearfix{
	&:before,
	&:after {
	    content:"";
	    display: table;
	  }
	&:after {
	    clear:both;
	    overflow: hidden;
	  }
	}

编译结果是：

	clearfix:before, .clearfix:after {
	  content: "";
	  display: table;
	}
	.clearfix:after {
	  clear: both;
	  overflow: hidden;
	}

选择器嵌套最大的问题是将使最终的代码难以阅读，我们应该尽可能**避免选择器嵌套**

### 混合宏

需要重复使用大段的样式时，使用变量就无法达到我们目的，此时需要使用宏

#### 声明混合宏

##### 不带参数的混合宏

	@mixin border-radius{
	    -webkit-border-radius: 5px;
	    border-radius: 5px;
	}

##### 带参数的混合宏

	@mixin border-radius($radius:5px){
	    -webkit-border-radius: $radius;
	    border-radius: $radius;
	}

##### 带有逻辑的混合宏
	
`...`：代表有多个参数

`@include`：代表调用声明好的混合宏

	@mixin box-shadow($shadow...) {
	  @if length($shadow) >= 1 {
	    @include prefixer(box-shadow, $shadow);
	  } @else{
	    $shadow:0 0 4px rgba(0,0,0,.3);
	    @include prefixer(box-shadow, $shadow);
	  }
	}

#### 调用混合宏

	@mixin border-radius {
	    -webkit-border-radius: 5px;
	    border-radius: 5px;
	}
	
	body{
	    @include border-radius;
	}

编译的结果是：

	body {
	  -webkit-border-radius: 5px;
	  border-radius: 5px; 
	}
























































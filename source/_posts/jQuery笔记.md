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













